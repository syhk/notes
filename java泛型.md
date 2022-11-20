# java泛型

# 泛型

泛型类派生子类：

- 
**子类也是泛型类，子类和父类的泛型类型要一致**

- 
**子类不是泛型类，父类要明确泛型的数据类型**


```java
public class<T> extends p<T>
    
public class extends  p<String>
```

泛型接口：

- 
**实现类不是泛型类，接口要明确数据类型**

- 
**实现类也是泛型类，实现类和接口的泛型类型要一致**


泛型方法：

```java
//泛型方法  返回值为泛型 T
    public <T> T getkey(ArrayList<T> list){
        return list.get(0);
    }

//这个不是泛型方法，只是使用了类的泛型
//如果泛型方法的泛型名和类的泛型名相同，在使用的时候不会有任何相互影响
public T getkey(){
    
}

public <T> 返回值  函数名（参数列表） {
    函数体；
}
```

### 类型通配符 ？

```java
public class sy003<T> {

    private T first;
    public T getFirst() {
        return first;
    }

    public void setFirst(T first) {
        this.first = first;
    }

    public static void main(String[] args) {
        Number i=50;//Number是对数字类型的抽象
        sy003<Number> sy=new sy003<>();
        sy.setFirst(i);
        showsy(sy);
        sy003<String> sy1=new sy003<>();
        sy1.setFirst("hello");
        showsy(sy1);

    }

    public static void showsy(sy003<?> sy){
        Object first=sy.getFirst();
        System.out.println(first);
    }
    
}
```

**类型通配符是类型实参，而不是类型形参。**

### 类型通配符的上限（extends）

```java
语法：类型通配符的上限
 类/接口  < ? extends 实参类型>
  //表示这个泛型的类型，只能是实参类型，或者实参类型的子类类型
```

```java
    public static void showsy(sy003<? extends Number> sy){
//        这里表示我们只能传 Number 类型的对象进来，或者是 Number 的子类型的对象
        Number first=sy.getFirst();
        System.out.println(first);
    }
```

```java
   public static void main(String[] args) {
        ArrayList<Animal> animals=new ArrayList<Animal>();
//        showAmimals(animals);//错误，因为它没有继承自Cat
        ArrayList<Cat> cats=new ArrayList<Cat>();
        showAmimals(cats);
        ArrayList<MinCat> mcat=new ArrayList<MinCat>();
        showAmimals(mcat);

    }

    public static void showAmimals(ArrayList<? extends Cat> list){
      for(int i = 0; i < list.size(); i++){
          Cat cat=list.get(i);
          System.out.println(cat);
      }
  
    }
```

### 类型通配符的下限 （super）

```java
语法：
类/接口 < ? super 实参类型 >
// 要求这个泛型的类型，只能是实参类型，或者实参类型的父类类型
```

```java
    public static void main(String[] args) {
        ArrayList<Animal> animals=new ArrayList<Animal>();
        showAmimals(animals);//正确，因为它是 Cat 的父类类型
        ArrayList<Cat> cats=new ArrayList<Cat>();
        showAmimals(cats); //也可以传递自身
        ArrayList<MinCat> mcat=new ArrayList<MinCat>();
//        showAmimals(mcat);//错误，因为它是  Cat  的子类
        
    }
//   要求集合只能是 Cat 的父类类型
    public static void showAmimals(ArrayList<? super Cat> list){
     for (Object o: list){
         System.out.println(o);
     }
    }
```

比较器定义自定义类型排序

```java
package sy01;

import java.util.Comparator;
import java.util.TreeSet;

public class sy006 {

    public static void main(String[] args) {
        TreeSet<Cat> tree1= new TreeSet<Cat>(new Comparator2());
//        传入自己的比较器，按自己定义的方式比较
        //如果不传入自己的比较器就会报错，因为它不知道要怎么排序
              tree1.add(new Cat("Tom",3));
              tree1.add(new Cat("Jerry2",2));
              tree1.add(new Cat("Jerry1",1));
              tree1.add(new Cat("Jerry4",4));
        for(Cat cat:tree1){
            System.out.println(cat.getName()+" "+cat.getAge());
        }


    }
}

//比较器
class Comparator1 implements Comparator<Animal> {
    @Override
    public int compare(Animal o1, Animal o2) {
        return o2.name.compareTo(o1.name);
    }
}

class Comparator2 implements Comparator<Cat> {
    @Override
    public int compare(Cat o1, Cat o2) {
        return o1.age-o2.age;
    }
}

class Comparator3 implements Comparator<MinCat> {
    @Override
    public int compare(MinCat o1, MinCat o2) {
        return o1.level-o2.level;
    }
}
```

## 类型擦除

在进入 JVM 之前，与泛型相关的信息会被擦除。

### 第一种无限制类型擦除：



直接把泛型擦除成  Object

```java
public class sy007<T > {
private T key;

    public T getKey() {
        return key;
    }

    public void setKey(T key) {
        this.key = key;
    }

}


===========================================
   public static void main(String[] args) {
        List<Integer> intlist=new ArrayList<Integer>();
        List<String> strlist=new ArrayList<String>();

        System.out.println(intlist.getClass().getSimpleName());
        System.out.println(strlist.getClass().getSimpleName());


        sy007<Integer> s = new sy007<Integer>();
//        利用反射获取 sy007 类的字节码文件的 Class 类对象
        Class<? extends sy007> c=s.getClass();
//        获取所有的成员变量
        Field[] d=c.getDeclaredFields();
//        遍历所有的成员变量  getSimpleName() 方法获取类型短名
        for(Field f:d){
            System.out.println(f.getName()+":"+f.getType().getSimpleName());
//      输出 key:Object
        }

.    }
```

### 第二种有限制类型擦除：



```java
public class sy007<T extends Number > {
private T key;

    public T getKey() {
        return key;
    }

    public void setKey(T key) {
        this.key = key;
    }

}

=================================================
    public static void main(String[] args) {
        List<Integer> intlist=new ArrayList<Integer>();
        List<String> strlist=new ArrayList<String>();

        System.out.println(intlist.getClass().getSimpleName());
        System.out.println(strlist.getClass().getSimpleName());
//        以上两个都输出 ArrayList


        sy007<Integer> s = new sy007<Integer>();
//        利用反射获取 sy007 类的字节码文件的 Class 类对象
        Class<? extends sy007> c=s.getClass();
//        获取所有的成员变量
        Field[] d=c.getDeclaredFields();
//        遍历所有的成员变量  getSimpleName() 方法获取类型短名
        for(Field f:d){
            System.out.println(f.getName()+":"+f.getType().getSimpleName());
//      输出 key:Number
        }

    }
```

### 第三种：擦除方法中类型定义的参数：



```java
public class sy007<T extends Number> {
private T key;

//这不是泛型方法，只是使用类的泛型参数
    public T getKey() {
        return key;
    }

    public void setKey(T key) {
        this.key = key;
    }

//    泛型方法
    public <T extends List> T show(T t){
     return t;
    }
    
}
====================================================
            System.out.println("=====================================");
//获取所有的方法
 Method[] methods = c.getDeclaredMethods();

for (Method method : methods) {
//打印方法名和方法的返回值类型
            System.out.println(method.getName()+":"+method.getReturnType().getSimpleName());
        }
//输出： getKey:Number
//setKey:void
//show:List
```

### 第四种：桥接方法



```java
//泛型接口
public interface Info<T> {
    T info(T t);
}

=======================================
   public class infoshix implements Info<Integer>{
    @Override
    public Integer info(Integer integer) {
        return integer;
    }

}


================================================
 
    public static void main(String[] args) throws NoSuchMethodException {
Class<infoshix> iclass=infoshix.class;
 Method[] infomethod=iclass.getDeclaredMethods();
  for(Method method:infomethod){
   System.out.println(method.getName()+": "+method.getReturnType().getSimpleName());
         }
//         输出：
//        info: Integer
//        info: Object
    }
```

# 混型

C++：

```c
template<typename T>
class TimeStamped: public T{
    long timeStamp;
public:
TimeStamped(){
    timeStamp = time(NULL);
}
long getStamp(){
    return timeStamp;
}
};

template<typename T>
class SerialNumbered : public T {
    long serialNumber;
    static long counter;
public:
    SerialNumbered() {
        serialNumber = counter++;
    }
    long getSerialNumber() {
        return serialNumber;
    }

};

template<class T> long SerialNumbered<T>::counter = 1;

class Basic {
    string value;
public:
    void set(string val){
        value = val;
    }
    string get(){
        return value;
    }
};

int main(){
TimeStamped<SerialNumbered<Basic>> mixin1,mixin2;
mixin1.set("test string 1");
mixin2.set("test string 2");
cout<<mixin1.get()<<"  "<<mixin1.getStamp()<<"  "<<mixin1.getSerialNumber()<<"\n";
cout<<mixin2.get()<<"  "<<mixin2.getStamp()<<"  "<<mixin2.getSerialNumber()<<"\n";
//    输出：
//    test string 1  1648906092  1
//    test string 2  1648906092  2
return 0;
}
```

输出了类型看看：

```c
template<typename T>
class TimeStamped: public T{
    long timeStamp;
public:
TimeStamped(){
    timeStamp = time(NULL);
}
long getStamp(){
    cout<<typeid(T).name()<<endl;
    return timeStamp;
}
};

template<typename T>
class SerialNumbered : public T {
    long serialNumber;
    static long counter;
public:
    SerialNumbered() {
        serialNumber = counter++;
    }
    long getSerialNumber() {
        cout<<typeid(T).name()<<endl;
        return serialNumber;
    }

};

template<class T> long SerialNumbered<T>::counter = 1;

class Basic {
    string value;
public:
    void set(string val){
        value = val;
    }
    string get(){
        return value;
    }
};

int main(){
TimeStamped<SerialNumbered<Basic>> mixin1,mixin2;
mixin1.set("test string 1");
mixin2.set("test string 2");
cout<<mixin1.get()<<"  "<<mixin1.getStamp()<<"  "<<mixin1.getSerialNumber()<<"\n";
cout<<mixin2.get()<<"  "<<mixin2.getStamp()<<"  "<<mixin2.getSerialNumber()<<"\n";
//    输出：
//    test string 1  1648906092  1
//    test string 2  1648906092  2
//    输出：
//    test string 1  14SerialNumberedI5BasicE
//    1648906500  5Basic
//    1
//    test string 2  14SerialNumberedI5BasicE
//    1648906500  5Basic
//    2
return 0;
}
```

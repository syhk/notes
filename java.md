**this 不可能是类，它是对象的引用，它只能是对象**

==总结：==Java 程序设计语言对对象采用的不是引用调用，实际上，对象引用是按值传递的。

**抽象类和接口的最主要区别：**

-  继承是体现  a  is b 的关系 
-  接口是体现  a has b 的关系 

多态实现的两种方法：**可以是父类引用指向子类对象，或者是接口的引用指向实现类的对象！**

以下无法实现交换数值！要实现交换可以使用数组或者封装了对象 ！

```java
  public static void main(String[] args) {
   Integer i = Integer.valueOf(10);
   Integer j = Integer.valueOf(20);
    warp(i,j);
    System.out.println("i="+i+"j="+j);
  }
  public static void warp(Integer i , Integer j){
      Integer temp =Integer.valueOf(0);
      temp = i;
      i = j;
      j = temp;
    System.out.println("调用了交换函数"+"i="+i+"j="+j);
  }
}
```

总结一下 Java 中方法参数的使用情况：

-  一个方法不能修改一个基本数据类型的参数（即数值型或布尔型）。 
-  一个方法可以改变一个对象参数的状态。 
-  一个方法不能让对象参数引用一个新的对象。 

==多态：父类引用指向子类实例==。

java的构造函数不能被重写，但是可以重载；

getClass（）获得对象的字节码对象！

toString 用字符串形式返回一个对象!

java中的大的集合分为三种：List、Set、Map

==java中有2种数据类型：基本数据类型和引用数据类型==

基本的数据类型只有8种：

java中 long 型数据在定义时要在数字后加字母 L，否则会被当做整形编译!



java中两种比较的方式：

**==**    如果是基本数据类型，比的是值，如果是引用数据类型，比的是地址值

**equals** 不能比较基本数据类型，如果是引用数据类型，比较的是内存地址。

-  ==**对象 instanceof 类名**==，表示对象是类名的实例，也可以是其子类的实例才返回 true，否则false 
-  ==**对象.getClass() == 类名.class**== 只有对象是这个类的实例才会返回true 

==**重点：**==

-  ==如果两个对象相等（equals），那么必须拥有相同的哈希码（hash code）== 
-  ==即使两个对象有相同的哈希值（hash code），他们也不一定相等== 

正确的 equals 方法要满点以下5个条件：

1.  自反性。对任意 x ， x.equals(x) 一定返回 true。 
2.  对称性。对任意 x和y , 如果 x.equals(y) 返回 true ,那么  y.equals(x) 也返回 true 
3.  传递性。对任意 x,y,z 如果有 x.equals(y) 返回 true ,y.equals(z) 返回 true ,则 x.equals(z) 一定返回 true 
4.  一致性。对任意x和y，如果对象中用于等价比较的信息没有改变，那么无论调用 x.equlas(y) 多少次，返回的结果应该一致，那么 true，那么 false 
5.  对任何不是 null 的 x, x.equals(null)  一定返回 false。 

默认 object.equals 比较对象的地址。重载 equals 必须要重写 hashCode()

#### java switch 两种写法

```java
switch (2){
            case 1 :
                System.out.println("1");
                break;
            case 2 :
                System.out.println("2");
                break;
            case 3 :
                System.out.println("3");
                break;
            default:
                System.out.println("其他！！");

        }

//2.
        switch (2){
            case 1 -> System.out.println("1");
            case 2 -> System.out.println("2");
            case 3 -> System.out.println("3");
            default -> System.out.println("其他！！！");
        }
```

第二种写法：就是可以不用写 break;（目前理解）

native关键字说明是一个原生函数；说明这个方法是C/C++语言实现的，并且编译成了DLL，由java调用！

> hashCode与equals之间的区别：


1.  hashcode如果相等的情况下，对象的值不一定相等 
2.  如果equals比较对象的内容相同，那么hashcode一定相等 
```java
  Integer aInt = 97;
  String aStr = "a";
   String c="a";
    System.out.println(aInt.hashCode());//输出 97
    System.out.println(aStr.hashCode());//输出 97
    System.out.println(c.hashCode());//输出 97
```
 
==**总结**==：重写equals方法的时候一定要重写hashcode方法
==**原因**==:上面例子，如果重写了equals方法没有重写hashcode方法的话，那么两个对象使用equals比较后相等，但是它们的hashcode值不相等！所以要重写hashcode方法保证equals比较后相等，它们的hashcode也一定相同！也就是遵循上面的定律 

```java
//比较常见的重写equals方法的方式
/*
1.用instanceof实现重写equals方法
2.用getClass实现重写equals方法
*/
    
    /*
    1.先判断是否是同一个类的对象
    2.强转成对应的类型
    3.然后再写要判断如何相等    
    */
 
    
package java高并发编程;

public class sy04 {

  public static void main(String[] args) {
dog d = new dog("a",20);
    dog d2 = new dog("a",20);
dog1 d1 = new dog1("a",20);
    dog1 d3 = new dog1("a",20);

    System.out.println(d1.equals(d3));
    System.out.println("d  hashcode为"+d.hashCode());
    System.out.println("d2  hashcode为"+d2.hashCode());
    System.out.println("==============================");
    System.out.println(d.equals(d2));
    System.out.println("d1  hashcode为"+d1.hashCode());
    System.out.println("d3  hashcode为"+d3.hashCode());
  }
}
class dog {
  public String name;
  public int age;
  public dog(String name, int age) {
    this.name = name;
    this.age = age;
  }
//  重写equals方法
  @Override
public  boolean equals(Object obj) {
    if(this ==obj) return true;
    if(obj == null) return false;
    if(obj ininstanceof dog) return false;

    dog d = (dog) obj;
    return this.name.equals(d.name) && this.age == d.age;

}
  }

class dog1 extends dog {
  public dog1(String name, int age) {
    super(name, age);
  }
  //    重写equals方法
  @Override
  public boolean equals(Object obj) {
    if(this ==obj) return true;
    if(obj == null) return false;
    if(this.getClass() != obj.getClass()) return false;
      dog1 d = (dog1) obj;
      return this.name.equals(d.name) && this.age == d.age;

  }
}

/*
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String aString = (String)anObject;
            if (coder() == aString.coder()) {
                return isLatin1() ? StringLatin1.equals(value, aString.value)
                                  : StringUTF16.equals(value, aString.value);
            }
        }
        return false;
    }
 */




//  public boolean equals(Object otherObject)
//  {
//
//    if (this == otherObject) return true;
//
//    if (otherObject == null) return false;
//
//
//    if (getClass() != otherObject.getClass()) return false;
//
//
//    Employee other = (Employee) otherObject;
//
//
//    return name.equals(other.name) && salary == other.salary && hireDay.equals(other.hireDay);
//  }
```



### instanceof  isInstance()  class getclass

```rust
 public staic void main(String[] args){
test(new A());
        System.out.println("================================================");
    test(new B());

    }
  static class A{
        private int age;
        private String name;
    }

    static class B extends A{
        private int ji;
    }

   public static void test(Object x){
       System.out.println(A.class.isInstance(x));//判断x是否是A类的实例
       System.out.println(x instanceof A);//判断x是否是A类的实例
       System.out.println(x.getClass() == A.class);//判断x的类型是否是A类
       System.out.println(x.getClass().equals(A.class));//判断x的类型是否是A类
   }

//输出结果：
/*
true
true
true
true
================================================
true
true
false
false
*/
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=Fuxsh&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：结论：**instanceof 和 isInstance() 指的是：你是这个类的吗？或者你是这个类的派生类吗？**

**如果使用 == equals （class ,getClass()） 指的是：没有考虑继承，它是这个类的确切类型吗？是或者不是？**

HashMap  与 HashTable 有那些区别：

| HashMap | HashTable |
| --- | --- |
| 线程不安全 | 线程安全 |
| 效率高 | 效率低 |
|  |  |


## java 基础知识点总结

> 逗号操作符：


```java
for (int i = 1 , j = i +10; i <5 ;  i++, j=i*2)
```

> java: goto const
这两个虽然是java的关键字，但是没有实现


```java
  public static void main(String[] args) {

        const int i = 0;
        goto  sy;
        sy:
        System.out.println("第一个开始地方");

        for(int i=0;i<10;i++){

            System.out.println("i="+i);
            if(i==5){
                continue sy;
            }

        }
        System.out.println("第一个结束地方");
    }
```

#### this

this **只能在方法内部使用，表示当前对象的引用。**

需要返回当前对象的引用时：

```java
return this;
```

this 调用本类构造函数：

```java
public class javagoto {

    private int age;
    private String name;

    public javagoto() {
        this(45,"sfd");
    }

    public javagoto(String name) {
        this.name = name;
    }

    public javagoto(int age){
        this.age = age;
        this.name = "";
    }

    public javagoto(int age, String name) {
        this(name);

    }
}
```

**this 调用构造函数只能放在第一行。**

**super 是调用父类的。**

当前继承关系时，它们两个不能用在一个方法中；

### 内部类允许继承多个非接口类型（类或者抽象类）。

```java
    class A{

    }

    class B{

    }
    
    class C{

    }
    
    class D{ //在这个内中继承上面三个类
     class a1 extends A{}
     class a2 extends B{}
     class a3 extends C{}
    }

    class E extends D{//这个类继承 D类，就相当于实现了多继承
 
    }
```

# 反射机制

反射是 java 被视为动态语言的关键；

-  **一个类在内存中只有一个class对象，所以反射获取的是同一个对象** 
-  **一个类被加载后，类的整个结构都被封装在class对象中；** 

```java
package Java反射;

//反射
public class sy001 {

  public static void main(String[] args)throws ClassNotFoundException {
//      通过反射获取类的class对象
    Class c1 =  Class.forName("Java反射.sy001");
    System.out.println(c1);
      Class c2 =  Class.forName("Java反射.sy001");
      Class c3 =  Class.forName("Java反射.sy001");
      Class c4 =  Class.forName("Java反射.sy001");

//      一个类在内存中只有一个class对象，所以反射获取的是同一个对象
//      一个类被加载后，类的整个结构都被封装在class对象中；
    System.out.println(c1.hashCode()); //输出331844619
      System.out.println(c2.hashCode());//输出331844619
      System.out.println(c3.hashCode());//输出331844619
      System.out.println(c4.hashCode());//输出331844619
  }
}
//实体类
class Person{
    private String name;
    private int age;
    private int id;
//无参构造
    public Person() {
    }
    //    有参构造
    public Person(String name, int age, int id) {
        this.name = name;
        this.age = age;
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }


}
```

得到 Class 类的几种方式：

```java
package Java反射;

//测试Class类的创建方式有哪些
public class sy001 {

  public static void main(String[] args)throws ClassNotFoundException {

 Person p = new Student();
    System.out.println("这个人是："+p.name);
//    方式一：通过对象获得
    Class c = p.getClass();
    System.out.println(c.hashCode());
//    方式二：通过路径获得
    Class c1 = Class.forName("Java反射.Student");
    System.out.println(c1.hashCode());
//通过类名获得
      Class<Student> c2 = Student.class;
    System.out.println(c2.hashCode());
//    方式四:基本内置类型的包装类都有一个Type属性
      Class c3 = Integer.TYPE; //int
    System.out.println(c3);

//    获得父类类型
      Class c4 = c.getSuperclass();
    System.out.println(c4);
  }
}
//实体类
class Person{
    public String name;
    public Person(){

    }
    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}
class Student extends Person{
    public String school;
    public Student(){
     this.name="学生";
    }

}
class Teacher extends Person{
    public Teacher() {
        this.name="老师";
    }
}
```

所有的Class对象

```java
package Java反射;

import java.lang.annotation.ElementType;

//所有类型的Class
public class sy001 {

  public static void main(String[] args)throws ClassNotFoundException {

 Class c1 = Object.class; //类

 Class c2 = String.class; //字符串

 Class c3=Comparable.class; //接口


 Class c4=String[].class;  //一维数组

 Class c5=int[][].class;  //二维数组

 Class c6=int[][][].class;

 Class c7=Override.class; //注解

 Class c8= ElementType.class; //枚举

 Class c9 = Integer.class; //引用数据类型
 Class c10 = void.class;  //void类型
 Class c11=Class.class;   //Class类型

    System.out.println(c1);
    System.out.println(c2);
    System.out.println(c3);
    System.out.println(c4);
    System.out.println(c5);
    System.out.println(c6);
    System.out.println(c7);
    System.out.println(c8);
    System.out.println(c9);
    System.out.println(c10);
    System.out.println(c11);
    //只要元素类型和维度一样，就是同一个Class
    int[] a = new int[10];
    int[] b = new int[100];
    System.out.println(a.getClass().hashCode());
    System.out.println(b.getClass().hashCode());
  }
}
```

java内存分析



类的初始化：

```java
package Java反射;

//测试类什么时候会初始化
public class sy001 {
    static {
        System.out.println("main类被加载 ");
    }
//类的主动引用一定会发生类的初始化
//类的被动引用不会发生类的初始化
    public static void main(String[] args)throws ClassNotFoundException {
    //   主动引用
    // Son son = new Son();

    //        反射也会产生主动引用
    // Class.forName("Java反射.Son");
    // 不会产生类的引用的方法
    //  System.out.println(Son.m);
    // Son[] array = new Son[10];//只有main类被加载 其他类不会被加载
    System.out.println(Son.M);//只有main类被加载 其他类不会被加载
        
  }
}
class Father{
    static{
        System.out.println("父类被加载");
    }

}
class Son extends Father{
    static{
        System.out.println("子类被加载");
    }
    static  int m = 100;
    static final int M = 1;
}
```

类加载器的作用：就是用来将字节码加装进内存

```java
//获取系统类的加载器
      ClassLoader systemClassLoader= ClassLoader.getSystemClassLoader();
    System.out.println(systemClassLoader);
//      获取系统类加载器的父类加载器-》扩展类加载器
      ClassLoader parentClassLoader=systemClassLoader.getParent();
    System.out.println(parentClassLoader);
//      获取扩展类加载器的父类加载器-》根加载器（C/C++）
      ClassLoader parent = parentClassLoader.getParent();
    System.out.println(parent);
    /*
    以上输出结果：
    jdk.internal.loader.ClassLoaders$AppClassLoader@1f89ab83
jdk.internal.loader.ClassLoaders$PlatformClassLoader@12843fce
null
     */
//      测试当前类是由哪个加载器加载的
      ClassLoader classLoader=sy001.class.getClassLoader();
    System.out.println(classLoader);
    //输出： jdk.internal.loader.ClassLoaders$AppClassLoader@1f89ab83
//测试JDK内置的类是谁加载的
    ClassLoader classLoader1=Object.class.getClassLoader();
    System.out.println(classLoader1);
```

获取类的运行时结构示例：

类1：

```java
public class sy004 {
private  int age = 100;
private  String name ;
private sy004(){
}
public sy004(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    private void show(){
        System.out.println("show sy004");
    }

}
```

类2：

```java
//获取创建运行时类的对象
public class sy001 {
  public static void main(String[] args)throws Exception {

  Class c1=  Class.forName("Java反射.sy004");
//  获取类的名称
  System.out.println(c1.getName()); //获得包名+类名
//  获取类的简单名称
  System.out.println(c1.getSimpleName());//获得类名

      sy004 s1 = new sy004("张三",20);
       Class c2 = s1.getClass();
    System.out.println(c2);
    System.out.println("===========================");
//    获得类的属性
    Field[] fields = c2.getFields();//只能获得public的属性

      fields = c2.getDeclaredFields();//可以获得所有的属性
      for(Field f:fields){
      System.out.println(f);
      }
//获得指定属性的值
      Field name = c1.getDeclaredField("name");
    System.out.println(name);

//    获得类的方法
Method[] methods=  c1.getMethods(); //获得本类及其父类的全部Public方法
for(Method m:methods){
    System.out.println("正常的"+m);
}

methods=c1.getDeclaredMethods();//获得本类的所有方法
      for(Method m:methods){
          System.out.println("getDeclaredMethods"+m);
      }

//      获得指定方法  第二个参数表示方法的参数所需要的类型，参数就是放类型，如果没参数就是null
      //是什么类型就是 类型.class eg:int.class String.class char.class 。。。
  Method m1 =    c1.getMethod("getName",null);
    System.out.println(m1);
    Method m2 =    c1.getMethod("setName",String.class);
    System.out.println(m2);

    System.out.println("===========================");
//    获得指定的构造器
  Constructor[] constructor=   c1.getConstructors();//获取Public的构造器
  for(Constructor c:constructor){
      System.out.println(c);
  }
  constructor = c1.getDeclaredConstructors();//获取所有的构造器
      for(Constructor c:constructor){
          System.out.println(c);
      }
//      获取指定的构造器
     Constructor c1_1 = c1.getConstructor(String.class,int.class);
    System.out.println(c1_1+"指定的构造器");
  }
}
```

---

动态创建对象执行方法：

```java
//动态的创建对象，通过反射
public class sy005 {

  public static void main(String[] args) throws Exception{
   Class c1 =   Class.forName("Java反射.sy004");

//   构造一个对象

//    sy004 s1 =  (sy004)c1.newInstance();//从1.9被弃用了
//      sy004 s1 = (sy004)c1.getConstructor().newInstance();//可以用这个
//    System.out.println(s1);

//    通过构造器创建对象
    //sy004 s2 = (sy004)c1.getConstructor(String.class,int.class).newInstance("张三",28);
    //等价于下面这两句
    Constructor constructor = c1.getConstructor(String.class,int.class);
   sy004 s2=(sy004) constructor.newInstance("张三",28);
    System.out.println(s2);
//    通过反射调用普通方法
    sy004 s3 = (sy004)c1.getConstructor().newInstance();
//  通过反射获取一个方法
    Method setName = c1.getDeclaredMethod("setName",String.class);
    setName.invoke(s3,"李四");
    System.out.println(s3.getName());

//    通过反射操作属性,不能直接操作私有属性，我们需要关闭程序 的安全检测，属性或者方法的setAccessible(true);
      sy004 s4 = (sy004)c1.getConstructor().newInstance();
      Field name = c1.getDeclaredField("name");
      name.setAccessible(true);
      name.set(s4,"王五");
      System.out.println(s4.getName());
  }
}
```

性能分析：

普通方式》 关闭安全检测的反射 》普通反射

```java
package Java反射;
import java.lang.reflect.Method;

//分析性能
public   class sy005 {
    //普通方式
    public static void test01() throws Exception {
        sy004 s1 = new sy004("张三");
        long startTime = System.currentTimeMillis();
        for(int i=0;i<1000000000;i++){
        s1.getName();
        }
long endTime = System.currentTimeMillis();
    System.out.println("普通方式："+(endTime-startTime)+"ms");
    }
//    反射方式调用
public static void test02() {
    sy004 s1 = new sy004("张三");
    Class c=s1.getClass();
    try {
        Method getName =  c.getDeclaredMethod("getName",null);
    } catch (NoSuchMethodException e) {
        e.printStackTrace();
    }
    long startTime = System.currentTimeMillis();
    for(int i=0;i<1000000000;i++){
        s1.getName();
    }
    long endTime = System.currentTimeMillis();
    System.out.println("反射方式："+(endTime-startTime)+"ms");

}
//    反射方式调用  关闭检测
public static void test03() throws Exception {
    sy004 s1 = new sy004("张三");
    Class c=s1.getClass();
    Method getName =  c.getDeclaredMethod("getName",null);
    getName.setAccessible(true);
    long startTime = System.currentTimeMillis();
    for(int i=0;i<1000000000;i++){
        s1.getName();
    }
    long endTime = System.currentTimeMillis();
    System.out.println("反射方式关闭检测："+(endTime-startTime)+"ms");

}
  public static void main(String[] args) {

//关闭检测比不关闭的性能更高更快
          try {
              test03();
          } catch (Exception e) {
              e.printStackTrace();
          }

      test02();
      try {
          test01();
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
}
```

反射操作泛型:

```java
import java.lang.reflect.Method;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.util.List;
import java.util.Map;

//通过反射获取泛型
public class sy006 {

public void test01(Map<String,String> map, List<sy004> list){
    System.out.println("test01");
}
  public static void main(String[] args) throws NoSuchMethodException {
   Method method = sy006.class.getMethod("test01", Map.class, List.class);

   Type[] genericParameterTypes = method.getGenericParameterTypes();

   for(Type type : genericParameterTypes) {
      System.out.println(type);
    if(type instanceof ParameterizedType) {
        Type[] actualTypeArguments = ((ParameterizedType) type).getActualTypeArguments();
        for (Type actualTypeArgument : actualTypeArguments) {
            System.out.println(actualTypeArgument);
        }
    }
   }
  }
}
```

## 线程和进程

> JAVA默认有2个线程，一个是 main(),一个是负责回收内存的 GC


对于java而言有3种方式开线程：Thread（类，重点继承重写run方法）、Runnable（接口、重要）、Callable（接口）

JAVA是无法直接开启线程的，它是调用底层C++的实现的。

> Thread


自定义线程类



```java
public class sy02  extends Thread{
//创建线程方式一：继承 Thread 类，重写 run 方法 , 调用 start 方法启动线程
//注意：线程开启不一定执行，由CPU调度决定
public void run(){
   for (int i=0;i<10;i++){
   System.out.println("线程方式一"+i);
   }
}
  public static void main(String[] args) {
//      主线程，main 线程
      sy02 sy=new sy02();
      sy.start(); //每次执行结果都不一样，看cpu的调度策略
      for(int i=0;i<1000;i++){
      System.out.println("主线程"+i);
      }
  }
}
```

> 实现Runnable接口


```java
//创建线程方式二 实现Runnable接口 重写run方法，执行线程需要丢入
// runnable接口实现类，调用start方法启动线程
public class sy02  implements Runnable{
    @Override
    public void run() {
     for (int i = 0; i < 100; i++) {
      System.out.println("我在看代码！");
              }
    }
  public static void main(String[] args) {
   sy02 sy = new sy02();
/*
  创建线程对象 ，通过线程对象来开启我们的线程，代理
   Thread th = new Thread(sy);
   th.start();
  */
      new Thread(sy).start();//等价于上面 那两行代码

   for (int i = 0; i < 200; i++) {
      System.out.println("我要学习多线程！");
   }
  }
}
```



```java
//多个线程同时操作同一个对象
public class sy02  implements Runnable{
//    票数
    int ticketnumber = 10;
    @Override
    public void run() {
//        Thread.currentThread().getName()获取当前线程的名字
      while(true){
          if(ticketnumber<=0){
              break;
          }
          try {
              Thread.sleep(200);//模拟延时
          } catch (InterruptedException e) {
              e.printStackTrace();
          }      System.out.println(Thread.currentThread().getName()+"拿到了"+ticketnumber--+"张票");
      }
    }
  public static void main(String[] args) {
        
sy02 s1 = new sy02();
//开启三个线程同时操作同一个对象
new Thread(s1,"小明").start();
new Thread(s1,"老师").start();
new Thread(s1,"黄牛").start();
  }
}
```

结果会存在0和-1说明了线程的不安全性

问题：多个线程同时操作一个对象，会发生安全问题

> 实现 Callable 接口（了解）


四个步骤：

```java
import java.util.concurrent.*;

//多个线程同时操作同一个对象
public class sy02  implements Callable<Boolean> {
   //要指定类型
    @Override
    public Boolean call() throws Exception {
    System.out.println(Thread.currentThread().getName()+"执行了");
        return true;
    }

  public static void main(String[] args) {
   sy02 s1 = new sy02();
   sy02 s2 = new sy02();
   sy02 s3 = new sy02();
//创建执行服务
      ExecutorService ser= Executors.newFixedThreadPool(3);

//   提交执行
      Future<Boolean> r1=ser.submit(s1);
      Future<Boolean> r2=ser.submit(s2);
      Future<Boolean> r3=ser.submit(s3);
//获取结果
      try {
          Boolean rs1=r1.get();
          Boolean rs2=r2.get();
          Boolean rs3=r3.get();
      } catch (InterruptedException e) {
          e.printStackTrace();
      } catch (ExecutionException e) {
          e.printStackTrace();
      }
//      关闭执行服务
      ser.shutdown();
  }
}
=============================================
    Thread.currentThread().getName()//获取线程名字
```

> ==**函数式接口**==：任何接口，如果只包含一个抽象方法， 那么它就是一个函数式接口


```java
package java高并发编程;
public  class sy02 {
  public static void main(String[] args) {

 myfunc funcc=new myclass();
 funcc.func(100);
// 局部内部类
    class  myclass implements myfunc{
          //如果这里不是static 就会报错
          public void func(int a){
              System.out.println("hello world11");
          }
      }
      myfunc funcc0=new myclass();
      funcc0.func(100);
//      匿名内部类
      myfunc funcc1=new myfunc(){
          public void func(int a){
              System.out.println("hello world22");
          }
      };
      funcc1.func(100);
//  Lambda表达式
      myfunc funcc2=(int a)->{  //这里的a是参数
          System.out.println("hello world33");
      };
       funcc2.func(100);
  }

    //定义一个函数式接口
    interface myfunc{//接口里面的方法默认是 public
      void func(int a);
    }

//    实现类
  static   class  myclass implements myfunc{
      //如果这里不是static 就会报错
        public void func(int a){
            System.out.println("hello world");
        }
    }
}
```

```java
package java高并发编程;
public  class sy02 {
  public static void main(String[] args) {
 myfunc funcc=new myclass();
 funcc.func(100);
// 局部内部类
    class  myclass implements myfunc{
          //如果这里不是static 就会报错
          public void func(int a){
              System.out.println("hello world11");
          }
      }
      myfunc funcc0=new myclass();
      funcc0.func(100);
//      匿名内部类
      myfunc funcc1=new myfunc(){
          public void func(int a){
              System.out.println("hello world22");
          }
      };
      funcc1.func(100);
//  Lambda表达式
      myfunc funcc4=(int a)->{  //这里的a是参数
          System.out.println("hello world33");
      };
       funcc4.func(100);
//简化参数类型
      myfunc funcc2=(a)->{  //这里的a是参数
          System.out.println("hello world44");
     };
      funcc2.func(100);
//简化括号
      myfunc funcc3=a->{  //这里的a是参数
          System.out.println("hello world55");
      };
      funcc3.func(100);
//      也可以简化大括号,如果执行语句有多选就不能简化
      myfunc funcc5=a->System.out.println("hello world66");

//总结 lambda表达式的使用
      //Lambda表达式只能有一行代码才能简化成为一行省略{}，如果有多行代码就不能简化
      //前提是接口必须是函数式接口
      //参数只有一个可以简化了括号（），如果参数有多个就不能省略括号
  }
    //定义一个函数式接口
    interface myfunc{//接口里面的方法默认是 public
      void func(int a);
    }
//    实现类
  static   class  myclass implements myfunc{
      //如果这里不是static 就会报错
        public void func(int a){
            System.out.println("hello world");
        }
    }
}
```





> 线程停止


```java
package java高并发编程;
//测试停止线程
//建议线程正常停止，利用次数，不建议死循环
//建议使用标志位，设置一个标志位
//不要使用STOP或者destroy方法，会导致线程死锁，JDK不推荐使用
class sy02 implements Runnable{
    @Override
    public void run() {
      int i = 0;
      while (flag){
      System.out.println( "run.....Thread"+i++);
      }
    }
//设置一个标志位
private boolean flag = true;
//    设置一个公开的方法停止线程，转换标志位
    public void stop(){
    this.flag = false;
}
  public static void main(String[] args) {
      sy02 s1 = new sy02();
      new Thread(s1).start();
      for(int i = 0;i<1000;i++){
      System.out.println("main....."+i);
          if(i == 900){
              s1.stop();
        System.out.println("该停止线程");
          }
      }
  }
}
```



> 线程休眠


```java
//模拟倒计时
public class sy02 {
    public static void main(String[] args) {
//        打印当前系统时间
 Date startTime  = new Date(System.currentTimeMillis());

 while(true){

     try {
         Thread.sleep(1000);
        System.out.println(new SimpleDateFormat("HH:mm:ss").format(startTime));
        startTime=new Date(System.currentTimeMillis());//更新当前时间
     } catch (InterruptedException e) {
         e.printStackTrace();
     }
 }
    }
    public static void tenDown() throws InterruptedException {
      int num = 10;
      while(true){
          Thread.sleep(1000);
      System.out.println(num--);
      if(num <=0 ){
          break;
      }
      }
    }
}
```

每个对象都有一个锁，sleep不会释放锁！

> 礼让


```java
package java高并发编程;
//测试礼让线程
//礼让不一定成功，要看cpu的调度策略
public class sy02{

  public static void main(String[] args) {
      myYieldmy my=new myYieldmy();

      new Thread(my,"A").start();
      new Thread(my,"B").start();
  }
}
class myYieldmy implements Runnable {
    @Override
    public void run() {
    System.out.println(Thread.currentThread().getName()+"线程开始执行");
    Thread.yield(); System.out.println(Thread.currentThread().getName()+"线程执行完毕");
    }
}
```

> join


```java
//join方法
public class sy02 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 500; i++) {
            System.out.println("线程vim来了"+i);
        }
    }
  public static void main(String[] args) throws InterruptedException {
   sy02 s1 = new sy02();
   Thread t1 = new Thread(s1);
   t1.start();

//   主线程
      for (int i = 0; i < 1000; i++) {
          if (i == 200) {
              t1.join();
          }
          System.out.println("主线程来了"+i);
      }
  }
}
```

> 观察线程状态


```java
  public static void main(String[] args) {
    Thread thread =
        new Thread(
            () -> {
              for (int i = 0; i < 100; i++) {
                try {
                  Thread.sleep(100);
                } catch (InterruptedException e) {
                  e.printStackTrace();
                }
                System.out.println("|\\\\\\\\\\\\\\\\\\\\\\");
              }
            });
    // 观察线程状态
    Thread.State state = thread.getState();
    System.out.println("线程状态：" + state);
    //    观察启动后的线程状态
    thread.start();
    state = thread.getState();
    System.out.println("线程状态：" + state);
    //    判断只要线程不终止就一直输出状态
    while (state != Thread.State.TERMINATED) {
      try {
        Thread.sleep(100);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      state = thread.getState(); // 更新线程状态
      System.out.println(state);
    }
}
}
```

> 线程的优先级


```java
  public static void main(String[] args) {
    //    主线程默认优先级  默认为5 
    System.out.println(Thread.currentThread().getName()+"-->"+Thread.currentThread().getPriority());
    mythream m1=new mythream();
    Thread t1=new Thread(m1,"线程1");
    Thread t2=new Thread(m1,"线程2");
    Thread t3=new Thread(m1,"线程3");
    Thread t4=new Thread(m1,"线程4");
    Thread t5=new Thread(m1,"线程5");
    Thread t6=new Thread(m1,"线程6");

//先设置优先级，再启动,一定要先设置再启动
      t1.start();

      t2.setPriority(1);
      t2.start();

      t3.setPriority(4);
      t3.start();

      t4.setPriority(Thread.MAX_PRIORITY);
      t4.start();

//      t5.setPriority(-1);
//      t5.start();
//范围在0-11 的范围内，超出或者为负数都会报错
//      t6.setPriority(11);
//      t6.start();
  }
}
class mythream implements Runnable{
    public void run() {
    System.out.println(Thread.currentThread().getName() + "优先级"+Thread.currentThread().getPriority());
    }
}
```

> 守护线程  daemon


线程分为**用户线程**和**守护线程**；

```java
//   测试守护线程
  public static void main(String[] args) {
   God god = new God();
   You you = new You();
   Thread t1 = new Thread(god);
   t1.setDaemon(true);//默认是false表示是用户线程，正常的线程都是用户线程；true变成守护线程
//   JVM不管守护线程执行完毕，只保护用户线程执行完毕
    t1.start();
    new Thread(you).start();//用户线程启动了
  }
}
class You implements Runnable {
  public void run() {
    for (int i = 0; i < 36500; i++) {
      System.out.println("你一生都开心的活着");
    }
    System.out.println("=============goddby! hello world!=============");
  }
}
class God implements Runnable {
  public void run() {

      while (true) {
      System.out.println("上帝保佑着你");
      }
  }
}
```

> 线程同步


不安全问题：多个线程同时操作一个对象，就会不安全；

> 线程不安全的集合: **ArrayList、LinkedList、HashSet、TreeSet、HashMap、TreeMap等都是线程不安全的**
线程安全的集合：**Vector、HashTable、Properties是线程安全的；**


> 集合不安全示例：


```java
import java.util.ArrayList;
import java.util.List;
public class sy02 {
  public static void main(String[] args) {

      List<String>  list = new ArrayList<String>();

      for(int i = 0; i < 10000; i++) {
   new Thread(()->{//创建10000个线程，把名字放入list
       list.add(Thread.currentThread().getName());
          }).start();
      }try {
          Thread.sleep(300);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
      System.out.println(list.size());
      //多次运行打印结果会不对
  }
}
```

> 同步方法


==**synchronized是一个同步方法。**==

每个对象都有一个锁，方法一旦执行，直到方法返回才释放锁！

缺陷：

使用synchronized同步方法后，买票就变得安全了！

==synchronized默认锁的this==

它方法和块两种：方法就是相当于一个修饰符！块就是一个区域

```java
package java高并发编程;

public class sy03 {

  public static void main(String[] args) {
    //
  BuyTicket b1 = new BuyTicket();

  new Thread(b1,"a").start();
  new Thread(b1,"b").start();
  new Thread(b1,"c").start();
  }
}

class BuyTicket implements Runnable {
private int ticketNums = 10;
boolean flag = true;
    @Override
    public void run() {
        while (flag){
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    private  synchronized void buy() throws InterruptedException {
        if(ticketNums <= 0 ){
            flag=false;
            return;
        }
        Thread.sleep(100);
    System.out.println(Thread.currentThread().getName()+"购买了第"+ticketNums--+"张票");
    }
}

============================================== 
 /*    
   锁代码块:锁的对象就是要变化的量
 synchronized (要锁的东西、对象或者属性){
          把操作所锁住东西的代码放在块里面             
      }
*/
```

线程安全的集合：CopyOnWriteArrayList、Vector、HashTbale、Stack(继承Vector)。

不安全的有：Hashmap  Arraylist LinkedList  HashSet TreeSet TreeMap

```java
package java高并发编程;
import java.util.concurrent.CopyOnWriteArrayList;
//测试JUC安全类型的集合
public class sy05 {
  public static void main(String[] args) {
  CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<String>();

  for (int i = 0; i < 10000; i++) {
      new Thread(()->{
          list.add(Thread.currentThread().getName());
      }).start();
  }
      try {
          Thread.sleep(3000);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
    System.out.println(list.size());//输出10000说明线程是安全的
  }
}
```

> 死锁


```java
//死锁：多个线程互相抱着对方需要的资源，然后一直僵持
public class sy05 {
    public static void main(String[] args) {
    Makeup g1 = new Makeup(0,"灰姑娘");
    Makeup g2 = new Makeup(1,"白雪公主");
    g1.start();
    g2.start();
    }
}
//    口红
    class Lipstick{
}
//    镜子
    class Mirror{
}
class Makeup extends Thread{
//    需要的资源只有一份，用static来保证只有一份
    static Lipstick lipstick=new Lipstick();
    static Mirror mirror=new Mirror();
    int chooice;
    String girlName;
    Makeup(int choice , String girlName){
        this.chooice=choice;
        this.girlName=girlName;
    }
 @Override
    public void run() {
     try {
         makeup();
     } catch (InterruptedException e) {
         e.printStackTrace();
     }
 }
//    化妆，互相持有对方的锁，锁是需要拿到对方的资源
    private void makeup() throws InterruptedException {
        if(chooice==0){
            synchronized (lipstick){//获取口红的锁
        System.out.println(this.girlName + "拿到了口红");
        Thread.sleep(1000);
        synchronized (mirror){//1S后，拿到镜子
            System.out.println(this.girlName + "拿到了镜子");
        }
            }
        } else {
            synchronized (mirror){//获取镜子的锁
                System.out.println(this.girlName + "拿到了镜子");
                Thread.sleep(2000);
                synchronized (lipstick){//2S后，拿到口红
                    System.out.println(this.girlName + "拿到了口红");
                }
            }
        }
    }
 }
```



> Lock锁


```java
import java.util.concurrent.locks.ReentrantLock;

public class sy05 {

    public static void main(String[] args) {

  TestLock t1 = new TestLock();

  new Thread(t1).start();
  new Thread(t1).start();
  new Thread(t1).start();
    }
}
class TestLock implements Runnable {
int ticketNums = 10;

//定义 Lock锁
    private final ReentrantLock lock = new ReentrantLock();
    
    @Override
    public void run() {
     while (true) {
         lock.lock(); //加锁
         if (ticketNums > 0) {
             try {
                 Thread.sleep(1000);
             } catch (InterruptedException e) {
                 e.printStackTrace();
             }finally{
                 lock.unlock(); //解锁
             }
        System.out.println(ticketNums--);
         }else {
             break;
         }
     }
    }
}
```

Lock只有代码块锁，而synchronized有代码块锁和方法锁。

尽量使用 Lock锁；



> 线程池  两个类： **ExecutorService** 和 Executors




```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

//测试线程池
public class sy05 {

    public static void main(String[] args) {
          //创建一个线程池,服务
//        newFixedThreadPool(); 参数为：线程池的大小
        ExecutorService service = Executors.newFixedThreadPool(5);
//        执行
        service.execute(new mythread());
        service.execute(new mythread());
        service.execute(new mythread());
        service.execute(new mythread());
//        关闭线程池连接
        service.shutdown();
    }
}
class mythread implements Runnable {
    public void run() {
      System.out.println(Thread.currentThread().getName()+"正在执行");

    }
}
```

### 总结

三种创建线程方式总结 ：

```java
public class sy05 {
    public static void main(String[] args) {
      new mythread1().start();

      new Thread(new mythread2()).start();
//第三种比较麻烦
        FutureTask<Integer> futureTask = new FutureTask<Integer>(new mythread3());
        new Thread(futureTask).start();
        try {
            Integer integer = futureTask.get();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
//创建线程方式总结
//方式一：继承Thread类
class mythread1 extends Thread{
    public void run(){
        System.out.println("mythread1 run");
    }
}

//方式二：实现Runnable接口
class mythread2 implements Runnable{
    public void run(){
        System.out.println("mythread2 run");
    }
}

//方式三：实现Callable接口
class mythread3 implements Callable<Integer> {
    public Integer call(){
        return 1000;
    }
}
```

# 注解和反射

Annotation

```java
public class sy001  extends Object{
//  @Override 重写的注解  
    @Override
    public String toString(){
        return "sy001";
    }
}

@SuppressWarnings("参数")  压制编译器警告,all代表消除所有警告
```

---

## 并发和并行

并发（多个线程操作一个资源）

并行（多个人一起行走）

**线程阻塞后是回去到就绪状态**

**wait 方法调用的对象，必须要用调用 wiat 方法的对象来唤醒！！！ notify notifyAll**

```java
  public static void main(String[] args) {
    //用程序的方式输入自己电脑CPU的核数
      System.out.println(Runtime.getRuntime().availableProcessors());
  }
```

---

并发编程的本质：**想充分利用CPU的资源**

> 线程有几个状态NEW（新生）、RUNNABLE（运行）、BLOCKED（阻塞）、WAITING（等待，死死等待 ）、TIMED_WAITING(超时等待)、TERMINATED（终止）


> wait和sleep的区别


1.  来自不同的类：wait:Object   sleep:Thread 
2.  关于锁的释放：wait会释放锁，sleep不会释放 
3.  使用范围不同：wait必须在同步代码块中 ；sleep可以在任何地方 

## Lock锁（重点）

synchronized本质：队列、锁

传统的是用synchronized，现在用Lock

> Lock 接口


公平锁：十分公平，可以先来后到

**非公平锁：十分不公平，可以插队（默认）**

```java
package java高并发编程;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
public class sy01 {
  public static void main(String[] args) {
 //线程是一个单独的资源类，没有任何附属的操作！
   Ticket t1= new Ticket();

//FunctionalInterface 函数式接口， jdk1.8新特性 Lambda表达式 (参数）->{代码}
      new Thread(()->{for (int i = 0; i < 40; i++) {t1.sale();}}).start();
      new Thread(()->{for (int i = 0; i < 40; i++) {t1.sale();}}).start();
      new Thread(()->{for (int i = 0; i < 40; i++) {t1.sale();}}).start();
  }
}
//Lock三部曲
//new ReentrantLock();
//lock.lock(); 加锁
//lock.unlock(); 解锁
class Ticket{
    private int number=30;
   Lock lock=new ReentrantLock();

    public void sale(){
        lock.lock();//加锁
        try{//业务代码写这里面
            if(number>0){
                System.out.println(Thread.currentThread().getName()+"卖出了"+(number)+"票，剩余"+number);
                number--;
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally{
            lock.unlock();//解锁
        }
    }
}
```

> ==Synchronized 和 Lock 的区别：==


1.  **Synchronized** 内置的java关键字，**Lock**是一个java类 
2.  **Synchronized**无法判断获取锁的状态，  Lock可以判断是否取到了锁 
3.  **Synchronized**会自动释放，  **Lock**必须要手动释放锁，如果不释放，就会死锁 
4.  **Synchronized** 会一直等， **Lock**不会一直等待 
5.  **Synchronized**可重入锁 ，不可以中断的， **Lock** 可重入锁，可以中断，可以判断锁，非公平（可以自己设置） 
6.  **Synchronized**适合少量的代码同步问题； **Lock**适合锁大量的同步代码 

> 锁是什么？如何判断锁的谁？


### 生产者和消费者问题

> 生产者和消费者问题 Synchronized版


```java
package java高并发编程;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
/*
线程之间的通信问题，生产者和消费者问题！
线程交替执行  A B 两个线程操作同一个变量  num = 0
A+     B-
 */
public class sy01 {

  public static void main(String[] args) {

 Data data = new Data();
      new Thread(()->{for (int i = 0; i < 10; i++) {data.increment();}}).start();
      new Thread(()->{for (int i = 0; i < 10; i++) {data.decrement();}}).start();
  }
}

//判断等待，业务，通知
class Data{//资源类
    private int number=0;
//    +1
public synchronized void increment(){
    if(number !=0){//等待
        try {
            this.wait();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    number++;
    System.out.println(Thread.currentThread().getName()+"=>"+number);
    this.notify();
//    通知其他线程我加完了
}
//    -1
public synchronized void decrement(){
    if (number ==0){//等待
        try {
            this.wait();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    number--;
    System.out.println(Thread.currentThread().getName()+"=>"+number);
    this.notify();
//    通知其他线程我减完了
}
}
```



if改为while循环就可以了！

```java
线程之间的通信问题，生产者和消费者问题！
线程交替执行  A B 两个线程操作同一个变量  num = 0
A+     B-
 */
public class sy01 {

  public static void main(String[] args) {

 Data data = new Data();
      new Thread(()->{for (int i = 0; i < 10; i++) {data.increment();}}).start();
      new Thread(()->{for (int i = 0; i < 10; i++) {data.increment();}}).start();
      new Thread(()->{for (int i = 0; i < 10; i++) {data.decrement();}}).start();
      new Thread(()->{for (int i = 0; i < 10; i++) {data.decrement();}}).start();
  }
}

//判断等待，业务，通知
class Data{//资源类
    private int number=0;
//    +1
public synchronized void increment(){
    while(number !=0){//等待
        try {
            this.wait();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    number++; System.out.println(Thread.currentThread().getName()+"=>"+number);
    this.notify();
//    通知其他线程我加完了
}
//    -1
public synchronized void decrement(){
    while (number ==0){//等待
        try {
            this.wait();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    number--; System.out.println(Thread.currentThread().getName()+"=>"+number);
    this.notify();
//    通知其他线程我减完了
}
}
```

> Lock 版生产者与消费者问题




```java
public class sy01 {

  public static void main(String[] args) {

 Data data = new Data();

      new Thread(()->{for (int i = 0; i < 10; i++) {data.increment();}}).start();
      new Thread(()->{for (int i = 0; i < 10; i++) {
          try {
              data.decrement();
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
      }}).start();
      new Thread(()->{for (int i = 0; i < 10; i++) {data.increment();}}).start();
      new Thread(()->{for (int i = 0; i < 10; i++) {
          try {
              data.decrement();
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
      }}).start();
  }
}
//判断等待，业务，通知
class Data{//资源类
    private int number=0;
    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();
//    condition.await();  等待
//    Condition.signalAll(); 唤醒
//    +1
public synchronized void increment(){
    lock.lock();
    while(number !=0){//等待
        try {
            condition.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }finally{
            lock.unlock();
        }
    }
    number++; System.out.println(Thread.currentThread().getName()+"=>"+number);
    condition.signalAll();//    通知其他线程我加完了
}
//    -1
//两种异常抛出方式一样
    public synchronized void decrement() throws InterruptedException {
        lock.lock();
        while (number ==0){//等待
         condition.await();
         lock.unlock();
    }
    number--; System.out.println(Thread.currentThread().getName()+"=>"+number);
    condition.signalAll();
    //    通知其他线程我减完了
}
}
```

> synchronized 锁的对象是方法的调用者，只有一个对象 的情况下，谁先拿到谁先执行


```java

public class sy007 {
    /*
     *  问题：标准情况下，两个线程先打印 发短信还是打电话？ 1/发送短信 2/打电话
        sendSms 延迟4秒，两个线程先打印，发短信是条电话?  1/发送短信 2/打电话
     * */ 
  public static void main(String[] args) {
      Phone phone=new Phone();
      new Thread(()->{
          phone.sendSms();
      },"A").start();
      new Thread(()->{
          phone.call();
      },"B").start();

  }
}
class  Phone{
    //synchronized 锁的对象是方法的调用者
    //两个方法用的是同一个锁，谁先拿到谁执行
    public synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);//睡眠4秒，JUC的睡眠方法
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发送短信");
    }

    public synchronized void call(){
    System.out.println("打电话");
    }

}
```

---

```java
package java高并发编程;
import java.util.concurrent.TimeUnit;
public class sy008 {

    /*=========================================
     *  增加了一个普通方法后执行顺序：
     *  1/hello
        2/发送短信
        3/打电话
        * ==========================================
        *两个对象，两个同步方法 : 1/打电话  2/发送短信
        *
     * */
    public static void main(String[] args) {
        //两个对象 两个调用者，两个不同的对象，有两把锁！所以是按时间来的！
        Phone1 phone=new Phone1();
        Phone1 phone1=new Phone1();
        new Thread(()->{
            phone.sendSms();
        },"A").start();
        new Thread(()->{
            phone1.call();
        },"B").start();


}}
 class  Phone1{
    //synchronized 锁的对象是方法的调用者
    //两个方法用的是同一个锁，谁先拿到谁执行
    public synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);//睡眠4秒，JUC的睡眠方法
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发送短信");
    }

    public synchronized void call(){
        System.out.println("打电话");
    }
    //这里没有锁，它不是一个同步方法，所以不受锁的影响
    public void hello(){
    System.out.println("hello");
    }
}
```

---

```java
package java高并发编程;
import java.util.concurrent.TimeUnit;
public class sy009 {
    /*=========================================
     * 增加两个静态的同步方法,只的一个对象   1/发送短信  2/打电话
     *增加至两个对象呢？                   1/发送短信  2/打电话
     */
    public static void main(String[] args) {
//两个对象的Class类模板只有一个，锁的是 Class
        Phone2 phone=new Phone2();
        Phone2 phone1=new Phone2();
        new Thread(()->{
            phone.sendSms();
        },"A").start();
        new Thread(()->{
            phone1.call();
        },"B").start();


    }}
//Phone2只有唯一一个 Class
class  Phone2{
    //synchronized 锁的对象是方法的调用者
    //两个方法用的是同一个锁，谁先拿到谁执行
    //静态方法 类一加载就有了！锁是class（是反射中的那一个只有一个）
    public static synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);//睡眠4秒，JUC的睡眠方法
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发送短信");
    }

    public static synchronized void call(){
        System.out.println("打电话");
    }

    //这里没有锁，它不是一个同步方法，所以不受锁的影响
    public void hello(){
        System.out.println("hello");
    }
}
```

---

```java
package java高并发编程;
import java.util.concurrent.TimeUnit;
public class sy10 {
  /*=========================================
   一个静态同步方法，一个普通同步方法，一个对象？  1/打电话   2/发送信息
   一个静态同步方法，一个普通同步方法，两个对象？  1/打电话   2/发送信息
  */
  public static void main(String[] args) {
    Phone3 phone = new Phone3();
    Phone3 phone2 = new Phone3();

    new Thread(
            () -> {
              phone.sendSms();
            },
            "A")
        .start();
    new Thread(
            () -> {
              phone2.call();
            },
            "B")
        .start();
  }
}
// Phone2只有唯一一个 Class
class Phone3 {
  // 静态同步方法     锁的是Class 类模板
  public static synchronized void sendSms() {
    try {
      TimeUnit.SECONDS.sleep(4); // 睡眠4秒，JUC的睡眠方法
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println("发送短信");
  }
  // 普通同步方法  锁的是调用者
  public synchronized void call() {
    System.out.println("打电话");
  }
}
```

> **小结：** new this 具体的一个手机
static    Class 锁的一个模板，类一加载的时候就有了


**CopyOnWriteArrayList**

```java
package java高并发编程;
import java.util.*;
import java.util.concurrent.CopyOnWriteArrayList;
public class sy11 {
  public static void main(String[] args) {
    List<String> list= Arrays.asList("1","2","3","4","5","6","7","8","9","10");
    list.forEach(System.out::println);//新版打印方式

      //并发下ArrayList是不安全的
      /*
      解决方案：1. List<String> list= new Vector<String>(); 不推荐
           2.      List<String> list3= Collections.synchronizedList(new ArrayList<String>());
3. List<String> list2=new CopyOnWriteArrayList<String>();//JUC下的安全集合

       */
    List<String> list1=new ArrayList<String>();
     //CopyOnWrite 写入时复制， COW 计算机程序领域的一种优化策略：
      //多个线程调用的时候，List，读取的时候固定的，写入（覆盖）
      //在写入的时候避免覆盖，造成数据问题；
      //读写分离
      //CopyOnWriteArrayList比vertor更加高效，更加安全
    List<String> list2=new CopyOnWriteArrayList<String>();//JUC下的安全集合
      
    for(int i = 0 ; i < 10 ; i++){
      new Thread(
              () -> {
                list1.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(list1);
              },String.valueOf(i)).start();
      ;
    }

  }
}
```

> set和map不安全


-  CopyOnWriteArraySet 
-  CopyOnWriteArrayMap 

# 线程理解

-  用户级线程 
-  内核级线程 



### java 注解

```java
/*
  public @interface  注解名{

    }
* @Target 指定注解针对的地方
* ElementType:
* ElementType.TYPE : 针对类 接口
* ElementType.FIELD : 针对成员变量
* ElementType.METHOD : 针对成员方法
* ElementType.PARAMETER : 针对方法参数
* ElementType.CONSTRUCTOR : 针对构造器
* ElementType.PACKAGE : 针对包
* ElementType.ANNOTATION_TYPE : 针对注解
*
* */
```

元注解

-  [@Target ](/Target ) :用于描述注解的使用范围  
-  [@Retention ](/Retention ) : 需要在什么级别保存这个注解信息，描述生命周期  
-  [@Document ](/Document ) ：说明这个注解被包含在 javadoc中（RUNTIME>CLASS>SOURCE)  
-  @Inherited: 说明子类可以继承父类中的该注解 

```java
//测试元注解
@myAnnotation
public class test {

    @myAnnotation
    public void test(){
        
    }
    
}

//可以指定多个作用域，中间用逗号分隔
@Target(value = {ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME) //runtime > class > source
@Documented //生成在 JAVAdoc中
@Inherited    
@interface  myAnnotation{

}
```

```java
public class test1{
@MyAnnotation(name = "syhk") //有默认值可以不用传递参数
public void test(){
}
}
@Target({ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation{
//    注解的参数： 参数类型 + 参数名 ();
    String name() default "";//如果没有默认值就必须要给注解赋值
    int id() default  -1; //如果默认值为 -1 ，代表不存在
    String[] schools()  default  {"云南大学"};
}
@Target({ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface  myAnnotation2{
    String value();//参数名只有 value 才可以省略其他都必须上参数名 =
}
```

```java
public class testHashmap {

    /**
     * @param args
     */
    public static void main(String args[]) {
        Map<Integer,User> uMap = new HashMap<>();
        
        uMap.put(1, new User("s1",18));
        uMap.put(2, new User("s2",19));
        uMap.put(3, new User("s3",20));
        uMap.put(4, new User("s4",21));
        uMap.put(5, new User("s5",22));

        // 遍历 HashMap
        for(var s: uMap.keySet()){
            System.out.println(uMap.get(s).getAge()+"  "+uMap.get(s).getName());
        }

        // 同时遍历 Key value
    
        for (Map.Entry<Integer, User> entry : uMap.entrySet()){
            System.out.println(entry.getKey()+"   "+entry.getValue().getName());
        }
    }
}
```

### java 比较接口

```java
package main;

import java.util.Comparator;

public class test2 implements Comparable,Comparator{
  
   private String name;
   private Integer age;
   
  public static void main(String[] args) {
//    比较接口
// 实现 Comparable 的类必须实现 compareTo(Object o) 方法，通过这个对象来比较自定义对象
//    大于返回 正整数 ， 小于返回负整数  ， 相等返回  0
//实现了 Comparable 接口的类的对象数组和集合，可能通过 sort() 方法进行自动排序 
    

    
    
  }

  @Override
  public int compareTo(Object o) {
    test2 test2=(test2) o;
    return this.age - test2.age;
  }

//  Comparator 接口有两个抽象方法：  compare()  equals()  
  @Override
  public int compare(Object o1, Object o2) {
    return ((test2)o1).age-((test2)o2).age;
  }
}
```

### java 集合 Iterator

```java
package main;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;import javax.imageio.event.IIOReadWarningListener;
public class Test1 {
 public static void main(String[] args) {
   ArrayList<Integer> arrayList=new ArrayList<>();
   arrayList.add(2);
   arrayList.add(3);
   arrayList.add(5);
   arrayList.add(6);
   Iterator<Integer> iterator = arrayList.iterator();

//   System.out.println(iterator.next());//2
   
//   使用 while 循环输出 
   while(iterator.hasNext()) {
     System.out.println(iterator.next());
     
//    删除元素
     iterator.remove();
/*
 * hasNext()  判断集合是否还有元素可遍历 
 * next()  返回迭代的下一个元素
 * remove 删除迭代器刚越过的元素
 * */
 
   }
   
   System.out.println(arrayList);//元素为空
   
//   map 和 set 遍历 
   Map<String,Integer> map=new HashMap<>();
   map.put("s1", 1);
   map.put("s2", 2);
   map.put("s3", 3);
   map.put("s4", 4);
   map.put("s5", 5);
   map.put("s6", 6);
   Iterator<Map.Entry<String,Integer>> iterator2=map.entrySet().iterator();
   
   while(iterator2.hasNext()) {
     Map.Entry<String, Integer> entry=iterator2.next();
     System.out.println(entry.getKey()+"  "+entry.getValue());
   }

//   遍历 set 集合
   Set<String> set=new HashSet();
   set.add("syhk1");
   set.add("syhk2");
   set.add("syhk3");
   Iterator<String> iterator3=set.iterator();
   while(iterator3.hasNext()) {
     System.out.println(iterator3.next());
   }
 }
}
```

#### 字符 编码
**java 中的 char 类型是点 2 个字节**

### 线程组
java 中用 ThreadGroup 来表示线程组，可以使用线程组对线程进行批量控制。
Thread 不能独立于 ThreadGroup 存在，执行 main() 方法线程组的名字是 main，如果  new Thread 时没有显式指定，默认将父线程线程组设置为自己的线程组
```java
public static void main(String args[]) throws InterruptedException {
    Thread testThread = new Thread(() -> {
        System.out.println("当前线程组的名字：");
        System.out.println(Thread.currentThread().getThreadGroup().getName());//main
        System.out.println("test线程的名字：");
        System.out.println(Thread.currentThread().getName());
    });
    testThread.start();

    System.out.println("main 线程的线程组名字:" + Thread.currentThread().getThreadGroup().getName());//main
    System.out.println("main线程名字:" + Thread.currentThread().getName());
    Thread.sleep(6000);

}
```

#### 线程优先级
```java
// 线程的优先级
// 优先级范围为 1 - 10 ，但并不是所有操作系统支持 10 级的划分
Thread a1 = new Thread();
System.out.println("默认优先级为：" + a1.getPriority());
// 5
Thread b1 = new Thread();
b1.setPriority(10);
System.out.println("设置的为:" + b1.getPriority());
// 10
```
**java中的优先级不是特别的可靠，设置优先级只是给操作系统一个建议，它不一定会采纳，真正的调用顺序，是由操作系统的线程调度算法决定的。**
测试：
```java
public class T1 implements Runnable {
    public static void main(String args[]) throws InterruptedException {

        IntStream.range(1, 10).forEach(i -> {
            Thread thread = new Thread(new T1());
            thread.setPriority(i);
            thread.start();
        });
        Thread.sleep(5000);
    }

    @Override
    public void run() {
        // TODO Auto-generated method stub

        System.out.println(String.format("当前执行的线程是%s:，优先级是： %d", Thread.currentThread().getName(),
                Thread.currentThread().getPriority()));
    }
}

```
某次输出 ：
```latex
当前执行的线程是Thread-4:，优先级是： 5
当前执行的线程是Thread-7:，优先级是： 8
当前执行的线程是Thread-6:，优先级是： 7
当前执行的线程是Thread-2:，优先级是： 3
当前执行的线程是Thread-3:，优先级是： 4
当前执行的线程是Thread-1:，优先级是： 2
当前执行的线程是Thread-5:，优先级是： 6
当前执行的线程是Thread-8:，优先级是： 9
当前执行的线程是Thread-0:，优先级是： 1
```
**线程的优先级大于所在线程组的最大优先级，那么该线程的优先级将会失效，取而代之的是线程组的最大优先级**
```java
        ThreadGroup threadGroup = new ThreadGroup("t1");
        threadGroup.setMaxPriority(6);
        Thread thread1 = new Thread(threadGroup, "thread");
        thread1.setPriority(10);
        System.out.println("线程组的优先级" + threadGroup.getMaxPriority());//6
        System.out.println("线程的优先级" + thread1.getPriority());//6
```
```java
   // 线程组常用方法:
        Thread.currentThread().getThreadGroup().getName();// 获取名字
        // 复制线程组
        // 获取当前线程组
        ThreadGroup threadGroup2 = new ThreadGroup("t2");
        Thread[] threads = new Thread[threadGroup.activeCount()];
        threadGroup2.enumerate(threads);
```

#### 并发编程的两个关键问题
线程间如何通信？
线程间如何同步？
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663933112738-1b8e815b-c118-4089-81ed-167988090c15.png#clientId=uf8785947-97c3-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u388f1c7b&name=image.png&originHeight=175&originWidth=521&originalType=url&ratio=1&rotation=0&showTitle=false&size=19929&status=done&style=none&taskId=u14cea11f-7482-4835-8a2b-c318f3e6044&title=)
java 有使用的是共享内存并发模型

运行时内存的划分：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663933191901-2a4fd5e5-5cf1-429d-af6a-1c3ae26030fa.png#clientId=uf8785947-97c3-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ub1ee90fe&name=image.png&originHeight=383&originWidth=552&originalType=url&ratio=1&rotation=0&showTitle=false&size=26420&status=done&style=none&taskId=u7cce3c90-8314-4ac3-b4e0-c6472397fe9&title=)
对每一个线程来说，栈是私有的，堆的共有的
#### volatile 关键字解析
volatile 可以保证可见性，但不保证原子性。
> 重排序：指令重排是减少中断的一种技术
> 一般分为三种：
> - 编译器优化重排
> - 指令并行重排
> - 内存系统重排
> 
指令重排可以保证串行语义一致，但是没有义务保证多线程间的语义也一致。

当一个变量定义为 volatile 之后 ，它将具备两种特性：

- 保证此变量对所有线程的可见性。（可见性是指：一条线程修改了这个变量的值，新值对于其他线程来说都是立刻得知的；而普通变量的值需要在线程间传递都要通过主内存回写来完成，也就是去回写后，其他线程再去读取才能知道新值）
- 禁止指令重排序优化
####  内存间交互操作：
java 内存规定了所有的变量都存储在主内存中。
每条线程都有自己的工作内存，线程的工作内存中保存了该线程用到的变量的主内存副本拷贝，线程对变量的所有操作都必须在工作内存中进行，不能直接读写主内存中的变量。
不同线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递均需要通过主内存来完成。
关系图：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664094977604-c3daec7b-58e5-41a4-bb0a-7d22e81b7c84.png#clientId=u7a157686-38b2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=316&id=ua8ae3496&name=image.png&originHeight=395&originWidth=867&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100226&status=done&style=none&taskId=u6ac95768-7378-4258-b5c6-ce5cd8cb35b&title=&width=693.6)

内存间的交互操作：
主内存和工作内存之间的交互协议：一个变量是如何拷贝到工作内存，如何从工作内存同步加主内存的实现细节；
java 内存模型定义了 8 种操作来完成，虚拟机实现时必须下面的第一种操作都是原子的：

- lock 锁定：作用于主内存的变量，它把一个变量标识为一条线程独占的状态
- unlock 解锁：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量可以被其他线程锁定
- read 读：作用于主内存的变量，它一个变量的值从主内存传输到线程的工作内存中，以便随后的 load 动作使用
- load 载入：作用于工作内存的变量，它把 read 操作从主内存中得到的变量值放入工作内存的变量副本中
- use 使用：作用于工作内存的变量，它把工作内存中一个变量的值传递给执行引擎，每当虚拟机遇到一个需要使用到变量的值的字节码指令时将会执行这个操作
- assign 赋值：作用于工作内存的变量，它把一个从执行引擎接收到的值赋给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令就会执行这个操作
- store 存储：作用于工作内存的变量，它把工作内存中一个变量的值传送到主内存中，以便随后的 write 操作使用
- wrte 写入：作用于主内存的变量，它把 store 操作从工作内存中得到的变量的值放入主内存的变量中

一个变量在从主内存复制到工作内存：需要顺序执行 read 和 load 操作；如果需要从工作内存回到主内存需要顺序执行 store 和 write 操作。只要求它们按顺序执行，并没有要求连续执行。
8种操作操作时要满足的规则 ：

- 不允许 read 和 load ， store 和 write 操作之一单独出现，即不允许一个变量从主内存读取了但工作内存不接受，或者从工作内存回写了但内存不接受
- 不允许一个线程丢弃它的最近的 assign 操作，即变量在工作内存中改变了之后必须把该变化同步回主内存
- 不允许一个线程无原因地把数据从线程的工作内存同步加主内存中
- 一个新的变量只能在主内存中“诞生”，不允许在工作内存中直接使用一个未被初始化的变量；也就是对一个变量实施 use ，store 操作之前，必须先执行 assign 和 load 操作
- 一个变量在同一个时刻只允许一条线程对其进行 lock 操作，但 lock 操作可以被同一条线程重复执行多次，多次执行 lock 后，只有执行相同次数的 unlock 操作，变量都会被解锁
- 对一个变量执行 Lock 操作，会清空工作内存中此变量的值，在执行引擎使用这个变量前，需要重新执行 load 或 assign 操作初始化变量的值
- 如果一个变量事先没有被 Lock 操作锁定，不允许对它执行 unlock 操作，也不允许去 unlock 一个被其他线程锁定住的变量
- 对一个变量执行 unlock 操作之前 ，必须先把些变量同步回主内存中

long和double的非原子性协定：
上 8 个操作 java　内存模型要求操作都是原子的。对 64 位的 long 和 double 特别定义了一要规定：允许虚拟机将没有被 volatile 修饰的 64 位数据的读写操作划分为两次 32 位的操作来进行。（目前虚拟机几乎都 64 位数据的读写操作作为原子的）

原子性：由 java 内存模型来直接保证的原子性变量操作包括 read,load,assign,use,store,write 
可见性：当一个线程修改了共享变量的值，其他线程能够立即得知这个修改
除了 volatile 之外 ， synchronized 和 final 也能实现可见性。
有序性：如果在本线程观察，所有的操作都是有序的（线程内表现为串行）；如果在一个线程中观察另一个线程，所有的操作都是无序的（指令重排序 和工作内存与主内存同步延迟）。

两个操作之间的关系不是先行发生及下面规则无法推导出来的话虚拟机就可以随着对它们进行随意重排序。
推导规则 ：

- 程序次序规则：一个线程内，按照程序代码顺序，书写在前面的操作先行发生于书写在后面的操作。（控制流顺序而不是程序代码顺序，需要考虑分支，循环等结构 ）
- 管程锁定规则 ：一个 unlock 操作先行发生于后面对同一个锁的 lock 操作。（这里必须强调是同一个锁，’后面‘是指时间上的先后顺序 ）
- volatile 变量规则 ：对一个 volatile 变量的写操作先行发生于后面对这个变量的读操作（’后面‘ 也是就时间的先后的顺序 ）
- 线程启动规则 ： Thread 对象的 start() 方法先行发生于些线程和每一个动作
- 线程终止规则 ：线程中的所有操作都先行发生于对此线程的终止检测（可通过 Thrad.join() 方法结束，Thread.isAlive() 的返回值等手段检测到线程已经终止运行）
- 线程中断规则 ：对线程的 interrupt() 方法的调用先行发生于被中断线程的代码检测到中断事件的发生（可通过 Thread,interrupt() 方法检测到是否有中断发生）
- 对象终结规则 ：一个对象的初始化完成（构造函数执行结束）先行发生于它的 finalize() 方法的开始
- 传递性：如果操作 A 先行发生于操作B ， 操作 B 先行发生于操作 C，可以得出操作 A 先行发生于操作 C 的结论

#### 线程的实现

- 使用内核线程实现 (KLT)：直接由操作系统支持的线程，由内核来完成线程的切换，
- 使用用户线程实现 （UT）：它的操作都在用户态中完成，不需要内核 的帮助
- 使用用户线程加轻量级进程混合实现：这种实现方式下它们都有，操作系统的轻量级进程作为用户线程和内核线程之间的桥梁

#### java 线程调度

- 协同式线程调度：线程的执行时间由线程本身来控制，工作执行完后主动通知系统切换到另外一个线程上。（线程执行时间不可控制 ）
- 抢占式线程调度：由系统来分配执行时间，切换也不由本身来决定

java 语言定义了 5 种线程状态：

- 新建
- 运行
- 无限期等待
- 限期等待
- 阻塞
- 结束 

线程状态转换关系：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664098655421-ec46db70-4a5f-4800-b78a-bbbd154677a5.png#clientId=u7a157686-38b2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=450&id=u62d7f3f5&name=image.png&originHeight=562&originWidth=905&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103324&status=done&style=none&taskId=u5afa4297-4b23-466b-bec0-c8f3ba5c16a&title=&width=724)


java 语言中各种操作共享的数据分为五类：

- 不可变
   - 共享的数据是一个基本类型，定义使用 final 修饰，它保证是不可变的
- 绝对线程安全
- 相对线程安全
- 线程兼容
- 线程对立




















 















Synchronized 三种应用场景：
```java
// 关键字在实例方法上，锁为当前实例
public synchronized void instanceLock() {
    // code
}

// 关键字在静态方法上，锁为当前Class对象
public static synchronized void classLock() {
    // code
    }

    // 关键字在代码块上，锁为括号里面的对象
    public void blockLock() {
        Object o = new Object();
        synchronized (o) {
            // code
        }
    }
```

在并发编程中 `i++`操作是线程不安全的，因为 `i++`操作不是原子操作。

#### java并发编程知识图谱
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663934436399-f3268dcd-3a9c-45fd-b76f-75c62b7bbecf.png#clientId=uf8785947-97c3-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ud2f14729&name=image.png&originHeight=14432&originWidth=2450&originalType=url&ratio=1&rotation=0&showTitle=false&size=4029047&status=done&style=none&taskId=u1afbe2d7-6518-49dd-91e5-7a7af6b4221&title=)


悲观锁：每次数据操作都加上锁，特别悲观 （写操作多）
乐观锁：无锁，通常使用 CAS 来保证线程执行的安全性 ，特别乐观（适用读多写少）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663935390091-1da34afd-2281-4abe-ae10-7b5512c4a1c5.png#clientId=uf8785947-97c3-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u8157d53c&name=image.png&originHeight=1462&originWidth=1898&originalType=url&ratio=1&rotation=0&showTitle=false&size=395241&status=done&style=none&taskId=u5f3a56a1-fd0a-475a-b6ac-32a27c3d0a0&title=)
### CAS 
> 比较并交换 Compare And Swap 

三个值：
V  ----->  要更新的变量 
E  ----->  预期值（就是上一次状态的旧值）
N -----> 新值
多个线程同时使用 CAS（是一种原子操作，不可再切分） 操作一个变量时，只有一个会胜出，并成功更新。
CAS 的问题：

1. ABA 问题：就是一个原来是 A ，变成了 B ， 又变回了  A，  CAS 是检查不出变化。
   1. 解决：在变量前面追加版本号或时间戳
2. 循环时间开销大：CAS 多与自旋结合，如果自旋CAS 长时间不成功，会占用大量的 CPU 资源
   1. 解决：让 JVM 支持处理器提供的 pause 指令，自旋失败时，可以睡眠一小段时间再继续自旋

### AQS AbstractQueuedSynchronizer 
抽象队列同步器


#### 自旋锁 VS 适应性自旋锁

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663935467949-530e5493-3236-464f-b5b6-73dec08030b3.png#clientId=uf8785947-97c3-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u5ad341ee&name=image.png&originHeight=1276&originWidth=1320&originalType=url&ratio=1&rotation=0&showTitle=false&size=198426&status=done&style=none&taskId=u75d85c73-3a8d-48b0-b05f-31b71419522&title=)


#### 公平锁和非公平锁
公平锁：多个线程按照申请锁的顺序来获取锁，线程直接进入队列中排队，队列中的第一个线程才能获得锁。
优点：等待锁的线程不会饿死
缺点：整体吞吐效率相对非公平锁要低等待队列中第一个线程以外的所有线程都会阻塞，CPU 唤醒阻塞线程的开销比非公平锁大
非公平锁是多个线程加锁时直接尝试获取锁 ，获取不到都会等待队列的队尾等待。但如果锁可用，直接获取到锁
优点：吞吐效率高，线程可以有机率不阻塞直接获得锁
缺点：等待队列中的线程可能会饿死
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663935580911-b08c74ff-8fa0-4bb8-b02a-91f999c345a2.png#clientId=uf8785947-97c3-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u3f49851c&name=image.png&originHeight=954&originWidth=1672&originalType=url&ratio=1&rotation=0&showTitle=false&size=208788&status=done&style=none&taskId=uafbbfb6c-ff91-4590-a839-ce39f873c50&title=)

#### 并发容器类
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663936240170-06e9bcec-6b7b-4b9d-a996-8c60b8f15d9c.png#clientId=uf8785947-97c3-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u5aa58322&name=image.png&originHeight=563&originWidth=1089&originalType=url&ratio=1&rotation=0&showTitle=false&size=82608&status=done&style=none&taskId=ua0499bf6-aa8a-45cd-aaa4-c4693d45f21&title=)
##### ConcurrentHashMap  （并发 MAP)
```java
public static void main(String args[]) {
    Map<String, Integer> map = new ConcurrentHashMap<>();
    // 与原有的 Put 方法不同，它如果插入的 key 相同，则不替换原来的 value 值
    map.putIfAbsent("s1", 1);
    map.putIfAbsent("s1", 2);
    Iterator<Map.Entry<String, Integer>> iterator = map.entrySet().iterator();
    while (iterator.hasNext()) {
        Map.Entry<String, Integer> entry = iterator.next();
        System.out.println(entry.getKey() + ":   " + entry.getValue());
    }
    // 输出 s1:1

    // remove 也与原有的不同：它增加了对 value 的判断，如果要删除的 key-value 不能与 Map 中原有的 key-value
    // 对应上，则不会删除该元素
    // replace(k,v,v) ：增加了对 value 值的判断，与原有 Map 中原有的 key-value 对应上，才进行替换操作
    // replace(k,v) 这个不会判断，如果 key 存在就直接替换

}
```


#### CopyOnWrite 容器 （写时复制）



### 线程池
线程池构造函数的 5 个参数：

- int corePoolSize : 线程池中核心线程数的最大值 ； 
   - 核心线程：线程池中有两类线程，核心线程和非核心线程，核心线程默认情况下会一起存在于线程池中，即使它什么也不做；蜚核心线程长时间闲置就会被销毁
- int maximumPoolSize 线程池中线程总数最大值 （核心线程+非核心线程）
- long keepAliveTime : 非核心线程闲置超时时长
   - 超时长就会被销毁， allowCoreThreadTimeOut(true) 这个设置 true 也会作用于核心线程
- TimeUnit unit  : keepAliveTime 的单位
- BlockingQueue workQueue： 阻塞队列，维护着等待执行的 Runnable 任务对象
   - 常用的有 LinkedBlockingQueue 链式阻塞队列，底层是链表；ArrayBlockingQueue 数组阻塞队列，底层是数组； SynchronousQueue 同步队列 ； DelayQueue 延迟队列；

两个非必须参数：

- ThreadFactory threadFactory ：创建线程的工厂，用于批量创建线程，不指定会有一个默认
- RejectedExecutionHandler handler 拒绝处理策略：有四种
   - ThreadPoolExecutor.AbortPolicy ：默认，丢弃任务并抛出异常
   - ThreadPoolExecutor.DiscardPolicy：丢弃新来的任务，不抛出异常
   - ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列头部的任务，重新尝试执行
   - ThreadPoolExecutor.CallerRunsPolicy ：由调用线程处理该任务

线程池本身有一个高度线程，这个线程用于管理整个线程池中的各种任务和事务。

####  四种常见的线程池

- newCachedThreadPool 

它不创建核心线程，只能创建非核心线程
```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```

- newFixedThreadPool

只能创建核心线程
```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

- newSingleThreadExecutor

有且仅有一个核心线程
```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
```

- newScheduledThreadPool

创建一个定长线程池，支持定时及周期性任务执行
```java
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
    return new ScheduledThreadPoolExecutor(corePoolSize);
}

//ScheduledThreadPoolExecutor():
public ScheduledThreadPoolExecutor(int corePoolSize) {
    super(corePoolSize, Integer.MAX_VALUE,
          DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,
          new DelayedWorkQueue());
}
```














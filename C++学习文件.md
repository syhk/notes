# C++学习文件

# C++ 关键字 explicit

它的作用是：禁止通过构造函数进行隐式转换。防止隐式转换。

示例：

```c
class A{
          private:
              int age;

          public:
            explicit  A(int age):age(age){
                  cout<<"调用了构造函数"<<endl;
              }
              ~A(){
                  cout<<"调用了析构函数！"<<endl;
              }
          };
```

```c
 A a=100;
//发生了隐式转换，以下两种不是，如果构造函数被explicit 修饰这样就会报错，被禁止隐式转换了
    A a1(200);
    A a2 = A(300);
```

# 重载++和- -

为了区分前置和后置，后置会有一个 int 参数。而前置没有

```c
Data& operator++(int){
    //后置++
}
Data& operator++(){
    //前置++
}
Data& operator--(int){
    //后置--
}
Data& operator--(){
    //前置--
}
```

# 重载<<

## 只能利用全局函数重载左移运算符

```c
class Solution {
public:
Solution(int age , string name):age(age),name(name){
cout<<"调用了析构函数！"<<endl;
}

friend ostream& operator<<(ostream& os, const Solution& s);



~Solution(){
    cout<<"调用了析构函数"<<endl;
}
private:
   int age ;
   string name;
};

ostream& operator<<(ostream& os, const Solution& s){
os<<"age="<<s.age<<"name="<<s.name<<endl;
return os;
}


// @lc code=end
int main(){

Solution a1(18,"沈扬");
//如果重载的函数，返回的是 void 后面就不能再继续输出，如果返回的是输出流就可以
//继续输出，链式编程思想
cout<<a1<<"fsd";//运算符重载调用方式一
operator<<(cout,a1);//调用方式二

}
```

==用友元函数实现==

# 重载括号（）

```c
class Solution {
public:
Solution(int age , string name):age(age),name(name){
cout<<"调用了析构函数！"<<endl;
}

friend ostream& operator<<(ostream& os, const Solution& s);

Solution& operator()(const Solution& b){
cout<<"调用了重载的括号函数"<<b.age<<b.name<<endl;
return *this;
}


~Solution(){
    cout<<"调用了析构函数"<<endl;
}
private:
   int age ;
   string name;
};

ostream& operator<<(ostream& os, const Solution& s){
os<<"age="<<s.age<<"name="<<s.name<<endl;
return os;
}


// @lc code=end
int main(){

Solution a1(18,"沈扬");
Solution a2(100,"张三");
Solution opp= (a2);//调用方式一
a1.operator(a2);//调用方式二
a1(a2);//调用方式三    
cout<<"opp="<<opp<<endl;
//如果重载的函数，返回的是 void 后面就不能再继续输出，如果返回的是输出流就可以
//继续输出，链式编程思想
cout<<a1<<"fsd"<<endl;//运算符重载调用方式一
operator<<(cout,a1);//调用方式二

}
```

# 匿名函数对象

```c
class myadd{
public:
int operator()(int num1 , int num2 ){
    return num1+num2;
}

};

int main(){
cout<<myadd()(100,100)<<endl;//输出200
//通过上面那样可以创建一个匿名函数对象，执行完那句话就立即释放。    
 return 0;   
}
```

类名加上括号，再加上一个括号里面是成员变量的值，就是匿名函数对象。

# 多态

==没有 virtual 看指针类型，有 virtual 看对象类型==

```c
class AA{
public:

virtual void show(){
  cout<<"调用了父类的虚函数！"<<endl;
}

int age;
string name;
};

class BB : public AA{
void show() {
cout<<"调用了子类虚函数！"<<endl;

}
private:
int height;   
};

// 程序的主函数
int main( )
{
AA* a= new BB;
a->show();
a=new AA;
a->show();
   return 0;
}
```

# override标识符

==这个说明必须放在虚函数的尾部，明确告诉编译器这个虚函数需要覆盖基类的虚函数==

```c
class AB{
public:
virtual void some_function(){
   cout<<"调用了父类的虚函数："<<endl;
}
};

class AC : public AB{
public:
virtual void some_function() override{ //这里修饰的函数也可以不用是虚函数，只要名称参数返回值和基类中的一样就行了
   cout<<"调用了子类的虚函数！"<<endl;
}
};
```

# 虚析构

==作用==：通过基类指针、引用释放子类 的所有空间。

示例：

```c
class AA{
public:

AA(){
   cout<<"调用了AA的构造函数"<<endl;
}
virtual ~AA() {
   cout<<"调用了AA的析构函数"<<endl;
 }
 void show() {
   cout<<"展示AA的信息"<<endl;
}
private:
int age;
string name;
};

class BB : public AA{
public:
BB() {
   cout<<"调用了BB的构造函数"<<endl;
}
 ~BB() {
   cout<<"调用了BB的析构函数"<<endl;
}
void show() {
   cout<<"展示了BB的信息"<<endl;
}
private:
int height;   
};

void test01(){
    
 AA* a= new BB();
a->show(); 
delete a;
//如果基类的中析构函数不是虚析构的话，这里只会调用父类的析构函数。不会删除子类的就会发生内存泄露。
//通过将父类析构函数调协为虚析构就可以解决这个问题。
}

// 程序的主函数
int main( )
{

test01();
return 0;
 
}
```

这个有个重要的知识点：

==子类析构调用完，系统会自动调用父类析构函数==。

示例：

```c
class AA{
public:

AA(){
   cout<<"调用了AA的构造函数"<<endl;
}
virtual ~AA() {
   cout<<"调用了AA的析构函数"<<endl;
 }
 void show() {
   cout<<"展示AA的信息"<<endl;
}
private:
int age;
string name;
};

class BB : public AA{
public:
BB() {
   cout<<"调用了BB的构造函数"<<endl;
}
 ~BB() {
   cout<<"调用了BB的析构函数"<<endl;
}
void show() {
   cout<<"展示了BB的信息"<<endl;
}
private:
int height;   
};

class CC:public BB{
public:
CC() {
   cout<<"调用了CC的构造函数"<<endl;
}
~CC(){
   cout<<"调用了CC的析构函数"<<endl;
}

};

void test01(){
/*  AA* a= new BB();
a->show(); */
  /* BB* b = new BB();
delete b; */
CC* c = new CC();
delete c;//通过调用它的析构函数，它的以上父类都会自动调用析构函数
}

// 程序的主函数
int main( )
{

test01();
return 0;
 
}
```

# 纯虚函数和抽象类

```c
格式： virtual void sleep() = 0;
```

拥有纯虚函数的类称为抽象类。

抽象类不允许实例化对象。

抽象类的子类，必须实现父类的纯虚函数，如果没有实现一个，那么这个子类也是抽象类。

# 纯虚析构

纯虚函数：不需要实现函数体

```c
class DD{
public:
//第一步变成纯虚函数
virtual ~DD()=0;

};
//第二步 必须实现 析构函数的函数体
DD::~DD(){};
//通过基类指针 释放子类对象时 先调用了子类析构 再父类析构（如果父类的析构不实现 就无法实现调用）
```

# 虚函数  纯虚函数  虚析构  纯虚析构 的区别

虚函数：只是 virtual 修饰有函数体（作用于成员函数）

==作用==：通过基类指针或者引用操作子类的方法。实现多态

```c
virtual void sleep(){
    //函数体
}
```

纯虚函数： virtual 修饰加上 0 ，没有函数体，所在的类为抽象类。

==作用==：为子类提供固定的流程和接口。

```c
virtual void sleep()=0;
```

虚析构： virtual  修饰类中的析构函数

==作用==：为了解决基类的指针指向派生类对象，并用基类的指针删除派生类对象

```c
virtual ~A(){
    //函数体
}
```

纯虚析构： virtual 修饰加 0 ，必须实现析构的函数体。

作用：用基类的指针删除派生类对象，同时提供固定的接口。

```c
class A{
    public:
    virtual ~A()=0;
}
A::~A(){
    //函数体 ， 纯虚析构必须像这样实现函数体，否则无法调用析构函数
}
```

# 不要返回局部对象的引用

# STL函数适配器

示例：

```c

/*
bind1st bind2nd
bind函数：函数适配器
bind函数中有占位符是用来表示传入的参数的，如果用指定值，不用占位符，代表原函数的调用不需要该参数
bind函数返回的是一个函数指针
*/

int sum(int x1, int x2) {
    return x1 + x2;
}

int main() {
    /*绑定普通函数*/
    int(*psum)(int, int) = sum;
    sum(10, 10);
    auto Fun = bind(sum, placeholders::_1, 10);//这里绑定了第二个参数，适配成了一个参数
    cout << Fun(10) << endl;
    cout << Fun(10, 100) << endl;//这里传入了第二个参数也没用，第二个参数是无效的，依然是使用上面绑定的第二个参数

 
    return 0;
}
```

```c
/*
bind1st bind2nd
bind函数：函数适配器
bind函数中有占位符是用来表示传入的参数的，如果用指定值，不用占位符，代表原函数的调用不需要该参数
bind函数返回的是一个函数指针
*/

int sum(int x1, int x2) {
    return x1 + x2;
}

void show(int x) {
    cout << x << endl;
}

int main() {
    /*绑定普通函数*/
    int(*psum)(int, int) = sum;
    sum(10, 10);
    auto Fun = bind(sum, placeholders::_1, 10);//这里绑定了第二个参数，适配成了一个参数
    cout << Fun(10) << endl;
    cout << Fun(10, 100) << endl;//这里传入了第二个参数也没用，第二个参数是无效的，依然是使用上面绑定的第二个参数

    vector<int> sorce = { 88,15,69,78,26,48 };
     
    int count = count_if(sorce.begin(), sorce.end(), [](int data) {
        return data >= 60;
    });
    cout << count << endl;
     
    auto FUI = bind(show, placeholders::_1, 100);
   

    return 0;
}
```

# STL迭代器

命名方式： 容器名::iterator 迭代器名;

```c
vector<int>::iterator iter;
iter=vecdata.begin();
auto iter=vecdata.begin();//这样更快更方便
```

常正向迭代器： cbegin(),cend();

反向迭代器： rbegin(),rend();

# 函数的默认参数（缺省参数）和占位参数

==函数的默认参数==：

```c
int add(int i , int x = 10, int y=10) {
    return i + x + y;
}

int main() {

    add(10);
    add(10,20,30);//也是正确的
}
```

上面的默认参数必须要从右向左依次有，不能出现空着。

==函数的占位参数==：

在声明函数的时候可以设置占位参数，占位参数只有参数类型声明，而没有参数名，一般情况下，在函数体内部是无法使用占位参数的。

```c
int add(int x, int y, int = 100) {
    return x + y;
}
//调用的时候可以传占位参数，传入是没用的，也可以不用传入占位参数的参数
int main() {
    cout << add(10, 10) << endl;
    cout << add(100, 100, 10) << endl;
}
```

**占位参数主要是用于，C后面重载和–的时候区分前置和后置。**

# 改变set 容器内部排序

```c
class A{
    public:
    bool  operator()(int x ,int y){
        return x>y;
    }
};

int main()
{
vector<int> vc;
set<int,A> vv;
vc.push_back(100);
vc.push_back(200);
vc.push_back(300);

vv.insert(10);
vv.insert(20);
vv.insert(30);
vv.insert(12);
vv.insert(-52);

for(int i : vv){
    cout<<i<<"   "<<endl;
}



cout<<endl;

set<int>::const_iterator it;
it=vv.lower_bound(20);
if(it == vv.cend()){
    cout<<"没有找到20的下限"<<endl;
}else{
    cout<<"找到 了"<<*(it)<<endl;
}
pair<set<int>::iterator,set<int>::iterator> p1;
p1=vv.equal_range(20);//这个找上下限同时找
if(p1.first == vv.end()){
    cout<<"没有找到"<<endl;
}else{
    cout<<*(p1.first)<<"    "<<*(p1.second)<<endl;
}


pair<int,int> pa1 = make_pair(10,10);


}
```

```c
set<int,A> vv;
set容器的第一个参数为类型，第二个参数为排序规则的类型，是一个类型，利用仿函数来实现改变排序规则
```

# set容器存放自定义数据的时候，必须指定排序规则

方法一：

```c
class B{
private:
    int age;
    string name;
    public:
    B(int age, const string &name) : age(age), name(name) {}
    bool operator<(const B &o2)const {
        return this->name<o2.name;
    }
     const int getAge() const {
        return age;
    }
    const string &getName() const {
        return name;
    }
};
int main()
{

set<B> vc;
vc.insert(B(18,"张5"));
vc.insert(B(19,"张6"));
vc.insert(B(20,"张1"));
vc.insert(B(22,"张2"));
vc.insert(B(23,"张3"));
vc.insert(B(24,"张4"));
for(B b : vc){
   cout<< b.getName()<<"  ";
   cout<<endl;
   cout<< b.getAge()<<"   ";
}



}
```

==也就是要重载小于符号==：模板：

```c
  bool operator<(const B &o2)const {
        return this->name<o2.name;
    }
```

## 方法二：指定set排序规则

```c
class B{
private:
    int age;
    string name;


    public:
  

    B(int age, const string &name) : age(age), name(name) {}
    //方法一 重载小于号
#if 0
    bool operator<(const B &o2)const {
        return this->name<o2.name;
    }
#endif
     const int getAge() const {

        return age;
    }
    const string &getName() const {
        return name;
    }

   
};

//方法二  指定排序规则
class A{
public:
    bool  operator()(const B& o1 , const B& o2){
       return o1.getAge()>o2.getAge();
    }
};


int main()
{

set<B,A> vc;
vc.insert(B(18,"张5"));
vc.insert(B(19,"张6"));
vc.insert(B(20,"张1"));
vc.insert(B(22,"张2"));
vc.insert(B(23,"张3"));
vc.insert(B(24,"张4"));
for(B b : vc){
   cout<< b.getName()<<"  ";
   cout<<endl;
   cout<< b.getAge()<<"   ";
}

}
```

重载（）

```c
class A{
public:
    bool  operator()(const B& o1 , const B& o2){
       return o1.getAge()>o2.getAge();
    }
};
```

==注意：==

```c
set<B,A> vc;
重载小括号以后，把它以类型方式传入set容器的第二个参数中支，指定排序规则
```

# 尽量不要使用 delete 释放 void*

```c
class A1{

public:
    A1(){
        cout<<"调用了构造函数"<<endl;
    }
    bool operator()(int x , int y){
        return x > y ;
    }
~A1(){
        cout<<"调用了析构函数"<<endl;
    }

};
int main(){
A1 *p= new A1();
void * a=p;
delete a;
}

/*没有调用析构函数
原因：delete 发现指向的类型为 void ，无法从void 中寻找相应的析构函数。*/
```

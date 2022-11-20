# 新 C++ 复习

⬜️⬜️⬜️⬜️⬜️⬜️⬜️⬜️⬜️⬜️ 0%

程序喵复习文件

[程序喵学习宝典2022.pdf](file/%E7%A8%8B%E5%BA%8F%E5%96%B5%E5%AD%A6%E4%B9%A0%E5%AE%9D%E5%85%B82022_IgyBByAKCJ.pdf)

#### 引用

引用就是变量的别名。

它和指针的区别：

指针在声明时可以**暂时不初始化**，即 pointer=nullptr，指针在生命周期内随时都可能是空指针，所以在每次使用时都要做检查，防止出现空指针异常问题。而引用不需要检查，因为引用永远都不会为空，它一定有本体，引用在创建的同时必须被初始化。

指针可以被重新赋值，而引用初始后就不会再变

#### 构造函数相关

```c
using  std::cout;
class TClass{
public:
    TClass() {
        cout<<"构造函数 1 "<<'\n';
    }
    TClass(int a):a(a){
        cout<<"构造函数2"<<'\n';
        data=new int[10];
    }
    TClass(const TClass &a) : a(a.a){
        cout<<"拷贝构造函数"<<'\n';
        data=new int[10];

        memcpy(data,a.data,10*sizeof(int ));
    }

    TClass(TClass &&a) :a(a.a),data(a.data){
        a.data= nullptr;
        cout<<"移动构造函数"<<'\n';
    }
    TClass &operator=(TClass &a1){
        a=a1.a;
        if(data) delete[] data;
        data = new int [10];
        memcpy(data,a1.data,10*sizeof (int ));
        cout << "赋值构造函数"<<"\n";
        return  *this;
    }
    TClass &operator=(TClass &&a1){
        a=a1.a;
        if(data) delete[] data;
        data =a1.data;
        a1.data = nullptr;
        cout<<"移动赋值函数"<<"\n";
        return *this;
    }
    ~TClass(){
        cout<<"析构函数"<<"\n";
        if(data) delete[] data;
    }

    void func() {
        cout<<"a"<<a<<"\n";
    }

private:
    int a;
    int *data;
};

namespace  n1 {
    void func() {
        std::cout<<"n1 func"<<'\n';
    }
}

namespace  n2 {
    void func() {
        std::cout<<"n2 func"<<'\n';
    }
}
```

main 函数：

```c
    TClass a(1);
    TClass b(a);
    TClass c(std::move(a));
    c=b;
    c=std::move(b);
    TClass d =c ;
    TClass e = std::move(d);
    a.func();
    b.func();
//================================================
    n1::func();
    n2::func();
```

#### 函数模板和类模板

```c
//模板分为函数模板和类模板
template <typename T>
T max(T a,T b){ //函数模板
    return a>b ? a : b;
}
//类模板
template <typename T>
struct Vec{
    Vec(T a):a_(a){}
    void func(){cout<<"func value"<<this->a_<<'\n';}
    T a_;
};
```

main：

```c
  cout<<max('A','B')<<'\n';
    Vec<int> vi(1);
    vi.func();
    Vec<float> vf(1.1f);
    vf.func();
```

```c
#ifndef __TEST2_H_
#define __TEST2_H_
#include <iostream>
#include <functional>
#include <thread>
#include <atomic>
#include <condition_variable>
#include <chrono>
#include <vector>
#include <exception>

using std::cout;
using std::endl;

struct Base
{
    Base()
    {
        std::cout << "construct" << '\n';
    }
    virtual ~Base()
    {
        std::cout << "base destruct" << '\n';
    }
    void FuncA() {}
    virtual void FuncB()
    {
        std::cout << "Base  FuncB \n";
    }
    int a;
    int b;
};

struct Derive1 : public Base
{
    Derive1() { std::cout << "Derive1 construct \n"; }
    ~Derive1() { std::cout << "Derive1 destruct \n"; }
    void FuncB() override { std::cout << "Derive1 FuncB \n"; }
};

struct Derive2 : public Base
{
    Derive2() { std::cout << "Derive2 construct \n"; }
    ~Derive2() { std::cout << "Derive2 destruct \n"; }
    void FuncB() override { std::cout << "Derive2 FuncB \n"; }
};

void test1()
{
    {
        Derive1 d1;
        d1.FuncB();
    }
    std::cout << "====================== \n";
    {
        Derive1 d1;
        d1.FuncB();

        Base &b1 = d1;
        b1.FuncB();
    }
    std::cout << "====================== \n";

    {
        Derive2 d2;
        d2.FuncB();

        Base &b2 = d2;
        b2.FuncB();
    }

    std::cout << "====================== \n";
    {
        Base b;
        b.FuncB();
    }
    std::cout << "====================== \n";

    {
        Base *b = new Derive1();
        b->FuncB();
        delete b;
    }
}
/*
构造函数不能是虚函数
基类析构函数最好是虚函数
尽量不要使用多继承
*/

struct A
{
    int a_;
};

void test2()
{
    A *a1 = new A;
    delete a1;
    A *a2 = new A[10];
    delete[] a2;
}
/*
new 和 delete 要配对使用
new[] 和 delete[] 要配对使用
malloc 和 free 要配对使用
*/

// c++ 类型转换
struct Base1
{
};
struct Derive3 : public Base1
{
};

void func()
{
    Base1 *base = new Derive3;
    // Derive3 *derive3 = dynamic_cast<Derive3*>(base);
}
void test3()
{
    float f = 1.0f;
    int a = static_cast<int>(f);
    std::cout << typeid(a).name() << '\n';
    std::cout << typeid(f).name() << "\n";
    const char *cc = "hello syhk";
    char *c = const_cast<char *>(cc);
    std::cout << typeid(c).name() << "\n";
    std::cout << typeid(cc).name() << "\n";
    // reinterpret_cast<>
}

void printNum(int i)
{
    std::cout << "print num" << i << " \n";
}
typedef void (*FuncPtr)(int i);
void test4()
{
    auto x = 0;
    decltype(x) y; // y 是 int 类型
    // functional 用于封装一个函数类似于函数指针
    FuncPtr ptr = printNum;
    ptr(2);
    void (*ptr2)(int) = printNum;
    ptr2(3);
    std::function<void(int)> func = printNum;
    func(100);

    // Lambda 表达式
    auto func1 = [](int a) -> int
    { return a + 1; };
    int b = func1(2);
    std::cout << "lambda 的计算结果为： " << b << '\n';
    // 分为值捕获和引用捕获
    int a = 0;
    auto func2 = [=]()
    { return a; }; //捕获 a
    std::cout << func2() << '\n';
    auto func3 = [=]() mutable
    { return a++; }; //需要改变的话需要加这个 mutable
    std::cout << func3() << '\n';

    // 线程
    std::thread t(printNum, 100);
    if (t.joinable())
    {
        t.join(); // t.detach()
    }
    // 线程一定要 join 或者  detach
}

enum AColor
{
    KRed,
    KGreen,
    KBlue
};
enum BColor
{
    KWhite,
    KBlack,
    KYellow
};
// 不带作用域的枚举类型可以自动转换成整形，不再的枚举类型可以相互比较，这是一个 bug
// 防止以上的问题出现了 enum class
enum class AColor1
{
    KRed,
    KGreen,
    KBlue
};
enum class BColor1
{
    KWhite,
    KBlack,
    KYellow
};
// 条件变量
std::condition_variable cv;
std::mutex mutex_;
int count = 1;

void func1()
{
    std::unique_lock<std::mutex> lock(mutex_);
    --count;
    if (count == 0)
        cv.notify_all();
}
void func2()
{
    std::unique_lock<std::mutex> lock(mutex_);
    while (count)
    {
        cv.wait(lock);
    }
}
void customSleep();
void test5()
{
    // 原子操作
    std::atomic<int> ai;
    ai++;
    ai.store(100);
    int a = ai.load();
    std::cout << a << '\n'
              << ai << '\n';
    if (KRed == KWhite)
    {
        //对 customSleep 函数进行计时
        std::chrono::time_point<std::chrono::high_resolution_clock> begin = std::chrono::high_resolution_clock::now();
        customSleep();
        auto end = std::chrono::high_resolution_clock::now();
        long long diffCount = std::chrono::duration_cast<std::chrono::microseconds>(end - begin).count();
        std::cout << "执行总耗时为：  " << diffCount << '\t\n';
        std::cout << "red == white " << '\n';
    }
    // if(AColor1::KRed == BColor1::KWhite){
    //     // ... 报错编译失败
    // }
    func1();
    func2();
}
// chrono 时间相关库
void customSleep()
{
    std::this_thread::sleep_for(std::chrono::milliseconds(5));
    std::this_thread::sleep_for(std::chrono::seconds(5));
    /*
    它包含三种时钟：
    steady_clock 单调时钟，只会增加 ，常用记录程序耗时
    system_clock 系统时钟 , 会随系统时间改动而变化
    high_resolution_clock 当前系统最高精度的时钟，通常就是 steady_clock

    */
}

class inner
{
public:
    inner() { std::cout << "Constructing" << '\n'; }
    ~inner() { std::cout << "Destructing" << '\n'; }
};

void test7(int num1)
{
    std::vector<int> vec;
    std::cout << vec.size() << '\t' << vec.capacity() << '\t';
    vec.push_back(11);
    vec.emplace_back(12);
    vec.push_back(13);
    std::cout << vec.size() << '\t' << vec.capacity() << '\t';
    for (auto iter = vec.rbegin(); iter != vec.rend();)
    { // cbegin begin rbegin
        std::cout << *iter << '\t';
        ++iter;
    }

    std::array<int, 10> array;
    array.at(2) = 10;
    std::cout << array.at(1) << '\n';

    // 数组长度可以是变量；但是不建议使用，数组使用的栈内存，是有大小限制的，可能会发生栈溢出
    int array1[num1];
    std::cout << "数组容量为：" << sizeof(array1) << '\n';
    int a;
    int b[4];
    int c;
    std::cout << "func2a address\t" << &a << '\n';
    std::cout << "func2b address\t" << &b << '\n';
    std::cout << "func2c  address\t" << &c << '\n';
    for (int i = 0; i < 200; i++)
    {
        std::cout << b[i] << '\t';
    }
    // new[] 和 delete[] 一定要配对使用
    // new[] 会在头部多申请 4个字节，用于存储数组长度，这样 delete[] 才知道要调用多少次析构函数

    inner *p = new inner();
    inner *pa = new inner[2];
    delete p;
    delete[] pa;
    // 当类型为内置类型时，可以不配对使用，当是自定义类型时，必须要配对使用
    // 一般基类的析构函数都要设置成虚函数，不设置成虚函数，在析构的过程中只会调用到基类的析构函数而不会调用到子类的析构函数，可能会产生内存泄漏

    // volatile 关键字和 const 关键字相对应。
    // volatile 关键字告诉编译器其修饰的变量是易变的，不要对修饰的变量做过度的优化
    volatile int voa = 10;
    // 一个对象是 volatile ，那么对象里的所有成员都是 volatile
    // volatile 只保证内存可见性，不能保证操作的原子的

    // 死循环：因为减到0的时候，再减就会变成一特别大的正数，不会为负数
    // for(unsigned int i = 10 ; i >= 0 ; --i){
    //     std::cout<<"==================="<<'\t';
    // }
    // 容器的 size()  返回类型是无符号整数
    // auto i1=vec.size();
    // for(auto i=i1 ; i >=0 ; --i){
    //     std::cout<<vec[i]<<'\t';
    // }
    // 不要返回局部变量的指针或引用
}

/*
规范要点：
每个头文件都要使用 #ifndef  #define #endif 或者 #pragma once 修饰，避免重复引用
所有的引用形参不做改动都要加 const ，任何可能的情况下都要加 constexpr 或 const
new 内存的地方尽量使用智能指针

*/

// 自定义异常
class MyException : public std::runtime_error
{

public:
    MyException() : std::runtime_error("MyException") {}
};
// 如果一个成员函数不会产生任何异常，可以使用 noexcept 关键字修饰
// 永远不要在析构函数中把异常抛出

void test6()
{
    try
    {
        // throw MyException();
        test7(10);
    }
    catch (const std::exception &e)
    {
        std::cerr << e.what() << '\n';
    }
}
#endif
```

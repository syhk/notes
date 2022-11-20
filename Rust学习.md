Rust

> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=iJZ37&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 学习建议：


-  先从 整体出发，不要让自己陷入到细节中去 
-  和自己已知的知识建立联系 
-  rust 和go一样采用 组合的手段实现代码复用，不要深思为什么不是继承 
-  学会阅读源码，从源码中学习 
-  Rust设计哲学 
-  使用 cargo new 项目名 

在终端中构建项目

使用 cargo build 来构建和运行项目

也可以使用 cargo run 来完成编译和运行任务

第一个程序：

```rust
fn main(){
    println!("hello world!");
}
```

==Rust中变量默认都是不可变的，如果要改变使用**mut**关键字来修饰变量就可以改变了。==

用let创建变量：

```rust
let foo = 5;//foo是不可变的
let mut bar = 5; //bar是可变的
```

==& 引用在默认情况也是不可变的==

```rust
&mut guess //声明一个可变的引用 
&guess //声明一个不可变的引用
```

示例：

```rust
use std::io; // 导入包
fn main() {
println!("Guess the number!"); //输出
println!("Please input your guess:");
let mut guess =String::new();//声明一个可变的变量并且绑定一个空白字符串
io::stdin().read_line(&mut guess).expect("Failed to read line");//从键盘获取输入
//read_line它读取的同时还会返回一个值 io::Result值（它一个枚举类型），它有OK和ERR两个变体，如果读取失败就会返回expect并输出里面的内容,没有编写expect函数会出警告
println!("You guessed: {}", guess); // 这里面的花括号是一个占位符,打印几个值就用几个花括号{}
let x=50;
let mut y = 100;
println!("x= {}, y = {}",x,y);//多个输出
println!("game over!");
}
```

```rust
use std::io;

fn main(){

println!("猜数！");

println!("猜测一个数字：");

let mut guess=String::new();//创建一个空白字符串并绑定到变量guess

io::stdin().read_line(&mut guess).expect("无法读取行！");
//下面这行和上面那行是等价的没有使用 use 导入的话，就需要使用下面的方式
// std::io::stdin().read_line(&mut guess).expect("无法读取行！");
println!("你猜测的数字是:{}",guess);

}
```

```rust
# 使用rand包需要在cargo.toml文件中将rand包声明为依赖
rand="0.3.14"  
# 添加完成后使用 cargo build 重新构建这个项目
```

升级依赖包使用 cargo update 命令升级依赖包

```rust
// 猜数游戏示例
use std::io;
use rand::Rng;//导入随机数模块  trait
use std::cmp::Ordering;
fn main() {
println!("Guess the number!");

let numrand=rand::thread_rng().gen_range(1,101);//左闭右合区间

println!("Please input your guess:");

let mut guess =String::new();

io::stdin().read_line(&mut guess).expect("Failed to read line");
//rust 允许使用同名的新变量来隐藏旧变量的值
//从这行之后，这个guess就不是上面那个变量了，而第二个guess是原来上面的那个变量
//用户要输入过程中按的回车键，会导入我们的输入字符串额外多出一个换行符，所以使用 trim（）函数来去除
//trim()就是去掉字符串前后的空格
//parse()方法会把字符串解析成数值类型
let guess:u32=guess.trim().parse().expect("plasce type a number!");

println!("You guessed: {}", guess);

println!("随机数字为：{}",numrand);
match guess.cmp(&numrand){//和谁匹配就执行谁
Ordering::Less => println!("too small!"),
Ordering::Greater => println!("to big!"),
Ordering::Equal => println!("you win!"),
}

println!("game over!");

}
```

上面的代码里面有一个概念叫做 **隐藏（shadow）**:

rust 允许使用同名的新变量来隐藏旧变量的值

#### 使用循环来实现多次猜测

```rust
use std::io;
use rand::Rng;
use std::cmp::Ordering;
fn main() {
println!("Guess the number!");

let numrand=rand::thread_rng().gen_range(1,101);//左闭右合区间

loop{//循环起始位置
println!("Please input your guess:");
let mut guess =String::new();

io::stdin().read_line(&mut guess).expect("Failed to read line");

let guess:u32=match guess.trim().parse(){
Ok(numrand) => numrand,
Err(_) => continue,//不需要错误信息可以下划线忽略
};//这里把 expect方法换成了match表达式

println!("You guessed: {}", guess);

println!("随机数字为：{}",numrand);

match guess.cmp(&numrand){
Ordering::Less => println!("too small!"),
Ordering::Greater => println!("to big!"),
Ordering::Equal => {
    println!("you win!");
    break;//猜对了就退出
}
}
}
}
```

这里把 expect方法换成了match表达式

```rust
let guess:u32=match guess.trim().parse(){
Ok(numrand) => numrand,
Err(_) => continue,//不需要错误信息可以下划线忽略
};
```

continue用法和C++、go、java等语言一样！

### Rust保留的关键字
| 关键字 | 描述 |
| --- | --- |
| as | 执行基础类型转换，消除包含条目的指定 trait 的歧义，在 use 与 extern crate 语句中对条目进行重命名 |
| break | 立即退出一个循环 |
| const | 定义常量或者不可变祼指针 |
| continue | 继续下一次循环迭代 |
| crate | 连接一个外部包或一个代表了当前包的宏变量 |
| dyn | 表示 trait 对象可以进行动态分发 |
| else | if 和 if let 控制结构的回退分支 |
| enum | 定义一个枚举 |
| extren | 连接外部包、函数、变量 |
| false | 字面量布尔值假 |
| fn | 定义一个函数或者函数指针类型 |
| for | 在迭代元素上进行迭代，实现了一个 trait，指定一个高阶生命周期 |
| if | 基于条件表达式的分支 |
| impl | 实现类型自有的功能或者 trait 定义的功能 |
| in | for循环语法的一部分 |
| let | 绑定一个变量 |
| loop | 无条件循环 |
| match | 用模式匹配一个值 |
| mod | 定义一个模块 |
| move | 让一个闭包获得全部捕获变量的所有权 |
| mut | 声明引用、祼指针或者模式绑定的可变性 |
| pub | 声明结构体字段、impl块或模块的公共性 |
| ref | 通过引用绑定 |
| return | 从函数中返回 |
| Self | 指代正在其上实现 trait 的类型别外  S是大写的
= |
| self | 指代方法本身或者当前模块  s是小写的 |
| staticc | 全局变量或者持续整个程序执行过程的生命周期 |
| struct | 定义一个结构体 |
| super | 当前模块的父模块 |
| trait | 定义一个 trait |
| true | 字面量布尔真 |
| type | 定义一个类型别外或关联类型 |
| unsafe | 声明不安全的代码、函数、trait或实现 |
| use | 把符号引入作用域中 |
| where | 声明一个用于约束类型的 从句 |
| while | 基于一个表达式结果的条件循环 |


==未来可能会使用的保留关键字：==

| abstract | async | become | box | do |
| --- | --- | --- | --- | --- |
| final | macro | override | priv | try |
| typeof | unsized | virtual | yield |  |


### 通用编程概念

```rust
fn main(){
println!("hello world!");
//默认不可变
let mut x = 5;

println!("x= {}",x);

x=10;

println!("x= {}",x);

//常量:它不可以使用mut关键字，常量永远都是不可变的
//声明一个常量使用const关键字
//常量只可以绑定到常量表达式
//RUST中常量 一般使用大写字母
const MAX_POINTS:u32=1000;


//Shadowing
let x = 5;
println!("{}",&x);//5
let x =x+1;
println!("{}",&x);//6
let x =x+2;
println!("{}",&x);//8

let spaces_str="     ";
let spaces_num=spaces_str.len();
println!("{}",spaces_num);//5
```

使用 const来定义一个常量，不能使用let关键字来定义常量；

==不能使用 mut 关键字修饰一个常量==，**常量总是不变的！**

在 rust 中，变量名一般都有大写！

隐藏机制不同于将变量声明为 mut 的 ！
重复使用 **let** 关键字**会创建出新的变量**，因此可以复用的时候改变它的类型！

## rust数据类型

#### 标量类型和复合类型

注意：**Rust是一门静态类型语言，这意味着它在编译程序的过程中需要知道所有变量的具体类型。**

> 标量类型：单个值类型的统称； 4种：整数、浮点数、布尔值、字符


```rust
/*标量类型：整数，浮点数（f32,f64默认的），布尔值，字符（char）,字符串，元组，枚举
复合类型：数组，结构体，指针，元组，枚举
u32:无符号整数类型，占32位空间
u8,u16,u32,u64,u128 无符号
i8,i16,i32,i64,i128 有符号
无符号以U开头，有符号以I开头
整数默认类型是i32
*/
//isize 和 usize 这两种是由运算程序的计算机硬件决定的
let guess:u32 = "42".parse().expect("Not a number!");
println!("{}",guess);
//rust声明的变量没有使用会有警告
let x:f32 = 3.0;
let x:f64 = 4.0;
let b:bool = true;
let b:bool =false;
let x='z';
let y:char ='y';
let z='😀';//也可以存放这种
```

整数类型有: (它们占的空间大小也就是后面对应的数字单位为bit)

-  无符号：u8,u16,u32,u64,usize 
-  有符号：i8,i16,i32,i64,isize 

==usize和isize==：取决于程序运行的目标平台；在64位架构上就是64bit，而32位架构上就是 32bit

rust默认的整数字面量是：**i32**

浮点数类型：

-  f32 
-  f64 （默认） 

> 复合类型：可以将多个不同类型的值组合为一个类型；2种：元组（tuple） 数组 （array）


```rust
//复合类型
//Tuple类型可以将多个类型的值放在一个类型里面，和C++中的元组类似
//tuple的长度的固定，一旦创建就不能改变
//如果不明确是什么类型，可以使用_来代替
let tup: (i32,char,bool)=(15,'a',true);
let tup:(_,_,bool)=(3.14,"boy",false);
let tup=(50,1.25,1);//也可以使用模式匹配
let ont=tup.0;
let two=tup.1;//也可以通过点来进行访问
let (x,y,z)=tup;
println!("{},{},{}",x,y,z);//获取tup的值 解构：将元组拆解为n个不同的部分
//数组
//数组和C++中的差不多
//长度也是固定的
// Vertor更加灵活，长度可以改变
//和数组类似，不确定使用哪个，就使用 Vector
let arr=[1,2,3,4,5,6,7,8,9];
let avec=vec![1,2,3,4,5,6,7,8,9];
println!("{}",avec[0]);
println!("{}",arr[5]);
//另外一声明数组的方法
let a=[3;5]; //创建数组并且初始化为5个3
```

元组和数组都拥有固定的长度！

元组用小括号（）；数组用中括号 []

有一种动态数组类型：**vector**

## 函数

rust使用蛇形命名法（只使用小写字母，使用下划线分隔单词）来规范函数和变量名称的风格！

```rust
// 函数调用 
add_function();
add(4,56);

let y=5+6;

let y={//表达式
    let x=3;
    x+1 //这个加上了分号就变成了语句；这个相当于返回值
};

let n1={
    let u=6+5;
    u
};


println!("{}",y);//4

=================================================
//函数和注释
// 声明函数使用  fn 关键字 : go语言使用 func 关键字
// 规范是函数名称使用小写，单词之间使用_分割
fn add_function(){
println!("hello function");
}

fn add(x:u32 , y: u32){//rust必须指定函数参数类型
println!("x={}",x);
println!("y={}",y);
}
// 函数的返回值
fn add1(x:u32 , y:u32) ->u32 {//在参数括号后面加上->类型 就是返回
let x=x+y;
x   //返回语句不能有分号，有了分号就变成了语句
}
```

参数和参数类型之间使用 ： 分隔！

rust把语句和表达式区分为两个不同的概念：

-  **语句：** 执行操作但不会返回值的指令 
-  **表达式：** 会进行计算并且产生一个值作为结果的 指令 

记住：**语句不会有返回值**

**表达式加上分号就会变成了语句。**

rust函数的返回值使用 **->** **返回值类型**；如果是多个就是小括号括起来：**->** **（类型1， 类型2 ….）**

```rust
fn five() -> i32 {
    5  //这样也是对的返回5
    //如果不使用这种方式返回，也可以使用 return，使用这个需要加分号 : return 5;
}
=================================================

fn five() -> i32 {
    5; //错的！！！不能加分号
}
```

### rust注释

> // 单行注释


> / **/ 多行注释


### 控制流

#### if 和 else

示例：

```rust
// 控制表达式  if else
//这个和go,python语言差不多
let x=5;

if x<10 {
    println!("你的数字真小！");
}else if x>10&&x<90{
    println!("你的数字在10-90之间！");
}else{
    println!("你的数字真大！");
}
let y=true;//这里y的值不能为0或者1不然会报错！！！
if y {
    println!("{}",y);
}
```

==rust不会自动尝试将非布尔类型的值转换为布尔类型！！！==，所以上面代码中y的值只能为布尔值。

过多的else if 表达式应该用 match 替代！！！

```rust

let b=10;
//if是一个表达式，可以let语句右侧使用它来生成一个值
// 也可以这样判断实现像 ? : 相同的功能
let number=if b>5 {100} else { 900 }; //if和else里面的类型要一样，静态编译型语言

//else if 太多了，可以使用match来重构
let number=100;
match number{//这规则和case差不多，也可以使用下划线_
    1=>println!("one"),
    2=>println!("two"),
    3=>println!("three"),
    _=>println!("other"),
}
```

==所有 if 分支里面可能返回的值都必须是一种类型的==

```rust
let nu=if 4>5{
    54-12
}else{ -900+65};//像这样也是可以的，记住不要里面加分号
println!("nu={}",nu);
```

#### rust循环结构

==**rust提供了 3 种循环结构：loop 、while、for。**==

> loop  ：反复执行一块代码，直到条件满足（break）或者我们强制退出！！


```rust
loop {
    ...
    if 条件 {
        
    }
}
```

```rust
// loop  不会像 do while必定会执行一次，其他和它一样，如果条件放在最前面，一开始就不成立，就不会执行
let mut count=0;
let res = loop{
   count+=1;
    println!("725");
    if count==10{
        break count;
    }
};//这里分号别忘记写了
```

```rust
let mut con1=0;
let res=loop{
    con1+=1;
    if con1 == 10 {
       break con1*2 //这里不加分号
    }
};
println!("con1={}",res);//20
let mut con2=0;
let res=loop{
    con2+=1;
    if con2 == 10 {
       break con2*2; // 这里加上分号
    }
};
println!("con2={}",res);//20
//上面两种方式是等价的
```

> while  用法和其他语言一样


```rust
// while
let arr=[1,2,3,4,5,6,7,8,9];
let mut le=arr.len();//数组长度比数组下标大1
while le>0{
    le=le-1;
   println!("{}",arr[le]); 
}
```

> for 循环：推荐使用简洁又高效；rust最为常用


```rust
// for
// 使用for循环又安全又高效
let arr=[1,2,3,4,5,6,7,8,9];
for a in arr.iter(){
println!("{}",a);
}
===============================================
语法： 
for 变量名
```

Range：用来生成数字序列！

```rust
// Range 标准库提供
// 指定一个开始数字和一个结束数字，它可以生成它们之间的数字（左闭右开）
// rev方法可以反转 Range
for number in(1..10).rev(){//小括号数字中间是两个点
    println!("{}",number);
}
```

# 所有权

```rust
// 所有权是Rust最独特的特性核心特性
// 内存是通过所有权系统来管理的
//堆和栈是代码在运行时可以傅 的内存空间
// stack 栈  这上面的数据必须拥有固定的大小 
//  heap 堆  编译时大小未知或者大小可能发生变化的数据必须存放在 heap中
// 访问heap中的数据要比访问stack中的数据慢，多了次指针跳转
```

**所有权是Rust最独特的特性核心特性**

**所有权规则：**

1.  **rust中的每一个值都有一个对应的变量作为它的所有者；** 
2.  **在同一时间内，值有且仅有一个所有者；** 
3.  **当所有者离开自己的作用域时， 它持有的值就会被释放掉；** 

作用域：一个对象在程序中有有效范围；

rust 中可以用大括号 {} 表示一个作用域，或者隔离一个作用域！！！

### String类型

==字符串字面量是不可变的；==

```rust
let s="hello world!"; //不可变的 分配在栈上的
```

为了方便操作rust提供了第二种String类型：这个类型会在==**堆**==上分配自己需要的存储空间：调用 **from** 函数来创建 String 实例

```rust
let s=String::from("hello");
//在堆上分配的，是可变的
```

**区别：字符串字面量是分配在栈上的不可变，而String是分配堆上的是可变的！！！**

内存布局：



注意图中 String 类型的分配方式；

#### 内存与分配

**两个关键概念：**

-  rust 在变量离开作用域的时候，会调用一个叫作 drop的特殊函数 
-  **rust会在作用域结束的地方自动调用 drop 函数** 

> 在C++中这种对象生命周期结束时释放资源的模式也称为资源获取即初始化（RAII）


变量和数据交互的方式：移动 Move

```rust
// 变量和数据交的方式：移动（Move)
//多个变量可以与同一个数据使用独特的方式来交互
let s1=String::from("shenyang");
let s2=s1;//在这里这样，rust会废弃s1的所有权，s1的值被移动到s2中，s1的值被清空
//println!("{}",s1);//这里使用报错，因为s1已经被废弃了
/*let s1="shenyang";
let s2=s1;
像这样就可以，不会报错！！！
*/
println!("{}",s2);
// 一个String 由3部分组成：
// 一个指针，len（长度）,cap（容量）分配在栈上，而字符串的内容被分配在堆上
```

内存布局：





上面把 s1 的值赋给 s2 的时候只复制了它在**存储在栈上的指针、长度及容量字段**。

需要注意的是**它没有复制指针指向的堆上数据！**

> 引出问题：s1 和 s2 离开作用的时候会尝试去重复释放相同的内存，导致二次释放


> rust解决方案： rust在这种情况下会将 s1 废弃，不再视为一个有效的变量，s1 离开作用域后也不需要清理任何东西！！！


### 浅拷贝和深拷贝

> C++中的深浅拷贝：


-  深拷贝：在堆区重新申请空间进行拷贝操作、拷贝完整的内容 
-  浅拷贝：只拷贝地址，也就是编译器本身提供的拷贝构造函数做的浅拷贝操作 

> 浅拷贝带来的问题：堆区的内存重复释放以及内存泄漏
有堆区开辟的属性，一定要提供拷贝构造函数防止浅拷贝带来的问题。


rust拷贝s1到s2的方式就可以视为浅拷贝。

术语： 移动（MOVE)

rust中应该是 s1 被移动到 s2 中。因为 s1 会被废弃了！！

**一个设计原则****：rust 永远不会自动地创建数据的深拷贝。所以在 rust中，任何自动的赋值操作都可以视为高效的。**

需要用到深拷贝就是克隆（clone)

#### 变量和数据交互的方式： 克隆 Clone

当要做深拷贝操作的时候，rust提供一个方法： Clone()

```rust
// clone 克隆 比较消耗资源
let a1=String::from("hello");
let a2=a1.clone();//克隆作了深度拷贝操作
println!("{},{}",a1,a2);//这里a1变量就没有被废弃，因为是直接把a1克隆给a2
// 和上面的作对比
```

Clone()方法复制栈上数据的同时，也复制了堆上的数据！！！

> 克隆有个缺点：就是比较消耗资源




==重点：==**如果一个类型拥有了 Copy 这种 trait ，那么它的变量可以在赋值给其他变量之后仍然保持可用性。**

**如果一个类型本身或者这种类型的任意成员实现了 Drop 这种 trait ，那么rust 就不允许它实现 Copy 这种 trait了。**

```rust
/*
stace上的数据：复制
Copy trait，可以用于完全存放在栈上的类型
如果一个类型实现Copy trait，那么旧的变量在赋值后仍然可以使用
一些拥有Copy trait的类型：
任何简单标量的组合类型都可以是Copy的；任何需要分配内存或者某种资源的都不是Copy的
拥有的：bool char 所有的浮点类型，所有的整数类型
tuple(元组)前提是其中所有的字段都是Copy的 eg:
（i32,i32)是
（i32,String)不是
*/
```

**任何简单标量的组合类型都可以是Copy的；任何需要分配内存或者某种资源的都不是Copy的**

### 所有权与函数

理解这里：要理解了上面的内容比如：复制操作、克隆操作、 Copy 、Drop

```rust
fn main(){
 // 所有权与函数
// 函数在返回值的过程中也会发生所有权的转移
let s1=gives_ownership();

let s2=String::from("hello");

let s3=takes_and_gives_back(s2);//s2的所有权被移动到函数里面，从这里开始 s2 不再有效

 let s4=100;//由于 i32 类型是 Copy 的，我们在这里之后还可以继续使用 s4
    
/*
一个变量离开作用域时会被Drop函数还回，除非它的所有权被转移另外一个变量上
*/
    
}

fn gives_ownership() -> String{
let some_string=String::from("hello");
some_string //这个的所有权移动到调用它的上面也就是上面的s1上
}

fn takes_and_gives_back(a_string:String)->String{//s2的所有权被移动到函数参数上面
    a_string //这个作为返回值的所有权移动到调用它的上面也就是上面s3上面
}

fn makes_copy(x:i32) {
    println!("x={}",x);//x在这里离开作用域并不会有什么特别的事发生就是正常的消亡
}
```

上面的函数中的返回值是**移动操作**不是返回所有权操作 ！！！参数传递进去函数的时候，函数会获得所有权

#### 返回值和作用域

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f30d.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=gqPlC&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)

> 遵循模式：将一个值赋值给另外一个变量时就会发生所有权转移，当一个持有堆数据的变量离开作用域时，它的数据就会被 Drop 清理回收，除非数据的所有权被移动到了另一个变量上 面。


**函数在返回值的过程中也会发生所有权的转移！！！**

问题：当希望调用函数的时候保留参数的所有权，就要将传入的值作为结果返回，但同时函数也可能会需要返回自己的结果。

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f440.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=jOE6p&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：采用元组解决：太过于繁琐

```rust
fn main(){
    
    let s1=String::from("hello");
    
    let (s2,len)=calculate_length(s1);
    //接收多个参数的时候，需要忽略某个参数可以下划线 
    println!("{},{}",s1,len);
}

fn calculate_length1(s:String) ->(String,usize) {
   let length= s.len();//取得所有权
   (s,length)//采用元组解决同时返回多个值
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/2699.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=Cvp8d&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18):采用**元组**可以让函数同时返回多个值！！！

## 引用与借用

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4c4.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=MCr7K&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 解决上面采用元组返回太过于繁琐的问题

> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/2b55.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=J1Jxq&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 问题：我们想要调用函数的时候，不转移值的所有权


![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f58c.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=Gdqb5&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**&** 代表**引用**的含义，**可以在不获取所有权的情况下使用值。**

        *****  代表**解引用**

        **&** **参数类型** 不可变引用（默认的）

        **& mut**  **参数类型** 可变引用（调用时的参数也要是可变）

```rust
fn main(){
 let mut s1=String::from("hello");
    
//& 表示引用，允许使用值并且不取得所有权  对应解引用 * 
//把引用作为函数参数传递就叫引用
// 不可以修改借用的东西，引用也是默认不可变的，可以使用 mut来让引用可变  &mut 数据类型/参数
let len=calculate_length(&mut s1);//参数的变量也要是可以变的，否则会报错
    
println!("{},{}",s1,len);  
}

// 函数使用变量不获得所有权
fn calculate_length(s:&mut String) ->usize {//注意参数里面不是在变量前面加& ,而是在类型前面加&
    s.len()//不会取得所有权
}

fn first_world(s: &String ) -> usize {
let bytes=s.as_bytes();
for (i,&item) in bytes.iter().enumerate() {
   if item == b' '{
       return i;
   }
}
s.len()
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f5bc.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=xCEew&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18):



![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=yOxVx&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**通过引用传递参数给函数的方法就叫做****借用**！！！

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f441-200d-1f5e8.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=D5Urm&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：可变引用

```rust
fn main(){
    
 let mut s=String::from("hello");
change(&mut s);  
    
}
// 可变引用
fn change(some_string: &mut String){
    some_string.push_str(", world");
    //相当拼接字符串的功能 append
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=u05uz&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**限制点：在特定作用域中，对于某一块数据，只能有一个可变的引用（一次只能声明一个可变引用 ）。** 可以通过大括号来分隔作用域实现有多个可变引用

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f440.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=NJeAO&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：这里要多想想记住！！！！

```rust
// 可变引用有一个限制：要特定作用域内，对于某一块数据，只能有一个可变的引用 。
 let mut p=String::from("hello");
 let z1=&mut p;
 let z2=&mut p;//报错！！！违反了规则
 println!("{},{}",z1,z2);
```

上面的限制性规则可以帮助我们在编译时避免数据竞争。

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d6.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=nQfyA&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：数据竞争

> 以下三种行为会发生数据竞争：


1.  **两个或者多个指针同时访问同一个数据** 
2.  **至少有一个指针用于向空间中写入数据** 
3.  **没有使用任何机制来同步对数据的访问（没有同步访问）** 

```rust
// 可以通过创建新的作用域，来允许非同时的创建多个可变引用
//eg:
let mut k=String::from("hello");
{//可以大括号分隔作用域
    let s1=&mut k;
}//到这里s1就不再有效了,因为已经出了作用域了
let s2=&mut k;
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=Qzsgl&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：可以通过花括号{ } ，来创建一个新的作用域范围，这样就可以创建多个可变引用 ！！！

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=bpvub&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：限制：**不可以同时拥有一个可变引用 和 一个不可变的引用；****但同时有多个不可变的引用是可以的**

```rust
let mut s=String::from("hello");
let r1=&s;
let r2=&s;
let s1=&mut s;//报错！！！！：因为不可以把s借用为可变的引用，因为它已经借给了不可变的引用 
println!("{}，{}，{}",r1,r2,s1);
```

#### 悬垂引用

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4c4.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=yxEFh&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)**概念：** 一个指针引用了内存中的某个地址，但是这块内存可能已经释放并且分配给其它变量使用了。

rust保证不会让引用进入悬垂状态！！！

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=AIM7t&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：这里我目前可以理解为：**C++中的不要返回局部对象的引用，因为离开它自己的作用域也就被销毁了！！！**

```rust
fn main(){
    // 悬空引用示例 
   let r=dangle();
}
fn dangle() -> &String {//报错！！！
let s = String::from("hello");
&s  //s的引用返回给调用者，s在这里离开作用域并且被销毁，它指向的内存也就无效了
}
====================================================
//直接返回 String 就不会报错了
fn dangle() -> String {
let s = String::from("hello");
s   //所有权被转移出函数并没有被销毁
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=Dmvfe&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：引用的规则

-  在任何一段给定的时间内，要么只能拥有一个可变引用，要么只能拥有任意数量的不可变引用 
-  引用总是有效的 

### 切片（slicce）

切片（slicce）：是 rust 中不持有所有权的数据类型。（允许我们**引用**集合中某一段连续的元素序列）

使用方式和go语言的切片差不多。

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f440.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=RKZ9R&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：示例

```rust
fn main(){
    let mut s=String::from("hello");
let wordindex=first_world(&s);
println!("{}",wordindex);
// 字符串切片 和 
let s=String::from("hello world!");
let hello=&s[0..5];//左闭右开  中间数字之间也两个点
let hello=&s[..5];//等价于上面那个
let world=&s[6..11];
let world=&s[6..];//和上面那个一样
// 整个字符串
let u=[..];
}
```

==方括号数字之间是两个点==:

> 字符串切片的边界必须位于有效的 UTF-8 字符边界内。


[ start ..  end ]  是一个左闭右开区间

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d6.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=ubc8y&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：字符串切片的类型是： **&str**

> 因为 &str 是一个不可变的引用，所以字符串字面量自然也是不可变的


![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=hEoN8&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)**：字符串字面值实质上是一个切片**

```rust
fn main(){
    let s1="hello";//字符串字面值实质是切片
 // 将字符串切片作为参数传递
// 使用 &str作为函数参数，这样就可以现时接String 类型和 & str 类型的参数，更加通用
// eg:
let my_string=String::from("hello world");
let wordindex=first_w(&my_string[..]);
let my_string_str="hello world";
let wordindex=first_w(&my_string_str[..]);//可以简化为下面这种形式，因字符串字面值本质是切片
let wordindex=first_w(my_string_str);
    
}


fn first_w(s: &String ) -> usize {
let bytes=s.as_bytes();
for (i,&item) in bytes.iter().enumerate() {
   if item == b' '{
       return i;
   }
}
s.len()
}



// 示例函数 参数为String引用建议改为这个
//因为这样改进后既可以处理 String类型又同时可以处理 &str 类型，更加通用
fn first_w(s:&str) -> &str{
let bytes=s.as_bytes();
for ( i,&item) in bytes.iter().enumerate(){
if item==b' '{
return &s[..i];
}
}
&s[..]
}
```

数组切片：

```rust
// 这个切片go语言中的切片用法差不多,go和rust中的切片数字都不能为负数
let a=[1,2,3,4,5];
let slice=&a[1..5];//数组切片；切片的第二个参数不可以像python那样写成负数
```

## struct  结构体

```rust
//语法：
struct 结构体名  {
    字段名 ： 类型 ,
    字段名 ： 类型 ,
    ...          ,
    //最后一个字段也要有逗号
}
```

```rust
// 定义一个结构体在花括号里面为所有字段定义名称和类型
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool, //最后一个对象也要有逗号
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=hUXcs&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 创建对象实例：**不能只赋值其中几个字段，必须对所有字段赋值，顺序可以不一样**

```rust
  //    创建一个对象实例
    // 不能只赋值其中几个字段，必须对所有字段赋值，顺序可以不一样
    let mut user1 = User {
        email: String::from("725482520"),
        username: String::from("shenyang"),
        active: true,
        sign_in_count: 1,
    };
```

==使用 .  来说属性==

```rust
    // 使用点 . 来访问属性
    println!("{}", user1.email);
    println!("{}", user1.username);
    println!("{}", user1.active);
    println!("{}", user1.sign_in_count);
   // 更改结构体的字段的值
user1.email = String::from("654321"); 
//前提要是创建对象是要可变的 加了 mut 关键字
//struct 的实例是可变的，那么实例中所有字段都是可变的
```

> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4a5.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=HtTBD&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)**更新语法：** 想用某个struct实例来创建一个新实例的时候可以使用更新语法
语法： .. 对象实例名


```rust
   // struct更新语法：想用某个struct实例来创建一个新实例的时候可以使用更新语法
    let user2 = User {
        email: String::from("123456"),
        username: String::from("sy"),
        ..user1 
//在这使用了更新语法（也就是除了上面两个字段，其他的字段跟user1的一样）
    };
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f985.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=tWHxJ&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：上面更新语法：user2除了自己定义的两个属性，其他的属性和 user1 相同。

**struct可以作为函数的返回值**

```rust
fn restr(e: String, u: String) -> User {
    User {
        email: e,
        username: u,
        active: true,
        sign_in_count: 1,
    }
}
```

**字段初始化可以简写**

```rust
//字段初始化可以简写，当字段名与字段值对应变量名相同时，就可以省略字段名
fn restr1(email: String, username: String) -> User {
    User {
        email, //可以使用简写方式
        username,
        active: true,
        sign_in_count: 1,
    }
}
```

**struct 的实例是可变的，那么实例中所有字段都是可变的**

结构体实例对象也分为可变和不可变的：

```rust
let mut 名称 = 结构体名 {
    对应字段赋值
}
====================================================
let  名称 = 结构体名 {
    对应字段赋值
}
```

#### Tuple struct

```rust
//语法：
struct 名称 ( 类型1 ， 类型2 ，类型3 ... );
//最后一个类型不需要加逗号

struct Color(i32, i32, String, bool);
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f440.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=IxZhF&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：实例

```rust
// tuple struct 实例
let red = 
Color(255, 255,String::from("blacke"),true); 
//tuple struct 的实例
let mut black = 
Color(255, 255, String::from("blacke"), true); //tuple struct 的实例
black.0 = 246; //也可以使用点语法来访问元素
black.1 = 200; //想要改变必须创建时是可变的
black.2 = String::from("red");
```

使用点来访问属性，字段序号从 0  开始 !!!

#### struct 方法

**方法第一个参数是self，相当于C++中的 this，方法可以有多个参数，但第一个必须是self**** _，在 impl 块里面定义方法_*

每个 struct 允许拥有多个 impl 块

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&height=18&id=ReKi8&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：语法

```rust
impl  结构体名 {
    方法
}
```

```rust
// struct 方法
// 方法第一个参数是self，相当于C++中的 this，方法可以有多个参数，但第一个必须是self
//访问使用实例对象 加 . 访问
// 在 impl 块里面定义方法
// 每个struct允许拥有多个 impl 块
impl Rectangle {//绑定方法到 struct上   impl  结构体名 { 对应的方法 } 
    fn area(&self) -> u32 {//也有可变与不可变 self  &self  &mut self  对应 获得所有权  借用 可变借用
        self.width * self.length
    }

// 关联函数，是函数不是方法 通过用于构造器
//调用关联函数使用  类型名：：函数名
    fn square(size:u32) -> Rectangle {//创建一个正方形
        Rectangle {
            width: size,
            length: size,
        }
    }
}

impl Rectangle {

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.length > other.length
    }

}
```

**关联函数：** 不带 self 的函数

```rust
// 关联函数，是函数不是方法 通常用于构造器
//调用关联函数使用  类型名：：函数名

 fn square(size:u32) -> Rectangle {//创建一个正方形
        Rectangle {
            width: size,
            length: size,
        }
    }
```

示例：

```rust
// 示例
// 计算长方形的面积
fn area(dim: (u32, u32)) -> u32 {
    dim.0 * dim.1
}

fn main(){
    
        let w = 30;
        let l = 50;
        let rect = Rectangle {
            width: w,
            length: l,
        };
        println!("{}", area(&rect));
        println!("{}", rect.area());//使用对象实例调用它自己的方法
        fn area(rect: &Rectangle) -> u32 {
            rect.width * rect.length
        }

        println!("{:?}", rect);

        println!("{:#?}", rect);
    
}

#[derive(Debug)] 
struct Rectangle {
    width: u32,
    length: u32,
}

impl Rectangle {//绑定方法到 struct上   impl  结构体名 { 对应的方法 } 
    fn area(&self) -> u32 {//也有可变与不可变 self  &self  &mut self  对应 获得所有权  借用 可变借用
        self.width * self.length
    }

// 关联函数，是函数不是方法 通过用于构造器
//调用关联函数使用  类型名：：函数名
    fn square(size:u32) -> Rectangle {//创建一个正方形
        Rectangle {
            width: size,
            length: size,
        }
    }
}
```

### 枚举

**使用 enum 关键字**

```rust
//语法：
enum 名称 {
    字段1 ，
    字段2 ，
    ...
    //最后一个字段不需要加逗号
}
```

```rust
enum ipAddress{
    V4,
    V6
}
```

创建枚举对象

```rust
// 创建枚举
let four= ipAddress::V4;

let mut four= ipAddress::V4;
four=ipAddress::V6;

route(four);
route(ipAddress::V6);

fn route(ip : ipAddress){
    match ip {
        ipAddress::V4 => println!("ipv4"),
        ipAddress::V6 => println!("ipv6"),
    }
}
```

和 struct 组合：

```rust
enum ipAddress{
    V4,
    V6
}
// 和 struct 组合
struct add{
    ipkind:ipAddress,
    ip:String,
}
```

上面的可以由枚举变体替代：

```rust
//语法
enum 名称 {
    字段名 （类型1，类型2，类型3，...）,
    字段名  (类型),
    ...,
    //最后一个字段也要加逗号
}
```

```rust
// 可以将数据附加到枚举的变体中，这样就可以不用像上面那样要使用struct，每个变体可以拥有
// 不同的类型以及关联的数据量
enum ipAdd{
    V4(u8,u8,u8,u8),
    V6(String),
}
```

```rust
// 枚举变体
let four=ipAdd::V4(127,0,0,1);
let six=ipAdd::V6(String::from("1k:8p:1o:9D"));

// 枚举和结构体可以相互嵌套也可以自己嵌套，枚举也可嵌套枚举
```

为枚举定义方法也可以使用 impl 关键字：

```rust
impl ipAdd {
    
fn show(&self){
match self {
        ipAdd::V4(a,b,c,d) => println!("{}.{}.{}.{}",a,b,c,d),
  ipAdd::V6(a) => println!("{}",a),
    }
}
}
```

示例：

```rust
enum Message{
    Quit,
    Move {x : i32 , y : i32},
    Write(String),
    ChangeColor(i32,i32,i32),
}
/*
Quit 没有任何关联数据
Move 包含了一个匿名结构体
Write 包含了一个String
ChangeColor 包含了3个 i32 值
*/
```

### Option 枚举

**定义在标准库中在 Prelude 中**，用它来标识一个无值无效或缺失！

Option 是一个枚举，它可以有两个变体：**Some** 和 **None**。（描述了某个值可能存放（某种类型）或不存在的情况）

**rust 中没有 Null 或者 Nullable 的概念**，而是使用 Option 来表示可能存在（有值）或不存在（无值）的情况。

标准库定义：

**T 表示是一个泛型参数！！！**

```rust
enum Option<T> {//标准库定义
    Some(T),
    None,
}
```

示例：

```rust
// Option示例：
{
    let sn=Some(5);
    let ss=Some("a string");
    let absent_number: Option<i32>  = None;
}
```

==注意：==

-  **Option**** 和 T 是不同的类型，不可以把 Option 直接当成 T** 
-  **如果想使用 Option**** 中的T，必须将它转换为 T，或者使用 match 语句来处理 None 值** 

```rust
let x:i8 =5;
let y:Option<i8> =Some(5);
//x 和 y 是两种不同的类型
let sum= x + y; //报错：必须把 y 转换为 i8 类型
println!("{:?}",sum);
```

### match 和 if let

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=cMhBM&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**match 匹配必须穷举所有的可能性！！！**

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f440.svg#crop=0&crop=0&crop=1&crop=1&id=I2w1o&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：示例：

```rust
// 定义一个枚举
enum Coin{
  Penny,
  Nickel,
  Dime,
  Quarter,
}
//匹配 Coin 的所有可能性
fn value_in_cents(coin: Coin)  -> u8 {
 match coin{
    Coin::Penny =>1,
    Coin::Nickel =>5,
    Coin::Dime =>10,
    Coin::Quarter =>{
        println!("Quarter");
        25
        },
 }
}
```

如果处理的语句有多条，需要用大括号括起来！！！

匹配 Option

 示例：记住它只有两种状态！！

```rust
// 匹配 Option<T>
fn plus_one(x : Option<i32> ) -> Option<i32> {
    match x{
        None => None,
        Some(i) => Some(i+1),
    }
}
fn main(){
 let five = Some(5);
let six =plus_one(five);
let none = plus_one(None);  
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=N0s7h&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**如果情况太多，可以使用下划线通配符来代替其它情况，不用穷举所有的情况了！！！**

```rust
// 如果值的情况有点多，不想列出所有的情况，可以使用 _ 通配符来替代没列出的值
// 示例：这样就可以不穷举所有可能了
let v=0u8;
match v{
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("anything"),
    // 其他情况用 通配符替代
}
```

#### if let

> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d6.svg#crop=0&crop=0&crop=1&crop=1&id=D4ib1&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)： **if let 处理只关心一种匹配而忽略其它匹配的情况**


```rust
let v =Some(0u8);
match v {//这里只处理 3 和其他 两种情况这样使用 if let 更好
    Some(3) => println!("three"),
    _ => println!("anything"),
}
```

上面只处理一种情况值为 3 情况，可以使用 if let 来处理

```rust
// 简洁写法
if let Some(3) = v{
    println!("three");
}
```

### 包、单元包、模块

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=gBFRd&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：

-  ==包（package）==：一个用于构建、测试并分享单元包的Cargo 功能。 
-  ==单元包（crate）==：一个用于生成库或可执行文件的树形模块结构 
-  ==模块（module）及 usu 关键字==：它们被用于控制文件结构、作用域及路径的私有性 
-  ==路径（path）==：一种用于命名条目的方法，这些条目包括结构体、函数和模块等 

Cargo 会默认将 src/main.rs 视作一个二进制单元包的根节点而无须指定，这个**二进制单元包与包拥有相同的名称**。

> 模块：以 mod 关键字来定义一个模块，接着指明这个模块的名字，用花括号包裹块体。


路径：

-  使用单元包或字面量 crate 从根节点开始的**绝对路径** 
-  使用 self 、super 或内部标识符从当前模块开始的**相对路径** 

标识符之间使用 ::  隔开。

```rust
//  定义模块
mod my_mod{
    pub  fn a(){
          println!("a");
      }
      fn b(){
          println!("B");
      }
      mod my_mod1{
          fn c(){
              println!("C");
          }
      }

    pub  mod my_mod2{
       pub fn c(){
            println!("C");
        }
    }
   
  }

fn main() {
crate::my_mod::my_mod1::c();//报错因为 my_mod1 是私有的
    
    crate::my_mod::my_mod2::c();
    my_mod::my_mod2::c();
    
}
```

如果模块没有 pub 属性修饰，就不能直接访问，但如果 pub 修饰了模块，没有修饰里面的函数可以访问模块，但不能访问里面的函数。

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=WFKfG&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**Rust 中的所有条目（函数、方法、结构体、枚举、模块及常量）默认都是私有的。**

```rust
mod my_mod{
    pub  fn a(){
          println!("a");
      }
      fn b(){
          println!("B");
          my_mod2::c();
      }


      mod my_mod1{
          fn c(){
              println!("C"); 
            }
      }

    pub  mod my_mod2{
       pub fn c(){
            println!("C");
            crate::my_mod::a();
            super::a();
            crate::my_mod::b();
            //子模块可以使用它所有祖先模块中的条目
        }
    }
   
  }
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=KQvy9&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)**结论：在父模块中的条目无法使用子模块中的****私有条目****，但是子模块中的条目可以使用它****所有祖先模块中的条目****。**

#### 使用 pub 关键字来暴露路径

要注意一下使用 pub 关键字来暴露了模块，但是它里面的函数依然是私有的没有被暴露，要暴露某个函数必须要在前面加 pub 。

==super 关键字是从父级模块开始构建相对路径，它可以相当于 linux 文件系统中的两个点 .. 。==

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=OvWnL&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：结构体定义时使用了 pub ，结构体本身成为了公共结构体，但它的字段依旧还是私有的，要一个一个字段的进行是否需要成为公共的。

```rust
 mod back_of_house{
    pub struct Breakfase{
        pub name:String,//公共的
            age:i16,//私有的
    }
    impl Breakfase{
        pub fn new (name:String) -> Breakfase{
            Breakfase{
                name:name,
                age:18,
            }
        }

    }

  }

fn main() {

    let s1=back_of_house::Breakfase::new(String::from("shenyang"));
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=fds7f&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**我们将一个枚举声明为公共的时候，它所有的变体都自动变成为公共的，与结构体区分开**

```rust
mod sy{
    pub enum xianze{
        A,
        B,
        C,
        D,
    }
impl xianze{
    pub fn show(&self){
        println!("A");
    }
}   
}
fn main() {
    let x1=sy::xianze::A;
    x1.show();
    let x2=sy::xianze::B;
}
```

#### 用 use 关键字将路径导入作用域

使用  use 将路径引入作用域时也需要遵守私有性规则！！！

使用 use 指定相对路径必须要传递给 use 的路径的开始处使用关键字 **self** ，而不是从当前作用域中可用的名称开始。

使用 use 将函数的父模块引入作用域意味着在调用时我们必须指定这个父模块，从而更清晰的地表明当前函数没有定义在当前作用域中。

当使用 use 将结构体、枚举和其他条目引入作用域时，我们通常通过完整路径来引入而不是引入父级模块。

当引入的函数名称相同的时候，我们可以使用它们的父模块来区分两个不同的类型。

```rust
mod my_mod{
    pub  fn a(){
          println!("a");
      }
      fn b(){
          println!("B");
          my_mod2::c();
      }
   mod my_mod1{
          fn c(){
              println!("C"); 
            }
      }

    pub  mod my_mod2{
       pub fn c(){
            println!("C");
            crate::my_mod::a();
            super::a();
            crate::my_mod::b();
            //子模块可以使用它所有祖先模块中的条目
        }
    }
   
  }
use my_mod::my_mod1;//错误 my_mod1是私有的
  mod back_of_house{
    pub struct Breakfase{
        pub name:String,
            age:i16,
    }
    impl Breakfase{
        pub fn new (name:String) -> Breakfase{
            Breakfase{
                name:name,
                age:18,
            }
        }

    }

  }
//   use back_of_house::Breakfase;//绝对路径
//   use self::back_of_house::Breakfase;//相对路径

mod sy{
    pub enum xianze{
        A,
        B,
        C,
        D,
    }
impl xianze{
    pub fn show(&self){
        println!("A");
    }
}
    
}
use sy::xianze;
fn main() {
    let x1=sy::xianze::A;
    x1.show();
    let x2=sy::xianze::B;
    let s1=back_of_house::Breakfase::new(String::from("shenyang"));
    crate::my_mod::my_mod2::c();//报错因为 my_mod1 是私有的
    my_mod::my_mod2::c();
}
```

#### 使用as 来指定引入的别名

```rust
use sy::xianze as xz;//使用 as 指定别名
fn main() {
    let x1=xz::A;
    
    let x1=sy::xianze::A;
    x1.show();
    let x2=sy::xianze::B;
```

#### 使用 pub use 重导出名称

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
pub use crate::front_of_house::hosting;
pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

使用 use 关键字将名称引入作用域的时候，这个名称会以私有的方式在新的作用域中生效，为了让外部代码访问这些名称，可以使用 **pub use** ,也被称为 重导出。

#### 使用外部包

首先将它们列入 Cargo.toml 文件，再使用 use 来将特定条目引入作用域。

可以使用 嵌套的路径来清理众多的 use 语句：

```rust
use std::cmp::Ordering;
use std::io;
//==============================================
use std::{cmp::Ordering, io};

//使用 self
use std::io;
use std::io::Write;
//=================================================
use std::io::{self, Write};

//==============================================
use std::collections::*;//使用通配符
```

写出相同的部分，用花括号包裹有差异的部分。

可以使用通配符 * 来引入某个路径中所有的公共条目。

## 通用集合类型

```rust

use std::collections::HashMap;

//  vector 学习
fn main(){
// 只能存放相同类型的值
// 创建方式
let v:Vec<i32>=Vec::new();
//也分为可变和不可变


let mut v=vec![1,2,3];//vec后面有个 !

v.push(5);

// get 会返回一个 Option<&T>

match v.get(1){
    Some(x)=>println!("{}",x),
    None=>println!("None"),
}

// 读取 vector里面的值
println!("{}",v.get(0).unwrap());
// unwrap方式:标准库实现
// pub const fn unwrap<T>(self) -> T {
//     match self {
//         Some(val) => val,
//         None => panic!("called `Option::unwrap()` on a `None` value"),
//     }
// }

match v.get(1){
    Some(i) => println!("{}",i),
    None => println!("None"),
}

println!("{}",v[0]);

// 索引 和 get 处理访问的越界的区别:索引: panic  get:返回 None

// 所有权和借用规则:不能在同一作用域内同时有可变的引用 和不可变 的引用
let mut v = vec![1,2,3,4,5];//可变
let first=&v[0];//不可变引用 
v.push(6);//可变借用
println!("{}",first);//不可变引用

// 遍历 vector的元素
for i in &v{
    println!("{}",i);
}
// 遍历同时更改值
let mut v = vec![100,120,130];
for i in &mut v{//要是可变引用
    *i+=50;//这里要解引用
}

// 示例存放多种类型
{
enum SpreadsheetCell{
Int(i32),
Float(f64),
Text(String),
}

let row=vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];

}


// ==========================================================================
// String 类型
let mut v=String::from("hello");
let mut v1=String::from("hello");
v.push_str(",world");//append
v.push_str(&v1);
v1.push('!');//把单个字符添加到字符串的末尾
let v2=v+&v1;//使用+号来连接
// 使用 format! 拼接字符串
let s=String::from("t1");
let s1=String::from("t2");
let s2=String::from("t3");

let s=format!("{}-{}-{}",s,s1,s2);//它不会取得任何参数的所有权
println!("{}",s);


// rust 的字符串不支持索引访问
let he="hello";
let s=&he[0..4];


// hashmap<k,v>
{
// key -values 存储方式
// use std::collections::HashMap; 引入才能使用
let hs:HashMap<String,i32>=HashMap::new();
// 在创建的时候没有数据,就要指定类型

let mut s=HashMap::new();
s.insert(String::from("hello"),10);//像这样 rust 就可以推导出它的类型了
// 另外一种方式创建 
let teams=vec![String::from("blue"),String::from("yellow")];
let initial_scores=vec![10,50];
let scores: HashMap<_,_>=
teams.iter().zip(initial_scores.iter()).collect();

for (k,v) in &scores{
    println!("{}: {}",k,v);
}
println!("{:?}",scores);
// hashmap 和所有权
/*
对于实现了 Copy trait 的类型， 值会被复制到 HashMap 中
对于拥有所有权的值，值会被移动，所有权会转移给 hashmap
如果将值的引用插入到 hashmap 中，值本身不会移动，在hashmap 有效期间，被
引用的值必须保持有效

*/

// 只丰 k 不对应任何值的情况下，才插入 entry() 返回值为枚举 
{
    let mut v = vec![1,2,3,4,5];
    let first=v[0];
    v.push(6);
    println!("{}",first);
}
}
}
```

> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=kXtqk&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 动态数组中：
**&** 与 **[ ]** **会直接返回元素的引用**。
索引访问会因为访问不存在的元素而发生 panic，而 get 方法会返回 None 不会发生 panic。
==注意所有权规则和借用规则：**不能在同一个作用域中同时拥有可变引用和不可变引用。**==
当我们需要修改可变引用的值，需要先对其解引用 *
要在动态数组中存储不同的元素类型时，可以枚举来；因为枚举中的所有变体都被定义为了同一种类型。
pop 方法移除并返回末尾的元素


---

> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=VCvMI&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 字符串：
Rust中的字符串使用了 UTF-8 编码。
rust 内置的string 编码格式是 utf-8，如果使用其它编码格式，就会报错，除非自己实现一个解码器
rust核心部分只有一种字符串类型：字符串切片 str ,它通常会以借用形式出现: & str
可以对那些实现了 Display trait 的类型调用 to_string() 方法；


```rust
let data="shenyang";
let s=data.to_string();//把字符串字面量转换成String
let s1="sy".to_string();//也可以直接应用于字面量，s1的类型为String
let s2=String::from("sy");
```

> **String::from 和 to_string 完成 相同的工作。**


> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f43c.svg#crop=0&crop=0&crop=1&crop=1&id=gZU5N&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)


> 更新字符串：


> 我们可以方便的使用 + 和 format! 宏来拼接字符串。（+ 方式会取得参数的所有权，而 format！不会取得参数的所有权）


```rust
let s=String::from("t1");
let s1=String::from("t2");
let s2=s+&s1;
//看下面方法知道：函数会取得 s 的所有权
==============================================
+ 号会调用一个方法
fn add(self , s: &str) -> String{...}
&s1能够调用add方法原因在于：编译器可以自动将 &String 类型的参数强制转换为  &str 类型。（解引用强制转换技术）
```

> push_str 添加一段字符串切片；push 添加单个字符


> 字符串不支持索引访问。


> 遍历方法： chars() 、 bytes()


> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=PxogP&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) HashMap
要使用它要引入当前作用域：
use std::collections::HashMap;
它的键必须要有相同的类型，它的值也必须要 相同的类型。
要记住可变与不可变原则；
作用 zip  、 iter 、collect 配合使用可以将动态数组转换为哈希映射：


```rust
let teams=vec![String::from("blue"),String::from("yellow")];
let initial_scores=vec![10,50];
let scores: HashMap<_,_>=
teams.iter().zip(initial_scores.iter()).collect();
```

> 所有权：实现了 Copy trait 的类型，它们的值会被简单的复制到哈希映射中，对于持有所有权的值，值会被转移，并且所有权会转移给哈希映射  ；将引用插入进去就不会转移所有权，指向的值要保证在哈希有效时自己也要是有效的。


> get 获取值，返回一个 Option。


> entry 方法检测一个键是否存在对应值，如果不存在就为它插入一个值。


## 错误处理

## 不可恢复错误与 panic!

**当 panic! 发生时，程序会默认从开始栈展开。可以在 Cargo.toml 文件中的 [profile] 区域添加  panic='abort'来改变 panic  默认行为从展开切换为 终止。**

```rust
//显示调用 panic!
panic!("发生了错误！！!");
```

> 回溯信息：
将环境变量 RUST_BACKTRACE 设置为一个非0值，从而获得回溯信息。


```rust
RUST_BACKTRACE=1 cargo run
```

> 带有调试信息的回溯：


> cargo build 或 cargo run 命令时，没有附带 - - release 标志，调试就是默认开启的


## 可恢复错误与 Result

```rust
enum Result<T,E> {
Ok(T),
Err(E),
}
//Result 枚举定义了两个变体：Ok 和 Err
```

```rust
  use std::fs::File;
    let f=File::open("hello.txt");
  let f=  match f{
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}",error),
    };
```

匹配不同类型的错误：

```rust
{
    use std::fs::File;
    let f=File::open("hello.txt");
  let f=  match f{
        Ok(file) => file,
        Err(error) => match error.kind(){
            std::io::ErrorKind::NotFound => match File::create("hello.txt"){
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}",e),
            },
            other_error => panic!("Problem opening the file: {:?}",other_error),
        },

        };
    };
```

### unwrap 和 expect （快捷方式）

> unwrap :
当 Result  的返回值是 Ok 变体时，它会返回 Ok 内部的值。返回值是 Err 变体时，它会替我们调用 panic! 宏。


```rust
use std::fs::File;
fn main(){
    let f=File::open("hello.txt").unwrap();
}
```

> expect：
它允许我 们在 unwrap 的基础上指定 panic! 所附带的错误提示信息。


```rust
fn main(){
    let f=File::open("hello.txt").expect("打开文件失败！！！");
}
```

### 传播错误

> 当执行失败的调用时，除了了可以函数中处理这个 错误，还可以将这个错误返回给调用者，这个过程就叫传播错误。


```rust
{
use std::io::{self,Read};
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {//将错误返回给调用者
    let f=File::open("hello.txt");

    let mut f=match f{
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s=String::new();

    match f.read_to_string(buf: &mut String){
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }//这里不加分号，表示发生错误返回错误，成功就忽略
}
}
```

### 传播错误的快捷方式： ? 运算符

```rust
fn read_username_from_file() -> Result<String, io::Error> {
    let mut f=File::open("hello.txt")?;
    let mut s=String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```

被 ？ 运算符接收的错误会隐式的被 from 函数处理，这个函数定义在标准库的 From trait中，用于在错误类型之间进行转换。

```rust
use std::{io,fs};
fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")

}
```

从文件中读取字符串是一 种相当常见的操作了，所以  rust  提供了一个函数 fs::read_to_string 用于打开文件，并且创建一个新 String,放入 String中并返回给调用者。。

**使用 ? 运算符的函数必须返回 Result、Option 或任何实现了 std::ops::Try 的类型。**

```rust
fn main() -> Result<(), Box<dyn Error>> {
   let f=File::open("hello.txt")?;
    Ok(())
}
//这里的Box<dyn Error>> 叫作 trait 对象，现在可以理解它为任何可能的错误类型
```

### 错误处理的指导原则

使用 panic!

1.  损坏状态并不包括预期中会偶尔发生的事情 
2.  随后的代码无法在出现损坏状态后继续正常运行 
3.  没有合适的方法 来将“处于损坏状态”这一信息编码至我们所使用的类型中 

如果错误是可预期的，就应该返回一个 Result 而不是调用 panic!

# 泛型、trait 与生命周期

### 泛型

```rust
// 泛型结构体,泛型也可以使用多个参数
struct Point<T,U> {
    x: T,
    y: T,
    v: U,
}

impl<T,U> Point<T,U> {
    fn x(&self) -> &T {
        &self.x
    }
    fn y(&self) -> &T{
        &self.y
    }
    fn v(&self) ->&U{
        &self.v
    }
}

===================================================
// 结构体泛型和方法泛型可以不同
#[derive(Debug)] // 这个注解可以让编译器自动生成 Debug 方法带有调试信息
struct p1<T>{
    x: T,
    y: T,
}
 impl<U> p1<U> {
    fn show(&self) {
        println!("fgfg");
    } 
}
```

使用泛型注意点：

```rust
// fn largest<T>(list: &[T]) -> T {
//     let mut largest = list[0];

//     for &item in list.iter() {
//         if item > largest {
//             //报错它这个泛型不能适用于所有可能的类型，比如说字符串，或者自己定义的结构体，而C++不会报错，但会在执行的时候报错
//             largest = item;
//         }
//     }
//     largest
// }
```

在函数中定义泛型的时候，泛型放置在函数签名中通常用于指定参数和返回值类型的地方。

如果泛型定义的函数不适用于所有类型，就会报错！！！

为泛型结构体定义方法的时候，也要在 impl 后面加泛型参数和结构体的一样。

> 泛型的性能问题：
**单态化：是一个在编译期将泛型代码转换为特定代码的过程，它会将所有使用过的具体类型填入泛型参数从而得到能具体类型的代码。所以使用泛型并不会性能问题。**（不需要为运行时付出任何的代价）


## trait 定义共享行为（接口）

和其他语言的接口功能类似，但也有不同的地方。

```rust
// trait : 定义共享行为，和其他语言的接口类似，但也有一些区别
pub trait Summary {
    fn summarize(&self) -> String;
    fn show(&self);
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}


// 在类型上实现 trait
impl Summary for NewsArticle {//要实现一个 trait， 就要实现它里面所有方法
    fn summarize(&self) -> String {
        format!("{},by {} ({})", self.headline, self.author, self.location)
    }
    fn show(&self) {
        println!("{},by {} ({})", self.headline, self.author, self.location);
    }
}
```

pub  trait  名称

实现语法： impl  trait名 for 类型名

> 限制：
只有当 trait 或 类型定义于我们的库中时，我们才能为该类型实现对应的 trait。
我们不能为外部类型实现外部 trait (孤儿规则)


### 默认实现

```rust
// 默认实现
pub trait sy{
    fn show(&self){
        println!("show ");//若没有实现就会使用默认实现
    }
    fn show1(&self, i:i32);

}

pub struct sy1{
    name:String,
    age:i32,
}

impl sy for sy1{
    fn show(&self){
        println!("show {}",self.name);
    }
    fn show1(&self,i:i32){
        println!("show {}",i);
    }
}

fn main(){
   let l=sy1{name:"zhangsan".to_string(),age:18};
   let l1=sy1{name:"lisi".to_string(),age:19};
   l.show();//有实现就会调用自己的实现
   l1.show();  
}
```

为某个类型实现 trait 时，可以选择保留或重载每个方法的默认行为！！

实现 trait 时，没有实现对应的方法也可以调用默认实现的方法。

#### 使用 trait 作为参数

```rust
// 使用 trait 作为参数
pub trait syhui{
    fn show2(&self){
        println!("show");
    }
}
// item 可以是任何实现了 syhui trait 的类型
pub fn notify(item :impl syhui){
    item.show2();
}
```

### trait 约束

感觉就是通过 trait 来限制函数参数的范围！！！

```rust
// item 可以是任何实现了 syhui trait 的类型
pub fn notify(item :impl syhui){
    item.show2();
}
//和上面那种功能等价，只是多了一个参数
pub fn notify1<T: syhui> (item:T , item1:T){
    item.show2();
    item1.show2();
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=XiZxK&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)可以通过 + 号来指定多个 trait 约束

```rust
// 使用 + 号来实现多个 trait约束
pub fn notify2<T: syhui + sy>(item:T){
    item.show2();
}

pub fn notify3(item: impl syhui + sy){
    item.show2();
}
```

两种写法：impl trait 适合短小的示例，而 trait 约束适用于复杂情形

### 使用 where 从句来简化 trait 约束

```rust
fn some<T : sy+syhui, U: sy5+syhui> (t: T, t1 : U) -> i32{
    t.show2();
    t1.show2();
    t.show1(1);
    t1.show3();
    45
}
// 使用 whrer 从句简化
fn some1<T,U> (t : T , t1 : U) ->i32 
 where T:sy+syhui,  //注意这里有个逗号
 U:sy5+syhui {//最后一个这里不用加逗号
        t.show2();
        t1.show2();
        t.show1(1);
        t1.show3();
        45
    }
```

```rust
语法：
fn  函数名<T,U> (t :T , u :U) -> 返回值类型
 where T : trait1 + trait2+...  ,
       U : trait1 + trait2+.. {
           ... 函数体
}
```

### 返回实现了 trait 的类型

```rust
pub trait sy5{
    fn show3(&self);
}

pub struct sy6{
    name:String,
    age:i32,
}

impl sy5 for sy6{
    fn show3(&self){
        println!("show {}",self.name);
    }
}
fn returntrait() -> impl sy5{
    sy6{
        name:"sy6".to_string(),
        age:18,
    }
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&id=VgAwJ&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：错误写法

```rust
pub trait sy5{
    fn show3(&self);
}
pub struct sy6{
    name:String,
    age:i32,
}
pub struct sy7{
    name:String,
    age:i32,
}
impl sy5 for sy6{
    fn show3(&self){
        println!("show {}",self.name);
    }
}

impl sy5 for sy7{
    fn show3(&self){
        println!("show {}",self.name);
    }
}
fn returntrait(sw : bool) -> impl sy5{
   if sw{

    sy6{
        name:"sy6".to_string(),
        age:18,
    }
    } else {
         sy7{
        name:"sy7".to_string(),
        age:18,
    }
}
   }
```

### 使用 trait 约束来有条件地实现方法

```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}
// 没有任何限制
impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self {
            x,
            y,
        }
    }
}
//只有实现了 PartialOrd(用于比较) 与 Display(用于打印) 的类型，才会实现 cmp_display方法
impl<T : Display + PartialOrd> Pair<T>{
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

```rust
// 也可以为实现了某个 trait 的类型条件地实现另外一个 trait ,对满足 trait 约束的所有类型实现 trait 也称作覆盖实现
 impl<T : Display> ToString for T{
    。。。
 }
//我们可以为任何实现了 Display trait 的类型调用 ToString trait 里面的 to_string 方法
```

# 生命周期

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d6.svg#crop=0&crop=0&crop=1&crop=1&id=GLSMb&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：

-  在rust中每个引用都有自己的生命周期，它对应着引用保持有效性的作用域。 
-  生命周期最主要的目标是避免悬垂引用（值在离开作用域时使用指向它的引用） 
-  rust 中不允许空值存在 

## 借用检查器

rust 编译器有一个借用检查器，它用于比较不同的作用域并确定所有借用的合法性。

**生命周期的标注不会改变任何引用的生命周期长度。**

```rust
语法：
'小写字符（通常使用小写的）
eg:
&i32   引用
&'a i32  拥有显式生命周期的引用
&'a mut i32  拥有显式生命周期的可变引用
标注是为描述多个泛型生命周期参数之间的关系
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f440.svg#crop=0&crop=0&crop=1&crop=1&id=kmGFJ&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：对比

```rust
// 生命周期错误
// fn longest(x : &str, y : &str) -> &str {
//     if x.len() > y.len() {
//         x
//     } else {
//         y
//     }
// }
//不会报错，这标注说明了参数和返回值它们三个的引用要拥有相同的生命周期 'a (或是两个参数的存活时间不能短于给定的生命周期 'a )
fn longes1<'a>(x : &'a str, y : &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
//当具体的引用传入函数时，泛型生命周期 'a 会被具体化为 x 与 y 两者中生命周期较短的那一个。我们将返回的引用也标为了 'a ，在具化后的生命周期范围内也是有效的
```

**我们在函数签名中指定生命周期参数时，我们并没有改变任何传入值或返回值的生命周期。**

---

**当返回一个引用时，返回类型的生命周期参数必须要与其中一个参数的生命周期参数相匹配。**

```rust
fn longes1<'a>(x : &'a str, y : &str) -> &'a str {
        x   //我们忽略y 生命周期的标注
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=SMVUw&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 指定生命周期的方式取决于函数的具体实现功能。！！！！

### 结构体定义中的生命周期标注

**在结构体中存储引用，需要为结构体定义中的第一个引用都添加生命周期标注。**

```rust
struct import<'a> {
    part : &'a str,
}
//结构体实例的存活时间不能超过存储在字段 part 中的引用的存活时间
fn main() {
    let novel=String::from("Call me Ishmael. Some years ago...");
    let first_sentence=novel.split('.').next().expect("Could not find a '.'");//截取第一个 . 的位置以前的字符串
    let i =import{
        part: first_sentence,
    };
}
```

## 生命周期省略

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d1.svg#crop=0&crop=0&crop=1&crop=1&id=lidqf&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：**任何引用都有一个生命周期，并且需要为使用引用的函数或结构体指定生命周期参数。**

> 下面这个函数没有标注生命周期却可以通过编译


```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

> 写入 rust 引用分析部分的模式也就是生命周期省略规则。就是编译器会考虑一些场景，我们无需去遵守，当我们的代码符合那些模式的时候，就不需要生命周期标注。


**输入生命周期：函数参数或方法参数中的生命周期；**

**输出生命周期：返回值的生命周期**

> 编译器目前使用的三条规则来计算引用的生命周期：


-  **每一个引用参数都会拥有自己的生命周期参数** 
-  **当只存在一个输入生命周期参数时，这个生命周期会被赋予给所有输出生命周期参数** 
-  **当拥有多个输入生命周期参数，而其中一个是 &self 或&mut self 时，self 的生命周期会被赋予给所有的输出生命周期参数** 

### 方法定义中的生命周期标注

```rust
struct import<'a> {
    part : &'a str,
}
impl<'a> import<'a> {
    fn level(&self) -> i32{
        3
    }
}
```

**声明在 impl 及类型名称之后的生命周期是不能省略的。**

根据规则我们可以不用为 self 引用标注生命周期。

### 静态生命周期

```rust
'static 生命周期
```

**'static 它表示整个程序的执行期。所有的字符串字面量都拥有这个静态生命周期**

综合示例：

```rust
fn longest_with_an_announcement<'a,T>(x: &'a str, y: &'a str, ann: T) -> &'a str
where
    T: Display,
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

# 测试简要

```rust
测试函数需要使用 test 属性
在函数上加上 #[test] 属性
使用 cargo test 命令运行所有测试

==================================================
pub fn add_two(a:i32) -> i32{
    a+2
}
// 断言 assert
// assert!  assert_eq!   assert_ne! 可以添加自定义信息
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        let result = 2 + 2;
        assert_eq!(result, 4);
    }
use super::*;//要引入进来才能使用
    #[test]
    fn it_add_two(){
        assert_eq!(4,add_two(2),"执行有失败吗？");
        //测试add_two 执行的结果是否等于 4
    }

    #[test]
    #[should_panic] //来指定应该发生 panic
    fn it_cmp(){
        panic!("this is a panic!");
    }
}

// should_panic 属性：发生了 panic 就测试成功，没有就测试失败
// 添加可选的 expect 属性

pub fn cmp(i:i32 ) ->bool{
    if i<0 {
        panic!("i must be greater than 0");
    }
    i>100
}

// cargo test 匹配的名 
// 会自动匹配带有名字字段的测试

// 忽略测试
// #[ignore]  添加字段就会忽略
// cargo test -- --ignored 运行有忽略属性的测试

// rust 中允许测试私有函数

===================================================
assert! 它可以确保测试中某些条件的值为 true，如果为 false 就会调用  panic!()

assert_eq! 宏和 assret_ne!宏，用于比较判断两个参数相等或不相等

在自定义的结构体或枚举的定义的上方添加 #[derive(PartialEq, Debug)] 标注来自动实现这两个 trait



should_panic 检查 panic
这个属性标记了测试函数会在代码发生 panic 时顺利通过，而不发生 panic 时失败
可以选参数 expected ：它检查 panic 发生时输出的错误提示信息是否包含了指定的文字 
#[test]
#[should_panic(expected="Guess value must be less than or equal to 100")]
测试函数


===================================================
Result<T,E> 来编写测试，它运行失败时会返回一个 Err 值而不panic

#[cfg(test)]
mod tests{
#[test]
fn it_works() -> Result((),String){
    let result = 2 + 2;
    assert_eq!(result, 4);
    Ok(())
}
}

================================================
rust 会默认使用多线程来并行执行测试
可以使用  cargo test -- --test-threads=1
来将线程数量限制为 1

如果希望测试通过时也将值打印出来，可以使用：
cargo test -- --nocapture 来禁用截获功能

cargo test 测试函数名   //只运行部分指定的测试
注意：我们不能指定多个参数来运行多个参数，只有第一个参数才会生效

想要运行多个测试可以用名称匹配来实现
cargo test add 
运行所有测试名称中带有 add 的测试

ignore 属性来标记忽略某些测试
在#[test] 下面标记 #[ignore]
运行那些被忽略的测试
cargo test -- --ignored

==============================================
单元测试和集成测试
```

# I/O 项目

> 跟着书写一个简易的 grep 工具


> ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f34e.svg#crop=0&crop=0&crop=1&crop=1&id=UCi5b&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：项目关注点分离


-  将程序拆分为 main.rs 和 lib.rs ，并将实际的业务逻辑放入 lib.rs 
-  当命令行解析逻辑相对简单时，将它留在 main.rs 中也可以 
-  当命令行解析逻辑变得复杂时，需要将它从 main.rs 提取到 lib.rs 中 

> main.rs 负责运行程序，而 lib.rs 负责处理真正的业务逻辑。


大多数终端都提供两种输出：

-  用于输出一般信息的标准输出(stdout) 
-  用于输出错误提示信息的标准错误(stderr) 

```bash
>  文件名
重定向 : 告诉终端将打印信息输出到指定文件而不是终端上面
```

```rust
eprintln!("{}",err);
eprintln!宏用来向标准错误打印信息
```

src/main.rs

```rust
/*
 * @=^=: ===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^
 * @Autor: 沈扬
 * @Date: 2022-04-04 09:04:55
 * @FilePath: \VScodeProjects\Rustlearn\minigrep\src\main.rs
 * @LastEditTime: 2022-04-04 13:10:35
 * @LastEditors: shenyang
 * @symbol_=custom_string_obkoro1: ..............因为不确定才有了期待...................................
 * @^=^: ===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^
 */

// 用于读取命令行参数值std::env::args() 返回一个命令行参数的迭代器（iterator）
use std::env;
use std::process;

// use minigrep::Config;//导入错误原因暂时不知
// 所先放在下面接着用

use std::{error::Error,fs};
    pub struct Config {  
        query : String,
        filename : String,
    }
 

    fn run(config:Config) -> Result<(),Box<dyn Error>>{
    
        let contents = fs::read_to_string(config.filename)?;//注意这里有个问号传播错误
    
        for line in search(&config.query, &contents){
            println!("{}",line);
        }
        Ok(())
        
        }
    






impl Config {
    fn new(args: &[String]) -> Result<Config, &'static str>{
        if args.len() <3{
           return  Err("你没有输入参数或者是参数不足够！！！")
        }
       
        Ok (Config {
            query: args[1].clone(),
            filename: args[2].clone(),
        })  
    }

}


fn main() {
    // collect() 方法将迭代器转换为一个集合例如： Vec。
   let args:Vec<String> =env::args().collect();

//    在程序退出时向调用者返回 非0的状态码是一种惯用的信号，它表明
// 当前程序的退出是由于某种错误状态导致的。
// unwrap_or_else （） 方法：定义在标准库的 Result<T,E>  中 ，它的值为OK时
// 行为和 unwrap 相同，当返回 Err 时，它会调用闭包中编写的代码。闭包的参数是写在两条竖线之间
// process::exit(1) 函数会立刻终止程序的运行，并将我们指定的错误码返回给调用者。




    let config=Config::new(&args).unwrap_or_else(|err|{
        eprintln!("Problem parsing arguments: {}",err);
        std::process::exit(1);
    });




    
//    将获取到的值存入变量
    // let query = &args[1];
    // let filename = &args[2];
    // println!("query: {}", query);
    // println!("filename: {}", filename);

    


// // 开始读取文件
//     let contents = fs::read_to_string(config.filename)
//         .expect("Something went wrong reading the file");
//     println!("With text:\n{}", contents);
    

// 处理传播过来的错误
// run(config);

if let Err(e) =run(config){
    eprintln!("Application error: {}",e);
    std::process::exit(1);
}
}


pub fn search<'a> (query: &str , contents: &'a str) -> Vec<&'a str> {
    // 创建一个动态数组，将匹配的文本存储进去 
    let mut results = Vec::new();
    for line in contents.lines(){
        if line.contains(query){
            results.push(line);
        }
    }
    results   //返回匹配到的所有文本
}
// 使用结构体重构
// struct Config {
//     query : String,
//     filename : String,
// }

// impl Config {
//     // fn new(args: &[String]) -> Config{
//     //     if args.len() <3{
//     //         panic!("你没有输入参数或者是参数不足够！！！")
//     //     }
        
//     //     Config{
//     //         query: args[1].clone(),
//     //         filename: args[2].clone(),
//     //     }
//     // }
//     // 我们倾向于使用 Panic! 来暴露程序的内部问题而非用法问题。
//     // 改进  使用 Result 表明结果成功还是失败
//     fn new(args: &[String]) -> Result<Config, &'static str>{
//         if args.len() <3{
//            return  Err("你没有输入参数或者是参数不足够！！！")
//         }
       
//         Ok (Config {
//             query: args[1].clone(),
//             filename: args[2].clone(),
//         })  
//     }

// }



//改进为与 结构体关联的new函数
// fn parse_config(args:&[String]) -> Config{
//     Config {
//         query: args[1].clone(),
//         filename: args[2].clone(),
//     }
// }


// fn parse_config(args:&[String]) -> (&str,&str){
//     let query =&args[1];
//     let filename =&args[2];
//     (query,filename)
// }


// 从 main 函数中除了配置解析和错误处理之外的所有逻辑都提取到单独的 run 函数中 
// 从 run 函数中返回错误
// use  std::error::Error;
// () 是空元组
// Box<dyn Error> 它会 返回一个实现了 Error trait 的类型，我们不需要指定具体是什么类型
// 意味着我们可以在不同的错误场景下返回不同的 错误类型， dyn 意味着动态的意思
// fn run(config:Config) -> Result<(),Box<dyn Error>>{

// // 开始读取文件
// // let contents = fs::read_to_string(config.filename)
// // .expect("Something went wrong reading the file");
// // println!("With text:\n{}", contents);


// let contents = fs::read_to_string(config.filename)?;//注意这里有个问号传播错误
// Ok(())


// }
```

src/lib.rs

```rust
/*
 * @=^=: ===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^
 * @Autor: 沈扬
 * @Date: 2022-04-04 09:31:23
 * @FilePath: \VScodeProjects\Rustlearn\minigrep\src\lib.rs
 * @LastEditTime: 2022-04-04 13:06:09
 * @LastEditors: shenyang
 * @symbol_=custom_string_obkoro1: ..............因为不确定才有了期待...................................
 * @^=^: ===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^^^===^
 */
use std::{error::Error,fs};



    pub struct Config {
        query : String,
        filename : String,
        pub case_sensitive : bool,//切换区分是否忽略大小 写
    } 
    fn run(config:Config) -> Result<(),Box<dyn Error>>{

        // let contents = fs::read_to_string(config.filename)?;//注意这里有个问号传播错误

        // for line in search(&config.query, &contents){
        //     println!("{}",line);
        // }
        // Ok(())

        // 改进功能，是否区分大小写
        let contents = fs::read_to_string(config.filename)?;
        let results = if config.case_sensitive {
            search(&config.query, &contents)//区分大小写
        } else {
            search_case_insensitive(&config.query, &contents)//不区分大小写
        };

        for line in results {
            println!("{}",line);

        }
        Ok(())
    }
use std::env;

// env::var 函数会返回一个 Result 作为结果，只有环境变量被设置时，这个结果才会包含Ok 的变体，否则返回Err。
// 我们使用了 Result 的 is_err 方法来检查结果是否为错误，如果 CASE_INSENSITIVE 被设置为了某个值那么就会返回假，也是不区分大小写搜索
// 使用： env:CASE_INSENSITIVE=1   cargo run to poem.txt
// std::env 模块中还有很多用于处理环境变量的 实用功能
impl Config {
    fn new(args: &[String]) -> Result<Config, &'static str>{
        if args.len() <3{
           return  Err("你没有输入参数或者是参数不足够！！！")
        }
       let case_sensitive=env::var("CASE_INSENSITIVE").is_err();
        Ok (Config {
            query: args[1].clone(),
            filename: args[2].clone(),
        })  
    }

}
// 编写 搜索函数
// 使用  lines 方法逐行遍历文本 , 它会返回一个迭代器
// contains 方法检查每一行是否包含查询字符串
pub fn search<'a> (query: &str , contents: &'a str) -> Vec<&'a str> {
    // 创建一个动态数组，将匹配的文本存储进去 
    let mut results = Vec::new();
    for line in contents.lines(){
        if line.contains(query){
            results.push(line);
        }
    }
    results   //返回匹配到的所有文本
}

//用于大小写搜索 ： 思路是： 把大写转换为小写，就可以忽略大小写，来达到目的
// 由上面结构体的 bool 字段控制 不分大小写调用这个搜索函数，分大小写就用上面那个 search 函数
pub fn search_case_insensitive<'a> (query: &str , contents : &'a str) -> Vec<&'a str> {

    let query=query.to_lowercase();//把字符串转换为小写
    let mut results=    Vec::new();

    for line in contents.lines() {
        results.push(line);
    }
results
}

#[cfg(test)]
mod tests {
    use super::*;
    #[test]
    fn one_result() {
        let query = "duct";//结果为这个
        let contents = "\

        Rust:
        safe, fast, productive. 
        Pick three.";
            assert_eq!(
                vec!["safe, fast, productive."],
                search(query, contents)
            );
    }
}
// 为不区分大小写 的 search 函数编写一个会失败的测试

#[#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn case_sensitive(){
        let query="duct";

        let contents= "\
        Rust:
        safe, fast, productive.
        Pick three.
        Duct tape.";

        assert_eq!(
            vec!["safe, fast, productive."],
            search(query, contents)
        );

    
    }
#[test]
fn case_insensitive(){

    let query="rUsT";

    let contents= "\
    Rust:
    safe, fast, productive.
    Pick three.
    Trust me.";

    assert_eq!(
        vec!["Rust:","Trust me."],
        search_case_insensitive(query, contents)
    );
}
}
```

# 函数式语言特性：迭代器与闭包

## 闭包

> 闭包：可以捕获其所在环境的匿名函数
可以存入变量或者作为参数传递给其他函数的匿名函数


**语法：**

```rust
let bibaoa= |参数1 ， 参数2 | -> 返回类型 {
    函数体;
    返回值
};  //注意末尾有个冒号
多个参数之间用逗号分隔
```

```rust
 let bp= |num1, num2| -> i32 {
        println!("{}",num1);
        println!("{}",num2);
        num1+num2
    };
    bp(100,200);//闭包调用
```

> 闭包一般不需要指定参数和返回值的类型，因为编译器能够推断出大多数变量的类型


**如果闭包只有一条语句，那么可以省略花括号：**

```rust
let bp1=|x| x*x;  //这里返回值也要加分号
    bp1(100);
```

不能使用两种不同的类型调用同一个**需要类型推导的闭包**

```rust
 let bp1=|x| x*x;
    bp1(100);
    bp1(String::from("hello"));//报错，因为上面已经被推导成了整数类型
```

### 使用泛型参数和 Fn trait 来存储闭包

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d6.svg#crop=0&crop=0&crop=1&crop=1&id=I8YFk&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 将闭包存入结构体中，我必须要明确指定闭包的类型，因为结构体各个字段在定义时必须要确定

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d6.svg#crop=0&crop=0&crop=1&crop=1&id=W10HJ&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) 第一个闭包实例都有它自己的匿名类型，两个闭包拥有完全相同的签名，它们的类型也被认为是不一样的

**所有闭包都至少实现了 Fn 、 FnMut 、 FnOnce 中的一个 trait。**

(函数也可以实现这三个 Fn trait)

```rust
struct Cacher<T> 
where T: Fn(i32) -> i32 {
   //Fn(i32) -> i32 这个添加代表了闭包参数和闭包返回值的类型 
    calculation: T,
    value: Option<i32>,
}
//where 约束了这个 T 代表一个使用 Fn trait 的闭包
```

示例：

```rust
struct Cacher<T> 
where T: Fn(i32) -> i32 {
    calculation: T,
    value: Option<i32>,
}

// p 364 示例
impl<T> Cacher<T>
where T: Fn(i32) -> (i32){

    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            value: None,
        }
    }

    fn value(&mut self, arg: i32) -> i32 {
        match self.value {
            Some(v) => v,
            None => {
                let v =(self.calculation)(arg);
                self.value = Some(v);
                v
            }
        }
    }

}
```

#### 使用闭包捕获上下文环境

==闭包可以使用定义在同一个作用域中的变量 x==(这个功能是函数所没有的)

```rust
fn main(){

let x = 4;
    let equal_to_x= |z| z == x; //x 不是它的参数，闭包也可以使用定义在同一个作用域中的变量x
    let y = 4;
    assert!(equal_to_x(y));
        
}
```

这一特性只能用于闭包。

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=QqcaJ&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：闭包可以通过三种方式从它们环境中捕获值: **获取所有权、可变借用、不可变借用**

> **FnOnce**  获得所有权，闭包不能多次获取并消耗掉同一变量的所有权，所以它只能被调用一次。
**FnMut** 可变地借用值
**Fn** 可以从环境不可变的借用值


当我们创建闭包的时候，rust 会基于闭包从环境中使用值的方式来自动推导它需要使用的 trait，所有闭包都自动实现了 FnOnce

> 如果希望强制获取环境中的所有权，可以在参数列表前添加关键字 move ；


```rust
// 使用 move
    let x =vec![1,2,3];
    let equal_to_x = move |z| z == x;
    println!("{:?}", x);//错误因为使用了move x 的所有权和值已经被移动到闭包中
    let y=vec![1,2,3];
    println!("{:?}", equal_to_x(y));
```

这里使用动态数组是因为：**整形只会被复制而不会被移动！！！**

> 大部分情况下，需要指定某一个 Fn 系列的 trait 时，可以先尝试使用 Fn trait, 编译器会根据闭包体中的具体情况来告诉你是否需要 FnMut 或 FnOnce。


#### 迭代器处理元素序列

> 在rust 中迭代器是惰性的，创建迭代器后，除非你主动调用方法来消耗并使用迭代器，否则它们不会产生任何的实际效果。


```rust
  let v1 = vec![1,2,3];
    let vi_iter=v1.iter();
    for val in vi_iter{
        println!("{}",val);
    }
```

#### Itertor triat 和 next 方法

==所有的迭代器都实现了标准库中的 Iterator triat。==

```rust
pub tracit Iterator {
    
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
    ...
}
//为了实现它我们必须要定义一个具体的 Item 类型，Item 类型将会是迭代器返回元素的类型。
//它要求实现者手动实现一个方法：next 方法，它会在每次被调用时返回一个包裹在 Some 中的迭代器元素，并在迭代结束时返回 None
```

```rust
#[test]
fn iterator_demonstration() {
    let v1 = vec![1, 2, 3];
    let mut v1_iter = v1.iter();
    assert_eq!(v1_iter.next(), Some(&1));
    assert_eq!(v1_iter.next(), Some(&2));
    assert_eq!(v1_iter.next(), Some(&3));
    assert_eq!(v1_iter.next(), None);
}
```

这些调用 next 的方法也被称为 **消耗适配器**

```rust
#[test]
fn iterator_sum() {
    let v1 = vec![1, 2, 3];
    let v1_iter = v1.iter();

    let total: i32 = v1_iter.sum();

    assert_eq!(total, 6);
}
//上面在调用 sum 的过程中获取了 v1_iter 的所有权,因此这个迭代器无法被后面的代码继续使用
```

#### 生成其他迭代器的方法

> 迭代器适配器：可以将已有的迭代器转换成其他不同类型的迭代器。


```rust
    let v1: Vec<i32> = vec![1, 2, 3];
    let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();
//Vec<_> 这个是代表 推导类型
    assert_eq!(v2, vec![1, 2, 3]);
```

**collect 方法它会消耗迭代器并将结果集收集到某种集合数据类型中。**

#### 使用闭包捕获环境

迭代器 filter 方法会接收一个闭包作为参数，这个闭包会在遍历迭代器中的元素时返回一个布尔值，而每次遍历的元素在闭包返回 true 时才会被包含在 filter 生成的新迭代器中。

```rust
// 使用闭包捕获环境
#[derive(PartialEq, Debug)]
struct Shoe {
    size: u32,
    style: String,
}

fn shoes_in_my_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter().filter(|s| s.size == shoe_size).collect()
}

#[test]
fn filters_by_size() {
    let shoes = vec![
        Shoe {
            size: 10,
            style: String::from("sneaker"),
        },
        Shoe {
            size: 13,
            style: String::from("sandal"),
        },
        Shoe {
            size: 10,
            style: String::from("boot"),
        },
    ];

    let in_my_size = shoes_in_my_size(shoes, 10);

    assert_eq!(
        in_my_size,
        vec![
            Shoe {
                size: 10,
                style: String::from("sneaker")
            },
            Shoe {
                size: 10,
                style: String::from("boot")
            },
        ]
    )
}
```

#### 使用 Iterator trait 来创建自定义迭代器

==需要提供一个 next 方法的定义就可以实现 Itreator triat==

```rust
struct Counter {
    count: u32,
}
impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}

impl Iterator for Counter {
    //将迭代器的关联类型指定了 u32
    type Item = u32;
    fn next(&mut self) -> Option<Self::Item> {
        self.count += 1;
        if self.count < 6 {
            Some(self.count)
        } else {
            None
        }
    }
}

#[test]
fn calling_next_directly() {
    let mut counter = Counter::new();

    assert_eq!(counter.next(), Some(1));
    assert_eq!(counter.next(), Some(2));
    assert_eq!(counter.next(), Some(3));
    assert_eq!(counter.next(), Some(4));
    assert_eq!(counter.next(), Some(5));
    assert_eq!(counter.next(), None);
}
```

> 循环和迭代器的性能：
迭代器是 rust 语言中的一种零开销抽象。
使用这些抽象时，不会引入额外的运行时开销。


## Cargo 及 crates.io

常用配置两种：

-  执行 cargo build 时使用 dev 配置， 以及执行 cargo build  - - release 时使用的 release 配置。 
-  ==dev==配置中的默认选项适合在开发过程中使用 
-  ==release==配置中的默认选项则适合在正式发布时使用 

==**编写有用的文档注释**==  : 三斜线  / / /

并且可以 markdown 语法来格式化内容。

```rust
/// 将传入的数字加 1 
/// 
/// # Examples
/// 
/// ```rust
/// let arg=5;
/// let answer=my_crate::add_one(arg);
/// 
/// assert_eq!(6,answer);
/// ```

pub fn add_one(x:i32) -> i32{
    x+1
}
fn main() {
   println!("文档注释!!")
}
```

> 运行 **cargo doc** 命令来生成文档  会生成 Html 文件
生成在 target/doc 路径下




> 还有一种文档注释： // !
它可以 为包裹当前注释的外层条目添加文档，这种注释常用在包的根文件


```rust
//! # my crate
//! my_crate 是一系列工具的集合
//! ..
//! ....
/// 将传入的数字加 1 
/// 
/// # Examples
/// 
/// ```rust
/// let arg=5;
/// let answer=my_crate::add_one(arg);
/// 
/// assert_eq!(6,answer);
/// ``
```

# 智能指针

> 智能指针起源于 C++
引用是用 & 符号表示，会借用它所指向的值。


> 智能指针是一些数据结构，它们的行为类似于指针但拥有额外的元数据和附和功能


> 引用和智能指针之间的区别：引用是只借用数据的指针；而大多数智能指针本身就拥有它们指向的数据。


#### 常见的智能指针

1.  **Box** :在 heap 内存上分配值 
2.  **Rc** :启用多重所有权的引用计数类型 
3.  **Ref**** 和 RefMut** , 通过 RefCell 访问： 在运行时而不是编译时强制借用规则的类型 

==在编译的时候 ， rust  需要知道一个类型所占的空间大小。==

递归类型的大小无法在编译时确定。

> Cons List：链接列表
（函数式编程语言常见）
链接列表的每一项都包含了两个元素：当前项的值及下一项。
链接列表的最后一项是一个 Nil （不包含下一项的特殊值，来做列表的终止标记，Nil 并不是一个无效或缺失的值）


==大部分情况下 **Vec** 都是一个好的选择，链接列表它并不常用==

```rust
 enum List{
    Cons(i32,Box<List>),
    Nil,
 }

 use crate::List::{Cons,Nil};

fn main() {
    
let b=Box::new(5);
println!("{}",b);

let list=Cons(4,
    Box::new(Cons(5,
    Box::new(Cons(6,
    Box::new(Nil))))));

}
```

```rust
let x= 5;
let y= &x;
assert_eq!(5,x);
// 数值和引用是两种不同的类型
assert_eq!(5,*y);
//如果 y 不加解引用就会报错，因为上面那句话的原因
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=d4Hwp&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：把 Box

 当成引用来操作

```rust
let x= 5;
// let y= &x;
let y=Box::new(x);
assert_eq!(5,x);
// 数值和引用是两种不同的类型
assert_eq!(5,*y);
```

定义我们自己的智能指针：

```rust
 use crate::List::{Cons,Nil};

struct MyBox<T> (T);

impl<T> MyBox<T> {
    fn new(x:T) -> MyBox<T>{
        MyBox(x)
    }

}

 
fn main() {
    
    let x= 5;
    // let y= &x;
    let y=MyBox::new(x);
    assert_eq!(5,x);
    // 数值和引用是两种不同的类型
    assert_eq!(5,*y);    
}
```

> 问题：上面代码报错原因：不知道如何去解引用 MyBox
解决办法：实现 Deref trait


```rust
 use std::ops::Deref;
 
impl<T> Deref for MyBox<T>{
    type Target=T;
    fn deref(&self) ->&T  {
        &self.0
    }

}
======================================
//*y 会被隐匿地展开为： *(y.deref())
```

#### 函数和方法的隐式解引用转换

> 解引用转换：当某个类型 T 实现了 Deref trait 时，它能够将 T 的引用转换为 T 经过 Deref 操作后生成的引用。我们将某个特定类型的值引用作为参数传递给函数或方法，但传入的类型与参数类型不一致时，解引用转换就会自动发生


```rust
fn main(){
let m=MyBox::new(String::from("Rust"));
hello(&m);
// 如果没有解引用转换功能，上面调用的代码就要这样写
hello(&(*m)[..]);
hello("Rust");

}
    
fn hello(name: &str) {
    println!("{}",name);
}
```

#### 解引用转换与可变性

1.  使用 Deref trait 能够重载不可变引用的 * 运算符。 
2.  使用 DerefMut trait 能够重载可变引用的 * 运算符 

满点下面3种类型情形时执行解引用转换：

-  当 T：Deref<Target=U>时，允许 &T 转换为 &U 
-  当 T：DerefMut<Target=U>时，允许&mut T 转换为 &mut U 
-  当 T：Deref<Target=U>时，允许&mut T 转换为 &U 

==rust 会将一个可变引用自动地转换为一个不可变引用。但这个过程不会逆转，**不可变引用永远不可能转换为可变引用**。==

### Drop trait （目前理解析构函数）

Drop trait 要 求实现一个接收 self 可变引用作为参数的 drop 函数。

```rust
struct CustomSmartPointer{
     data:String,
 }

 impl Drop for CustomSmartPointer{
    fn drop(&mut self){
        println!("Dropping CustomSmartPointer with data '{}'",self.data);
    }
 }


fn main() {
    
let c=CustomSmartPointer{
    data :String::from("my stuff")
};

let d=CustomSmartPointer{
    data:String::from("other stuff")
};
println!("CustomSmartPointres created.")
}
```

```rust
CustomSmartPointres created.
Dropping CustomSmartPointer with data 'other stuff'
Dropping CustomSmartPointer with data 'my stuff'
```

==变量的丢弃顺序与创建顺序相反。==

#### 使用 std::mem::drop 提前丢弃值

rust 并不允许我们手动调用 Drop trait 的 drop 方法；但可以调用标准库中的 std::meme::drop 函数来提前清理某个值。

==不允许我们手动调用 drop 函数（析构函数）==

要使用就使用 std::mem::drop 函数

```rust
struct CustomSmartPointer{
     data:String,
 }

 impl Drop for CustomSmartPointer{
    fn drop(&mut self){
        println!("Dropping CustomSmartPointer with data '{}'",self.data);
    }
 }


fn main() {
    
let c=CustomSmartPointer{
    data :String::from("my stuff")
};

let d=CustomSmartPointer{
    data:String::from("other stuff")
};
drop(c);
println!("CustomSmartPointres created.")
}
```

注意区别：![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=PPqyg&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)

```rust
c.drop();  //调用的是 drop 函数
drop(c);  //可以调用的是 std::mem:drop 函数
```

### 基于引用计数的智能指针 Rc

> Rc 的类型支持多重所有权， Rc  是 Reference countiing （引用计数）的缩写
Rc 只能被用于单线程场景中


```rust
 enum List{
     Cons(i32,Box<List>),
     Nil,
 }
use crate::List::{Cons,Nil};
fn main() {
    
    let a =Cons(5,Box::new(Cons(10,Box::new(Nil))));
    let b=Cons(3,Box::new(a));
    let c=Cons(4,Box::new(a));
}
//报错原因：Box<T> 无法让多个列表同时持有另一个列表的所有权
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f516.svg#crop=0&crop=0&crop=1&crop=1&id=wJi24&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：解决方法：将 List 中 Box

 改为 Rc

```rust
enum List{
    Cons(i32,Rc<List>),
    Nil,
}
//引入作用域
use crate::List::Nil;
use crate::List::Cons;
use std::rc::Rc;
fn main(){
    
     let a=Rc::new(Cons(5,Rc::new(Cons(10,Rc::new(Cons(8,Rc::new(Nil)))))));
    let b=Cons(3,Rc::clone(&a));
    let c=Cons(4,Rc::clone(&a));
}
```

> 每次调用 Rc::clone 都会使用引用计数增加，这里的 Rc::clone 不会执行数据的深度拷贝操作。


#### 克隆 Rc 会增加引用计数

```rust

fn main(){
    
    let a = Rc::new(Cons(5,Rc::new(Cons(10,Rc::new(Nil)))));
    println!("count after creating a= {}", Rc::strong_count(&a));
    let b = Cons(3,Rc::clone(&a));
    println!("count after creating b = {}",Rc::strong_count(&a));

    {
        let c = Cons(4,Rc::clone(&a));
        println!("count after creating c = {}",Rc::strong_count(&a));
    }
    println!("count after c goes out of scope = {}",Rc::strong_count(&a));
}
====================================================
/*
输出结果：
count after creating a= 1
count after creating b = 2
count after creating c = 3
count after c goes out of scope = 2
*/
```

==Rc::strong_count 函数会读取引用计数并将它打印出来==

-  strong_count  强引用计数 
-  weak_count 弱引用计数（避免循环引用）
Rc 通过不可变引用可以让你在程序的不同部分之间共享只读数据；但不可以让你持有多个可变引用，这样会违反借用规则 

#### RefCell 和内部可变性模式

> 内部可变性是 rust 的设计模式之一。它允许你在只持有不可变引用的前提下对数据进行修改；内部可变性模式在它的数据结构中使用了  unsafe （不安全）代码来绕过 rust 正常的可变性和借用规则


![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=bK2hc&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：回忆一下借用规则：

-  在任何给定的时间里，你要么只能拥有一个可变引用，要么只能拥有任意数量的不可变引用 
-  引用总是有效的 

使用 RefCell

 的代码， rust 只会在运行时检查这些借用规则，并在出现违反借用规则的情况下触发 panic 来提前中止程序。

**Rust 将编译期检查作为默认行为。**

Rc

 和 RefCell

 只能被用于单线程场景中。

#### 三者选择依据

-  **Rc**** 允许一份数据有多个所有者，而 Box 和 RefCell 都只能有一个所有者** 
-  **Box**** 允许在编译时检查的可变或不可变借用， Rc 仅允许编译时检查的不可变借用，RefCell 允许运行时检查的可变或不可变借用** 
-  **由于  RefCell**** 允许我们在运行时检查可变引用，所以即便 RefCell 本身是不可变的，我们仍然能够更改其中存储的值** 

#### 内部可变性：可变地借用一个不可变的值

> 借用规则的推论：你无法可变地借用一个不可变的值。


-  **borrow_mut 方法来获取可变引用** 
-  **borrow 方法获取不可变引用** 

```rust
 impl Messenger for MockMessenger{
    fn send(&self , message: &str){
//两个可变引用 
        let mut one_brrow = self.sent_messages.borrow_mut();
        let mut two_brrow = self.sent_messages.borrow_mut();

        one_brrow.push(Stirng::from(message));
        two_brrow.push(Stirng::from(message));

    }

 }
============================================================
RefCell<T> 在编译时可编译通过，但会在运行时触发 Panic
```

#### 将 Rc 和 RefCell 结合使用来实现一个拥有多个所有权的可变数据

```rust
#[derive(Debug)]
enum List{
    Cons(Rc<RefCell<i32>>, Rc<List>),
    Nil,
}
use crate::List::{Cons, Nil};
use std::rc::Rc;
use std::cell::RefCell;

fn main(){

    let value = Rc::new(RefCell::new(5));

    let a= Rc::new(Cons(Rc::clone(&value),Rc::new(Nil)));
    
    let b= Cons(Rc::new(RefCell::new(6)),Rc::clone(&a));
    let c= Cons(Rc::new(RefCell::new(10)),Rc::clone(&a));

    *value.borrow_mut() += 10;


    println!("a after = {:?}",a);
    println!("b after = {:?}",b);
    println!("c after = {:?}",c);
}
========================================================
/*
输出结果：
a after = Cons(RefCell { value: 15 }, Nil)
b after = Cons(RefCell { value: 6 }, Cons(RefCell { value: 15 }, Nil))
c after = Cons(RefCell { value: 10 }, Cons(RefCell { value: 15 }, Nil)) 
*/
```

```
#### 循环引用会造成内存泄漏
```

```rust
use std::rc::Rc;
use std::cell::RefCell;
use crate::List::{Cons,Nil};


// 创建循环引用
fn main(){
    let a = Rc::new(Cons(5,RefCell::new(Rc::new(Nil))));

    println!("a initial rc coutn ={}", Rc::strong_count(&a));
    println!("a next item = {:?}",a.tail());

    let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a))));
    
    println!("a rc count after b creation ={}",Rc::strong_count(&a));

    if let Some(link) = a.tail() {
        *link.borrow_mut()=Rc::clone(&b);
    }

    println!("b rc coutn after changing a = {}", Rc::strong_count(&b));
    println!("a rc count after changing a = {}", Rc::strong_count(&a));

    // 取消下面这行，就会看到循环引用造成栈溢出
    // println!("a next item ={:?} ", a.tail());



}
#[derive(Debug)]
enum List{

 Cons(i32, RefCell<Rc<List>>),
 Nil,
}

impl List {
    fn tail(&self) -> Option<&RefCell<Rc<List>>> {

        match self{
            Cons(_,item) => Some(item),
            Nil=> None,
        }

    }

}
```

#### 使用 Weak 代替 Rc 来避免循环引用

> 强引用可以被我们用来共享一个 Rc 实例的所有权，而弱引用不会表达所有权关系，因此弱引用不会造成循环引用
在使用 Weak 指向的值之前确保它依然存在，可以调用 Weak 实例的 upgrade 方法来完成验证；这个方法返回的 Option<Rc>


```rust
use std::rc::{Rc,Weak};
 use std::cell::RefCell;

 #[derive(Debug)]
struct Node{
    value:i32,
    parent : RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}


 fn main() {
  
    let leaf = Rc::new(Node{
        value:3,
        parent: RefCell::new(Weak::new()),
        children:RefCell::new(vec![])
    });

     println!("leaf parent ={:?}",leaf.parent.borrow().upgrade());

    let branch = Rc::new(Node{
        value:5,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });

    *leaf.parent.borrow_mut()=Rc::downgrade(&branch);
    println!("leaf parent = {:?}",leaf.parent.borrow().upgrade());


}
```

#### 显示 strong_count 和 weak_count 计数值的变化

```rust
use std::rc::{Rc,Weak};
 use std::cell::RefCell;

 #[derive(Debug)]
struct Node{
    value:i32,
    parent : RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}


 fn main() {
  
    let leaf = Rc::new(Node{
        value:3,
        parent: RefCell::new(Weak::new()),
        children:RefCell::new(vec![])
    });

    println!(
        "leaf strong ={} , weak = {}",
        Rc::strong_count(&leaf),
        Rc::weak_count(&leaf),
    );


    {
        println!("leaf parent ={:?}",leaf.parent.borrow().upgrade());

        let branch = Rc::new(Node{
            value:5,
            parent: RefCell::new(Weak::new()),
            children: RefCell::new(vec![Rc::clone(&leaf)]),
        });
    
        *leaf.parent.borrow_mut()=Rc::downgrade(&branch);

        println!(
            "branch strong ={}, weak = {}",
            Rc::strong_count(&branch),
            Rc::weak_count(&branch),
        );

        println!(
            "leaf strong = {} , weak = {}",
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf),
        );
        
    }
    
    println!("leaf parent = {:?}",leaf.parent.borrow().upgrade());

    println!(
        "leaf strong = {} , weak = {}",
        Rc::strong_count(&leaf),
        Rc::weak_count(&leaf),
    );
}
/*
输出结果：
leaf strong =1 , weak = 0
leaf parent =None
branch strong =1, weak = 1
leaf strong = 2 , weak = 0
leaf parent = None
leaf strong = 1 , weak = 0
*/
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4da.svg#crop=0&crop=0&crop=1&crop=1&id=je2dh&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18) **Rc**

**  和 RefCell**

**  都只能用于单线程中。** 

## 无畏并发

> 1：1 模型：意味着一个操作系统线程对应一个语言线程
M：N 模型：M个绿色线程对应着N个系统线程，M与N不必相等（绿色线程由程序语言提供的线程叫做绿色线程）


==rust 标准库只提供了 1：1 线程模型的实现==

#### 使用 spawn 创建新线程

```rust
/*
thread::spawn 函数来创建线程，它接收一个闭包作为参数，这个闭包会包含我们想要在新线程中运行的代码。
*/
 // 创建线程引入模块
    use std::thread;
    use std::time::Duration;
   
   let handle= thread::spawn(| | {
        for i in 1..10{
            println!("线程开始运行...");
            thread::sleep(Duration::from_millis(1));//线程睡眠时间
            println!("i = {}",i);
        }
    });
```

**线程执行顺序是由操作系统线程调试策略决定的。**

#### 使用 join 句柄等待所有线程结束

> 我们通过将 thread::spawn 返回的结果保存在一个变量中，它的返回值是一个自持有所有权的 JoinHandle ,调用它的 join 方法可以阻塞当前线程直到对应的新线程运行结束。调用 join 方法来保证新线程能够在 main 函数退出前执行完毕。


```rust
   let handle= thread::spawn(| | {
 ...
    });

handle.join().unwrap();//阻塞线程
//要在什么位置阻塞就放在什么位置
```

#### 在线程中使用 move 闭包

> move 闭包常常用来与 thread::spawn 函数配合使用，它允许你在某个线程中使用来自另一个线程的数据。
在闭包参数列表前面添加 move 来强制从外部环境中捕获值的所有权。


```rust
let v = vec![1,2,3];
    let handle=thread::spawn( ||{
        println!("v {:?}",v);
    });
    handle.join().unwrap();
```

问题：在推导出如何捕获 v 后决定让闭包借用 v ，因为打印只需要使用 v 的引用，但是 rust 不知道新线程会运行多久，所以无法确定  v 的引用是否一直有效？

eg:

```rust
  let v = vec![1,2,3];
    let handle=thread::spawn( ||{
        println!("v {:?}",v);
    });
    drop(v);//在这里已经清理了 v 但新线程却还需要使用
    handle.join().unwrap();
```

使用 move 解决：

```rust
  let v = vec![1,2,3];
    let handle=thread::spawn(move ||{
        println!("v {:?}",v);
    });
    handle.join().unwrap();
//move 会强制闭包获得它所需值的所有权
```

**move 会强制闭包获得它所需值的所有权**

我们把 v 的所有权移动到了闭包中，因此我们不能在外部继续操作 v 了。

#### 使用消息传递在线程间转移数据

> 不要通过共享内存来通信，而是通过通信来共享内存。


> rust 在标准库中实现一个名为 通道（channel ） 的编程概念，它可以被用来实现基于消息传递的并发机制。
通道由发送者和接收者两个部分组成。
任何一端被丢弃，相应的通道就被关闭了。


```rust
      // 通道 
        use std::sync::mpsc;

        let (tz,rx) =mpsc::channel();

        thread::spawn( move || {
            let val=String::from("hello rx");
            tz.send(val).unwrap();
        }
        );
        
        //接收发送过来的值
        let reveived = rx.recv().unwrap();
        println!("Got: {}",reveived);
```

==接收方法：recv 和 try_recv==

#### 通道和 所有权转移

```rust
  // 通道 
        use std::sync::mpsc;

        let (tz,rx) =mpsc::channel();

        thread::spawn( move || {
            let val=String::from("hello rx");
            tz.send(val).unwrap();
            println!("val is {}",val);
            //错误，val 已经送了，无法再使用它
        }
        );
        
        //接收发送过来的值
        let reveived = rx.recv().unwrap();
        println!("Got: {}",reveived);
```

#### 发送多个值并观察接收者的等待过程

```rust
    let (s,y) = mpsc::channel();

      thread::spawn(move ||  {
        let vals= vec![
        String::from("hi"),
        String::from("from"),
        String::from("the"),
        String::from("thread"),
        ];

        for val in vals {
            s.send(val).unwrap();
            thread::sleep(Duration::from_secs(1)); //等待1秒再发送
        }

      });
// 循环接收发送过来的信息
        //   let mut rs=  y.recv().unwrap(); //只接收一个就结束了
        //   println!("Got : {}", rs);//这个也能达到全部接收,就是麻烦
        //    rs=  y.recv().unwrap();
        //   println!("Got : {}", rs);
        //   rs=  y.recv().unwrap();
        //   println!("Got : {}", rs);
        //   rs=  y.recv().unwrap();
        //   println!("Got : {}", rs);
        for received in y { //如果前面已经被接收了一些信息,那么这个循环接收的就是从前面已经接收了的开始
            println!("Got : {}",received);
        }

================================================
输出：
Got : hi
Got : from
Got : the
Got : thread
```

上面代码中 y 视作迭代器，隐式的调用的是 revc 函数；

#### 通过克隆发送者创建多个生产者

```rust
 let (s,y) = mpsc::channel();

      let tx1=mpsc::Sender::clone(&s);


      thread::spawn(move ||  {
        let vals= vec![
        String::from("hi"),
        String::from("from"),
        String::from("the"),
        String::from("thread"),
        ];

        for val in vals {
            tx1.send(val).unwrap();
            // s.send(val).unwrap();
            thread::sleep(Duration::from_secs(1)); //等待1秒再发送
        }
      });

      thread::spawn(move | | {
        let vals=vec![
            String::from("more"),
            String::from("messages"),
            String::from("for"),
            String::from("you"),
        ];

        for val in vals{
            s.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
      });
      for received in y { //如果前面已经被接收了一些信息,那么这个循环接收的就是从前面已经接收了的开始
        println!("Got : {}",received);
    }
   ================================================
/*
  输出结果：
  Got : hi
Got : more
Got : messages
Got : from
Got : for
Got : the
Got : you
Got : thread
操作 系统不同可能结果不同
*/
```

### 共享状态的并发

通过共享内存来通信。

==多个线程可以同时访问相同的内存地址。==

#### 互斥体（Mutex）

> 一个互斥体在任意时刻只允许一个线程访问数据。（获取锁、加锁、解锁）


-  必须在使用数据前尝试获取锁 
-  必须使用完互斥体守护的数据后释放锁，其他线程才能继续完成获取锁的操作。 

> rust 中，由于类型系统和所有权规则的帮助，我们可以保证自己不会在加锁和解锁这两个步骤中出现错误。


#### Mutex 的接口

```rust
//单线程环境下
use std::sync::Mutex;
 fn main() {
    let m=Mutex::new(5);
    {
        let mut num=m.lock().unwrap();
        //lock 方法获取锁
        *num=6;
    }
    println!("m = {:?}",m);
}
```

Mutex

 是一种智能指针，对 lock 的调用会返回一个名为 MutexGuard 的智能指针。

```rust
//多线程环境下
use std::{sync::Mutex,thread};
fn main(){
        let counter= Mutex::new(0);
        let mut handles=vec![];
    
       for _ in 0..10 {  //创建 10 个线程
        let handle=thread::spawn(move|| {  //报错点
            let mut num=counter.lock().unwrap();
        });
        
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap(); 
    }

    println!("Result : {}",*counter.lock().unwrap());
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&id=vPfCM&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：

```rust
  let handle= thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num +=1;
        });

        handles.push(handle);

        let handle2 = thread::spawn( move|| { //报错点
            let mut num2= counter.lock().unwrap();
            *num2+=1;
        });
        handles.push(handle2);
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&id=q9ri9&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：

程序报错原因：counter 被移动到了 handle 指代的线程中，而移动行为阻止了第二个线程中调用 lock 再次捕获 counter。

#### 多线程与多重所有权

```rust
use std::{sync::Mutex,thread};
use std::rc::Rc;

fn main(){

    // let counter= Mutex::new(0);
    let counter = Rc::new(Mutex::new(0));
    let mut handles=vec![];

    for _ in 0..10 {  //创建 10 个线程
        let counter = Rc::clone(&counter);
        let handle=thread::spawn(move|| { //报错
            let mut num=counter.lock().unwrap();
        });
        
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap(); 
    }

    println!("Result : {}",*counter.lock().unwrap());
}
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&id=tKym8&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18):报错原因

```rust
error[E0277]: `Rc<Mutex<i32>>` cannot be sent between threads safely
   --> main.rs:38:20
    |
38  |           let handle=thread::spawn(move|| {
    |  ____________________^^^^^^^^^^^^^_-
    | |                    |
    | |                    `Rc<Mutex<i32>>` cannot be sent between threads safely
39  | |             let mut num=counter.lock().unwrap();
40  | |         });
    | |_________- within this `[closure@main.rs:38:34: 40:10]`
    |
    = help: within `[closure@main.rs:38:34: 40:10]`, the trait `Send` is not implemented for `Rc<Mutex<i32>>`
    = note: required because it appears within the type `[closure@main.rs:38:34: 40:10]`
note: required by a bound in `spawn`
   --> C:\Users\sysy\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib/rustlib/src/rust\library\std\src\thread\mod.rs:621:8
    |
621 |     F: Send + 'static,
    |        ^^^^ required by this bound in `spawn`

error: aborting due to previous error


==========================================================
`Rc<Mutex<i32>>` cannot be sent between threads safely 这意味着我们新创建的 std::rc::Rc<std::sync::Mutex<i32>> 类型无法安全地在线程间传递。

the trait `Send` is not implemented for `Rc<Mutex<i32>>` 这个是说明这个类型不满足 trait 约束 Send.
```

Rc

 在跨线程使用时并不安全。

#### 原子引用计数 Arc

> Arc 类型：它拥有类似于 Rc 的行为，又保证了自己可以被安全地用于并发场景。 A 代表原子（atomic）表明自己是一个原子引用计数


```rust
use std::{sync::Mutex,thread,sync::Arc};
use std::rc::Rc;

fn main(){

    // let counter= Mutex::new(0);
    // let counter = Rc::new(Mutex::new(0));
    let counter = Arc::new(Mutex::new(0)); //它本身不可变
    let mut handles=vec![];

    for _ in 0..10 {  //创建 10 个线程
        // let counter = Rc::clone(&counter);
        let counter = Arc::clone(&counter);
        let handle=thread::spawn(move|| {
            let mut num=counter.lock().unwrap();
            //我们可以获取它内部值的可变引用
        });
        
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap(); 
    }

    println!("Result : {}",*counter.lock().unwrap());
}
//输出结果 ： Result  : 10
```

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/24c2.svg#crop=0&crop=0&crop=1&crop=1&id=musGc&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)  终于把这个多线程程序写成功了！

#### RefCell/Rc 和 Mutex/Arc 之间的相似性

#### 使用 Sync trait 和 Send trait 对并发进行扩展

> rust 语言本身内置的并发特性非常少。
std::marker 模块内的  Sync trait 与 Send trait


-  ==允许线程间转移所有权的 Send trait== 
-  ==允许多线程同时访问的 Sync trait== 

## Rust 面向对象编程特性

> 面向对象的程序由对象组成。对象包装了数据和操作这些数据的流程。这些流程称为方法或操作。
rust 中默认的 trait 方法进行代码的共享
可以在 Rust 中使用泛型来构建不同类型的抽象，并使用 trait 约束来决定类型必须提供的具体特性。这技术称为 限定参数化多态。


# 模式匹配

```rust
match 分支 {
    模式 => 表达式,
    模式 => 表达式,
    模式 => 表达式,
    ...    
}
```

==记住 match 表达式必须穷尽匹配值的所有可能性。但是我们可以在最后的 分支处使用全匹配模式。使用特殊的下划线 _可以用来匹配所有可能的值。==

#### if let 条件表达式

> 我们可以混合使用 if  let 、 else  if 及 else if let 表达式来进行匹配。


```rust

 fn main() {

    let favorite_color : Option<&str> = None;
    let is_tuesday = false;
    let age: Result<u8,_>="34".parse();
    
    if let Some(color)=favorite_color {
        println!("Using  your favorite  color , {} , as the background ",
    color
    );    
    } else if is_tuesday{
        println!("Tuesday  is green  day!");
    } else if let Ok(age) = age {
        if age>30{
            println!("Using  purple  as the  background color");
        }else {
            println!("Using  orange  as the  background  color");
        }
    }else {
        println!("Using  blue  as the  bakcground  color ");
    }
}

// 输出结果为： 
Using  purple  as the  background color
```

#### while  let 条件循环

> 它和 if let 的构造十分类似，但它会反复执行同一个模式匹配直到出现失败的情形。


```rust
  let mut stack= Vec::new();

    stack.push(1);
    stack.push(2);
    stack.push(3);

    while let Some(top) = stack.pop(){
        println!("{}",top);

    }
// 上面这个 while let ： 只要 stack.pop() 返回的值是 Some 变体，那么这个循环就会不断的打印
```

#### for 循环

```rust
println!("==============================================");
let v=vec!['a','b','c'];

for (index , value ) in v.iter().enumerate(){
    println!("{} is  at index {}",value,index);
}
```

for 语句中紧随关键字 for 的值就是一个模式。

**enumerate 方法它会在每次 迭代过程中生成一个包含值本身及值索引的元组。**

#### let 语句

```rust
let x = 5;
let PATTERN=EXPRESSION;
//上面x就是整个模式本身，实际上意味着“无论表达式会返回什么样的值，我们都可以将它绑定到变量 x 中

let (x,y,x)=(1,2,2);
//使用模式来解构元组并一次性创建出 3 个变量
//上面的模式要匹配，否则会报错
```

#### 函数参数

> 函数的参数也是模式。


```rust
fn foo(x: i32){
    
}

//签名中的 x 部分就是一个模式！

fn print_sy(&(x,y) : &(i32,i32)){
    println!("Current location: ({} , {}",x,y);
}

fn main(){
    let point =(3,5);
    print_sy(&point);
}
```

## 可失败性：模式是否会匹配失败

> 模式可以分为不可失败和可失败两种类型。
不可失败的模式能够匹配任何传入的值。


```rust
let x=5;
//x 就是一个不可失败的模式匹配；它能够匹配右侧表达式所有可能的返回值

if let Some(x)=a;
//Some(x) 就是一个可失败模式，如果值为 None 就会匹配失败
```

> ==函数参数、let 语句、for 循环只接收不可失败模式；if  let 和 while let 表达式则只接收可失败模式==


> 不能在不可失败模式中使用可失败模式


### 模式语法

#### 匹配字面量

```rust
println!("==============================================");
let v=vec!['a','b','c'];

for (index , value ) in v.iter().enumerate(){
    println!("{} is  at index {}",value,index);
}
```

#### 匹配命名变量

```rust

let x=Some(5);
let y=10;

match x{

    Some(50) => println!("Got 50"),
    Some(y)  => println!("Matched,y = {:?}",y),
    //在这个匹配分支中的模式引入了新的变量 y ，它会匹配 Some 变体中携带的任意值。所以这里的 y 是一个新的变量
    _        => println!("Default case , x={:?}",x),
}

println!("at the end : x = {:?}, y= {:?} ",x,y);
//输出结果
Matched,y = 5
at the end : x = Some(5), y= 10
```

**命名变量是一种可以匹配任何值的不可失败模式**

#### 多重模式

> 可以在 match 表达式的分支匹配中使用 | 表示 或的意思，它可以被用来一次性匹配多个模式。


```rust
//  多重模式
println!("=============================");

let x=1;
match x {
    1 | 2 => println!("one or two"),
     3 => println!("three"),
     _ => println!("anything"),
}
```

#### 使用 … 来匹配区间

```rust
// 匹配区间
let x=5;
match x {
    1..=5 =>println!("one through  five"),
    //这句代码也换成 : 1 | 2 | 3 | 4 | 5
    // 6...9 =>println!("6 到  9"),  这里编译器说不建议使用 范围模式
    _ => println!("something else"),
}
//输出结果：
one or two
one through  five
```

**范围模式只被允许使用数值或 char  值来进行定义；**

```rust
let x = 'c';

match x {
    'a' ..= 'j'  => println!("early  ASCLL letter"),
    'k' ..= 'z'  => println!("late  ASCLL letter "),
    _ => println!("something else"),
}
```

#### 使用解构来分解值

> 我们可以使用模式来分解结构体、枚举、元组或引用


```rust
struct Point{
    x:i32,
    y:i32,
}

let p=Point{x:0,y:7};

let Point{x:a,y:b}=p;
//简便写法
//let Point{x:x,y:y}=p;
//上面这条的简便写法
let Point{x,y}=p;

assert_eq!(0,a);
assert_eq!(7,b);
```

```rust
let p = Point{x:0,y:7};

match p{
    Point{x,y:0} => println!("On the x axis  at {}",x),
    Point{x:0,y} => println!("On the y axis at{}",y),
    Point{x,y}  => println!("On neither axis :({}{})",x,y),
}
```

#### 解构枚举

```rust
enum Message {
    Quit,
    Move {x:i32 , y : i32},
    Write(String),
    ChangeColor(i32,i32,i32),
}

let msg=Message::ChangeColor(0,160,255);

match  msg {
    Message::Quit =>{
        println!("The Quit  variant has no  data to  destructure.")
    },
    Message::Move{x,y} => {
        println!("Move in the x direction {} and in the y direction {}",x,y);
    },
    Message::Write(text) =>println!("Text message :{} ",text),
    Message::ChangeColor(r,g,b) => {
        println!("
        Change the color to red {}, green {} ,and blue {}",r,g,b);
    }

}
//输出结果：
  Change the color to red 0, green 160 ,and blue 255
```

#### 解构嵌套的结构体和枚举

> 匹配语法可以被用于嵌套的结构中！！！


```rust


enum Message {
    Quit,
    Move {x:i32 , y : i32},
    Write(String),
    ChangeColor(Color),
}

enum Color {
    Rgb(i32,i32,i32),
    Hsv(i32,i32,i32)
}

// 以下是主函数部分

let msg=Message::ChangeColor(Color::Hsv(0,160,255));

match msg{
    Message::ChangeColor(Color::Rgb(r,g,b)) => {
        println!("Change  the color  red {} , green{} , and blue {}",r,g,b);
    },
    Message::ChangeColor(Color::Hsv(h,s,v)) => 
    {
        println!("Chang the color to hue {} , saturation {} , and value {}",h,s,v);
     }
     _ => ()

}
```

#### 解构结构体和元组

```rust
let ((feet, inches),Point{x,y})= ((3,10),Point{x:3,y:-10});
```

#### 忽略模式中的值

#### 使用 _  忽略整个值

```rust
//在函数签名中作用 _
foo(3,4);
    fn foo ( _:i32 , y : i32){
        println!("This  code only  uses the y parameter : {}",y);
    }
```

上面代码的函数签名中，虽然是两个参数，但是传入参数的时候，会忽略到第一个参数！！！

#### 使用嵌套的 _ 忽略值的某些部分

```rust
let mut setting_value = Some(5);
    let new_setting_value = Some(10);

    match (setting_value,new_setting_value) {
        (Some(_),Some(_)) => {
            println!("Can't  overweite an existing  customized value");
        }

        _ => {
            setting_value=new_setting_value;
        }
    }
    println!("Setting is {:?}",setting_value);
//输出结果：
Can't  overweite an existing  customized value
Setting is Some(5)
```

当我们不需要 Some 中的值时，在模式中使用下划线来匹配 Some 变体

```rust

    let  numbers=(2,4,8,16,32);

    match numbers {

        (first, _, third , _, fifth ) => {
            println!("
            Some numbers: {} {} {} ",first,third,fifth);
        }
    }


//输出结果：
2  8 32
```

#### 通过以 _ 开头的名称来忽略未使用的变量

```rust
    let _x = 5;//如果没有使用变量，不用下划线开头，就会有警告
    let y=10;//有警告，变量未被使用
```

> 使用下画线开头的变量名与仅仅使用_ 作为变量名存在一个细微的差别：
_x 语法仍然将值绑定到了变量上，而 _ 则完全不会进行绑定。


```rust
  let _x = 5;//如果没有使用变量，不用下划线开头，就会有警告
    let y=10;//有警告，变量未被使用

    let s = Some(String::from("Hello!"));

    if let Some(_s) = s {
        println!("found a string");
    }
    println!("{:?}",s);//报错：因为以下画线开头的未使用变量仍然绑定了值，会导致值的所有权发生转移

=======================================================

    let _x = 5;//如果没有使用变量，不用下划线开头，就会有警告
    let y=10;//有警告，变量未被使用

    let s = Some(String::from("Hello!"));

    if let Some(_) = s {
        println!("found a string");
    }
    println!("{:?}",s);//正确： 因为下划线不会转移值的所有权
```

==下划线开头的变量会转移值的所有权，而纯下划线不会转移值的所有权==

#### 使用  .. 忽略值的剩余部分

```rust
struct Poing {
    x :i32,
    y :i32,
    z :i32,
}

let origin = Poing {x:0,y:0,z:0};
match origin{
    //在这里模式匹配中我们忽略了 y 和 z 的值 
    Poing {x , ..} => println!("x is {} " ,x ),
}
```

**. . 语法会自动展开并填充任意多个所需要的值**

```rust
let numberss= (2,4,8,16,32);

match numberss {

    (first, .. , last ) => {
        println!("
        Some numberss : {} , {}",first,last);//输出 2 32 
    },

}
//以上代码只需要匹配第一个值和最后一个值，忽略中间的所有值

==============================
//不可以像下面这样不确定 

(.. , second , .. ) => {
    ...
}
```

#### 使用匹配守卫添加额外条件

> 匹配守卫：就是附加在 match 分支模式后的 if 条件语句，分支中的模式只有在这个条件被同时满足时才能匹配成功。


```rust
let num = Some(4);

match num {
    Some(x) if x<5 => println!("less than five : {} ",x),
    Some(x)  => println!("{}",x),
    None => (),
}
//就是添加一个条件来匹配
匹配守卫：就是附加在 match 分支模式后的 if 条件语句，分支中的模式只有在这个条件被同时满足时才能匹配成功。
```

```rust

let x = Some(5);
let y = 10;

match x {

    Some(50) => println!("Got 50"),
    Some(n) if n==y => println!("Matched, n = {:?}",n),
    //这里并没有引入新的变量 y ,而是合适外部的变量 y ; 使用 Some(n)  来避免覆盖 外部变量 y
    _ => println!("Default case , x = {:?} ",x),

}

println!("at the end : x= {:?}, y={:?}",x,y);
//输出结果：
Default case , x = Some(5)
at the end : x= Some(5), y=10

//上面代码中的  if  n==y  不是一个模式，所以它不会引入新的变量，所以就会使用外部变量中的  y
```

我们同样可以在匹配守卫中使用 或运算符 |  来指定多重模式

```rust
let x =4;
let y = false;

match x {
    4 | 5 | 6 if y => println!("yes"),
    _ => println!("no"),
}
//上面匹配优先级是先匹配数字再匹配 if 表达式
```

#### @绑定

> @ 运算符允许我们在测试一个值是否匹配模式的同时创建存储该值的变量。


```rust
enum Message1{
    Hello{id:i32},
}
let msg = Message1::Hello{id:5};
match msg {
    Message1::Hello{id : id_variable @ 3 ..=7} => {
        println!("Found an id in range :{}",id_variable)
    },
    Message1::Hello{id : 10 ..=12} => {
        println!("Found an id in another range");
    },
    Message1::Hello{id} => {
        println!("Found some other id : {}",id);
    },
}
//输出结果为：
Found an id in range :5

通过在 3...7 之前使用 id_variable @ ，在测试一个值是否满足区间模式的同时可以捕获到匹配成功
```

# 高级特性

## 不安全 Rust

### 不安全超能力

**可以在代码块前面使用 unsafe 来切换到不安全模式。**

就是将代码安全责任转交到程序员手上。

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/26a0.svg#crop=0&crop=0&crop=1&crop=1&id=rl0YC&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：不安全超能力有：

-  解引用祼指针 
-  调用不安全的函数或方法 
-  访问或修改可变的静态变量 
-  实现不安全 Rust 

> 注意： unsafe  关键字不会关闭借用检查器或禁用任何其他 Rust 安全检查。


#### 解引用裸指针

> 不安全 Rust 的世界里拥有两种类似于引用的新指针类型,叫作裸指针。
裸指针要么是可变的，要么是不可变的 ： *const T  和 *mut T


**裸指针与引用、智能指针的区别在于：**

-  允许忽略借用规则，可以同时拥有指向同一个内存地址的可变和不可变指针，或者拥有指向同一个地址的多个可变指针 
-  不能保证自己总是指向了有效的内存地址 
-  允许为空 
-  不骨实现任何自动清理机制 

```rust
   let mut num = 5;
// 通过引用创建裸指针
//这两个指针都来自有效的引用，可以确认它们的有效性
    let r1 = &num as *const i32;
    let r2 = &mut num as *mut i32;
```

**我们可以安全代码内合法地创建裸指针，但是不能在不安全代码块外解引用裸指针。**

下面的代码是合法的，但是不应该编写这样的代码，因为无法确定其有效性

```rust
  // 创建一个指向任意内存地址的裸指针
    let address =0x012345usize;
    let r= address as *const i32;
```

```rust
  let mut num = 5;
// 通过引用创建裸指针
    let r1 = &num as *const i32;
    let r2 = &mut num as *mut i32;

    unsafe{
        println!("r1 is : {}",*r1);
        println!("r2 is : {}",*r2);
    }

//不安全代码一定要在 unsafe 代码块中才可以解引用，否则会报错
```

#### 调用不安全函数或方法

**不安全函数或方法就是前面名字前面加了 unsafe 关键字修饰，不安全函数必须要不安全代码中才可以调用，否则会报错！！！**

```rust
fn main(){

    unsafe{
        priunsafe();
    }
}
//表明这个函数体内的代码块也是不安全的
unsafe fn priunsafe(){
    println!("unsafe!!!");
}
```

#### 创建不安全代码的安全抽象

函数如果有不安全代码并不意味着我们需要将整个函数都标记为不安全的。我们应该**将不安全代码封装在安全函数中是一种十分常见的抽象。**

```rust
    //下面调用的这个方法是标准库中使用不安全代码的函数
    let (a,b) = r.split_at_mut(3);
    println!("{:?}",a);
    println!("{:?}",b);

=============================================
fn split_at_mut(slice : &mut [i32], mid : usize) ->(&mut [i32],&mut [i32]){
    let len = slice.len();
    assert!(mid<= len>);
    (&mut slice[..mid],&mut slice[mid..])
}
```

#### 使用 extern  函数调用外部代码

> Rust 使用 extern 关键字简化创建和使用外部函数接口（FFI）的过程，FFI 是编程语言定义函数的一种方式，它允许其他外部的编译语言来调用这些函数


```rust
extern "C"{
    fn abs(input :i32) -> i32;
}
```

**任何在 extern 块中声明的函数都是不安全的**

> 在其他语言中调用 Rust 函数：
extern 关键字及对应的 ABI 添加到函数签名的 fn 关键字前，并为这个函数添加 #[no_mangle] 注解来避免 Rust 在编译时改变它的 名称。


> ABI:每个[操作系统](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/192)都会为运行在该系统下的应用程序提供应用程序二进制接口（Application Binary Interface，ABI）。ABI包含了[应用程序](https://baike.baidu.com/item/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F/5985445)在这个系统下运行时必须遵守的编程约定。ABI总是包含一系列的系统调用和使用这些系统调用的方法，以及关于程序可以使用的内存地址和使用机器寄存器的规定。从一个应用程序的角度看，ABI既是系统架构的一部分也是硬件体系结构的重点，因此只要违反二者之一的条件约束就会导致程序出现严重错误。在很多情况下，[链接器](https://baike.baidu.com/item/%E9%93%BE%E6%8E%A5%E5%99%A8/10853221)为了遵守ABI的约定需要做一些重要的工作。例如，ABI要求每个应用程序包含一个程序中各例程使用的静态数据的所有地址表，链接器通过收集所有链接到程序中的模块的地址信息来创建地址表。ABI经常影响链接器的是对标准过程调用的定义


> Rust 为了让其他语言 正常地识别 Rust 函数，必须要禁用编译器 的改名功能。


#### 访问或修改一个可变静态变量

> Rust 中全局变量也称为静态变量


```rust
// 静态变量
 static HELLO:&str ="Hello , world!!";

fn main() {
    
    println!("static is {}",HELLO);


    static mut COUNT:u32 =0;
    fn add_to_count(inc : u32){
        unsafe {
            COUNT+=inc;
        }
    }

    add_to_count(3);
    unsafe{
        println!("COUNT: {}",COUNT);
    }
}
```

> 静态变量必须要标注自己的类型；静态变量只能存储拥有 **'static**周期的引用。
常量和不可变静态变量的区别：


-  静态变量的值在内存中拥有固定的地址，使用它的值总是会访问到同样的数据；常量则允许在任何被使用到的时候复制其数据 
-  静态变量是可变的，访问和修改可变的静态变量是不安全的 

#### 实现不安全 trait

```rust
// 实现不安全 trait
unsafe trait Foo{

}
unsafe impl Foo for i32{
    
}
```

### 高级 triat

#### 在trait 的定义中使用关联类型指定占位类型

> 关联类型是 trait 中的类型占位符，它被用于  triat 的方法签名中


```rust
struct Point{
    x:u32,
    y:u32,
}

pub trait Iterator{
    type Item;
    // Item 是一个占位符，Iterator trait 的实现者需要为 Item 指定具体的类型
    fn next(&mut self) -> Option<Self::Item>;
}

impl Iterator for Point{
    type Item=u32;//指定类型
    fn next(&mut self) -> Option<Self::Item>{
        Some(5)
    }
}

// 使用泛型的版本
pub trait Iterator<T> {
    fn next (&mut self) -> Option<T>;
}
```

> 占位类型和泛型的区别：
使用泛型在每次实现该 trait 的过程中标注类型，我们可以实现任意的迭代类型，从而使用得可以有多个不同版本的实现。我们**可以为一个类型同时多次实现 trait**
关联类型不需要在使用 trait 的方法时标注类型，**不能为单个类型多次实现这样的 trait**


#### 默认泛型参数和运算符重载

```rust

 use std::ops::Add;

 #[derive(Debug,PartialEq)]
 struct Point1{
    x:i32,
    y:i32,
 }
 
 impl Add for Point1{
    type Output = Point1;
//这里相当于重载 + 号运算符
    fn add(self , other : Point1) -> Point1{
        Point1{
            x:self.x+other.x,
            y:self.y+other.y,
        }
    }
 }
//为了实现两个点相加我们重载了 + 号运算符
//rust 可以重载的运算符有限，只有几个，
// 这个重载和 C++ 中的差不多，就是可以重载的数量太少
//标准库实现：
#[doc(alias = "+")]
pub trait Add<Rhs = Self> {
    //这里的 Add trait 使用了默认泛型参数
    /// The resulting type after applying the `+` operator.
    #[stable(feature = "rust1", since = "1.0.0")]
    type Output;

    /// Performs the `+` operation.
    ///
    /// # Example
    ///
    /// ```
    /// assert_eq!(12 + 1, 13);
    /// ```
    #[must_use]
    #[stable(feature = "rust1", since = "1.0.0")]
    fn add(self, rhs: Rhs) -> Self::Output;
}
```

上面代码中  Rhs=self 使用了默认泛型参数；没有为 Rhs 指定类型，那么  Rhs 的类型就会默认为 self

默认参数使用场景：

-  扩展一个类型而不破坏现有代码 
-  允许大部分用户不需要的特定场合进行自定义 

#### 用于消除歧义的完全限定语法：调用相同名称的方法

> Rust 不会阻止两个 trait 拥有相同名称的方法，也不会阻止你为同一个类型实现这样的两个 trait


```rust
 trait Pilot{
     fn fly(&self);
 }

 trait Wizard{
     fn fly(&self);
 }

 struct Human;

 impl Pilot for Human{
    fn fly(&self){
        println!("This is your captain speaking.");
    }
 }

 impl Wizard for Human{
    fn fly(&self){
        println!("Up");
    }
 }

 impl Human{
     fn fly(&self){
         println!("*waving arms furiously*");
     }
 }
//上面定义了两个拥有同名方法 fly 的 trait 并且它本身也拥有一个 fly 方法

fn main() {

    let p=Human;
    p.fly();//调用自身的
    Pilot::fly(&p);
    Wizard::fly(&p);
// 在方法名前指定 trait 名称清晰的表明我们要调用哪个实现
}
```

```rust
trait Animal{
    fn baby_name() -> String;
}
struct Dog;
impl Dog{
    fn baby_name() -> String{
        String::from("Dog")
    }
}
impl Animal for Dog{
    fn baby_name() -> String {
        String::from("Animal")
    }
}


fn main() {


    println!("A baby dog is called a {}",Dog::baby_name());//Dog
    // println!("A baby dog is called a {}",Animal::baby_name()); //编译错误
    // 使用完全限定语法来调用 Dog 为 Animal trait 实现的函数
    println!("B baby dog is called a {}",<Dog as Animal>::baby_name());
}
```

==完全限定语法：==

```rust
<Type as Trait>::function(receiver_if_method, next_arg,...);
```

**可以任何调用函数或方法的地方使用完全限定语法；当代码中存在多个同名实现的时候使用这个语法是非常有用的**

#### 用于在  trait 中附带另外一个 trait 功能的超 trait

```rust
use std::fmt;
trait OutlinePrint : fmt::Display {

    fn outline_print(&self){
        let output = self.to_string();
        let len=output.len();
        println!("{}","*".repeat(len+4));
        println!("*{}*"," ".repeat(len+2));
        println!("*  {}  *",output);
        println!("*{}*"," ".repeat(len+2));
        println!("{}","*".repeat(len+4));
    }
}


struct Point2{
    x:i32,
    y:i32,
}
//因为上面的 OutlinePrint trait 依赖于 Display trait 所要在这里实现这个 trait 否则会报错
impl fmt::Display for Point2{
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result{
        write!(f,"({},{})",self.x,self.y)
        // 这个有点像 C++ 的重载 << 运算符
    }
}
impl OutlinePrint for Point2{}



fn main() {
    let p2= Point2{x:5,y:6};
    p2.outline_print();
```

#### 使用 newtype 模式在外部类型上实现外部 trait

> 孤儿规则：只有当类型和对应 trait 中的任意一个定义在本地包内时，才能够为该类型实现这一个 trait。
我们可以使用 newtype 模式来绕过这个限制


```rust
    struct Wrapper(Vec<String>);
    impl fmt::Display for Wrapper{
        fn fmt(&self , f : &mut fmt::Formatter) -> fmt::Result{
            write!(f,"[{}]",self.0.join(", "))
        }
    }
fn main() {

    let w=Wrapper(vec![String::from("hello"),String::from("world")]);
    println!("w = {}",w);//输出：w = [hello, world]
}
//上面代码中孤儿规则会阻止我们直接为 Vec<T> 实现 Display trait ,因为它们的类型都被定义在外部包中
//我们可以创建一个持有 Vec<T> 的实例，接着我们就可以使用它实现 Display
```

###　高级类型

#### 使用 newtype 模式实现类型安全与抽象

> newtype的另外一个作用就是为类型的某些细节提供抽象能力。
newtype 模式还可以被用来隐藏内部实现。


```rust
use std::ops::Add;
struct Millimeters(u32);
struct Meters(u32);
impl Add<Mteters> for Millimeters{
    type Output = Millimeters;
    fn Add(self , other: Meters)-> Millimeters {
        Millimeters(self.0+(other.0*1000))
    }
}
//这就是典型的 newtype 模式
```

#### 使用类型别名创建同义类型

==也就是创建类型别名==

```rust
type Kilometers = i32;
//这里的 Kilometers 就等同于 i32 类型
```

```rust
   // 类型别名功能
    type sy=i32;
    let x:i32 = 5;
    let y:sy  = 10;
    println!("x+y= {}",(x+y));
```

有时候我们拥有比较长的类型时，就可以使用类型别名来简短类型。

```rust
type bds= Box<dyn Fn() + Send +'static>;
//这样就可以使用 bds 去替换下面的比较长的类型名

    let f : Box<dyn Fn()+ Send + 'static>=Box::new(|| println!("hi"));

    fn takes_long_type (f: Box<dyn Fn() + Send +'static>){

    }
    fn returns_long_type() -> Box<dyn Fn() + Send + 'static>{
        Box::new(|| ())
    }
```

---

#####　永远不返回的 Never类型

> Rust 有一个名为 ！ 的特殊类型，它在类型系统中称为空类型，因为它没有任何值。它也从不返回的函数中充当返回值的类型。


```rust
 fn main(){
     
  let x:() =bar1();//
    let y=bar1();

}

fn bar() -> ! {//发散函数要有一个 panic! 否则会报错
    let x = 10;
    println!("x= {}",x);
    panic!("发散函数！！！");
}
//这里没有太明白，后面再加强一下这里

fn bar1() -> (){//这个括号相当于 c/c++ 中的 void 
    println!("这个也代表没有返回的意思");
}
```

`类型 ! 的表达式可以被强制转换为其他的任意类型。`

使用了 never 的类型：（一些）

-  panic! 
-  循环 loop（它的返回值类型也是 ！） 
-  continue 的返回值类型也是 ! 

#### 动态大小类型和 Sized trait

> 动态类型大小　DST；也就是不确定大小类型；它们只有在运行时才能确定其大小


str  就是一个动态类型大小；（**这里是  str 不是 &str**）

```rust
let s1:str="hello";//str 类型只有在运行时才能确定大小，因此我们不能创建出 str 类型的变量
    let s2:str="world";
//上面两个都报错，无法使用

//上面代码换成  &str就可以使用了
//因为 &str 实际上就由 str 的地址和它的长度两个值构成的
```

trait 也就是一种动态大小类型，每一个 trait 都是一个可以通过名称来进行引用的动态大小类型。

**Rust 提供了一个特殊的 Sized trait 来确定一个类型的大小在编译时是否可知，rust 会为每一个泛型函数隐式地添加 Sized 约束**

```rust
fn generic<T> (t : T) {

}
// 上成函数会被隐式转换为：
fn generic<T : Sized> (t : T){

}
```

==在默认情况下，泛型函数只能被用于在编译时已经知道大小的类型。==

可以使用下面的特殊语法来解除限制：

```rust
fn generic<T : ?Sized > (t : &T){
    ...
}
/*
?Sized trait 约束表达了与 Sized 相反的含义，我们可以将它理解为：T 可能是也可能不是 Sized 的
这个语法只能用在 Sized 上，而不能被用于其他 trait
上面将类型 t 改为了 &T ；因为可能不是 Sized
*/
```

### 高级函数与闭包

#### 函数指针

fn 就是函数指针

```rust
fn add_one(x :i32) ->i32{
    x+1
}

fn do_twice(f : fn(i32) -> i32 , arg :i32) -> i32 {
    f(arg)+f(arg)
}

//语法：
f : fn(参数列表类型) -> 返回值类型
```

> 与闭包不同， fn 是一个类型而不是一个 trait ，所以我们可以直接指定 fn 为参数类型，不用声明一个以 Fn trait 为约束的泛型参数


`函数指针实现了全部 3 种闭包 trait : Fn FnMut FnOnce`

**因此我们可以把函数指针用作参数传递给一个接收闭包的函数。**

eg: 既可以使用闭包也可以使用命名函数：

```rust
 let list_of_numbers = vec![1,2,3];
// 使用 map 函数将一个整形动态数组转换为一个字符串动态数组
    let list_of_strings : Vec<String> =list_of_numbers.
    iter()
    .map(|i| i.to_string())
    .collect();

    let list_of_stringsy: Vec<String> = list_of_numbers
    .iter()
    .map(ToString::to_string)
    //这里必须使用完全限定语法;因为这个作用域中存在多个可用的 to_string 函数
    .collect();


enum Status {
        Value(u32),
        Stop,
    }

    let list_of_statuses: Vec<Status> =
    (0u32..20)
    .map(Status::Value)
    .collect();
```

#### 返回闭包

**无法在函数直接返回一个闭包**

Rust无法推断出需要多大的空间来存储返回的闭包。

![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/1f4d6.svg#crop=0&crop=0&crop=1&crop=1&id=MlbrK&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)：解决： 使用 trait 对象：

```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32>{
    Box::new(|x| x+1)
}
```

#### 宏

> 使用　macro_rules! 构造的声明宏及另外 3 种过程宏：


-  用于结构体或枚举的自定义 #[derive] 宏，它可以指定随 derive 属性自动添加的代码 
-  用于为任意条目添加自定义属性的属性宏 
-  看起来类似于函数的函数宏，它可以接收并处理一段标记序列 

#### 宏和函数之间的差别

> 宏是一种用于编写其他代码的代码编写方式，也就是元编程范式
区别：


-  函数在定义签名时必须声明自己参数的个数与类型，而宏则能够处理**可变数量的参数**； 
-  宏的定义要比函数定义复杂得多 
-  在某个文件中调用宏时，必须提前定义宏或将宏引入当前作用域中，而函数则可以在任意位置定义并在任意位置使用 

#### 用于通用元编程的 macro_rules! 声明宏

```rust
  let v : Vec<u32> =vec![1,2,3];
    // 标准库中简化后的 vec! 宏定义
    #[macro_export]
    macro_rules! vec {
        ($ ($x:expr), *) =>{
            {
                let mut temp_vec=Vec::new();
                $(
                    temp_vec.push($x);
                )*
            temp_vec
            }
        }
    }

//#[macro_export] 意味着这个宏会在它所处的包被引入作用域后可用，缺少了这个标注的宏则不能被引入作用域
```

**我们使用 macro_rules! 及不带感叹号的名称来开始定义宏。**

#### 基于属性创建代码的过程宏

> 过程宏（它们的形式像函数）：
有 3 种： 自定义派生宏、属性宏、函数宏
当创建过程宏时宏的定义必须单独放在它们自己的包中，并使用特殊的包类型。


宏知道的不多，得后面再仔细看一下

# 构建多线程 web 服务器

> 超文本传输协议 HTTP 和 传输控制协议 TCP
它们两个都是基于 请求和响应 的协议
TCP 是一种底层协议，它描述了信息如何从一个服务器传送到另外一个服务器的细节，但它并不指定信息的具体内容。HTTP 协议建立在 TCP 之上，它定义了请求和响应的内容。


# TypeScript

javascript 的类型分为两种： 原始数据类型和对象类型

原始数据类型有：

-  boolean 
-  number 
-  string 
-  null 
-  undefined 
-  Symbol 
-  BigInt 

```typescript
// npm install -g typescript
// tsc 文件名.ts

let isDone : boolean =false;
// 数字类型都 number
let decLiteral : number=6
// 字符类型可以单引号或双引号
let name1: string  = "bob";
name1=`syhk`;

// 模板字符串:第一种被反引号包围
let stance: string=`my name is ${name1}`;
let stance1: string="my name is "+ (name1)+"";//同样的效果
let num1:number = 10
console.log(`be ${10+ num1}`) //支持运算
// 数组
let list:number[] = [1,2,3];
// 数组泛型
let list1: Array<number> =[1,2,3];
console.log(list)
console.log(list1)

// 元组 Tuple
let x:[string,number]=['hello',100];//赋值顺序要和声明顺序一样
console.log(x);
console.log(x[0].substring(1));
console.log(x[1]) //第二个是数字类型

// 枚举 默认编号从 0 开始为元素编号 
enum Color{Red,Green,Blue}
let c:Color =Color.Blue
// 也可以手动赋值编号 
enum Color1{Red =1,Green=2,Blue=4}
console.log(Color[0])
console.log(Color1[4])

//eg:动态内容 我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。
let notSure: any =4; 
//变量在声明的时候，未指定类型那么它就会被识别为任意值类型；
let notSure2 ="syhk";
//  notSure2= 200; //类型推论，上面已经被推断成了 string 类型
 
//  区别于上面：如果定义的时候没有赋值，不管之后有没有赋值，都被推断成 any 类型
 let notSure3;
 notSure3='syhk';
 notSure3=600;


notSure = 'maybe a string instead';
cout(notSure)
notSure=false;
cout(notSure)
// 和 Object 有相似的作用，但是它只允许给它赋任意值；也不能够在它上面调用任意的方法
// 而 any  可以调用任意方法，但编译为 js 就会出错，因为 boolean 类型调用了 string 类型的方法
// cout(notSure.substring(1));//不报错
let notSure1:Object =10;  
// notSure1.substring(1); //直接报错
let ayy1: any[] = [1, true, "free"];
// gbk  国标扩展码
cout(ayy1[0]) //下标从 0 开始

// void 类型与 any 类型相反： 表示没有任何类型
function warnUser(): void {
    cout("hello void");
}
warnUser();
// 它只能赋值 undefined  和 null
let s:void =undefined

// never 类型表示永远不存在的值的类型 
function error(message:string):never{
    throw new Error(message);
}

function fial(){
    return error("Something failed");
}

let someValue: any ='this is a  string';
let strLength: number = (<string>someValue).length;
cout(strLength)
// as 语法： 等价
let someValue1 : any ='this is a string';
let strLength1:number = (someValue1 as string).length;
cout(strLength1)

// let createboolean : boolean = new Boolean(1); //报错 换成 Boolean 就可以了
// new  Boolean 返回的是一个 Boolean 对象
let createboolean1:boolean = Boolean(1);//返回的是一个 boolean 类型

// 联合类型
let myFavoriteNumber : string | number;
myFavoriteNumber='syhk';
myFavoriteNumber=90;
// myFavoriteNumber 的含义是：只允许它是 string 或者是 number 类型，其他的不行
// 当不确定联合类型的变量是哪个类型的时候，只能访问联合类型的所有类型里共有的属性或方法
// 联合类型在赋值的时候会被推荐为某个类型


// ===============接口========================
interface Person{
    name:string;
    age:number;
}
// 定义变量的属性必须要和接口定义的一致
// 在赋值的时候，变量的开关必须和接口的开关保持一致
let tom : Person={
    name:'syhk',
    age:21
};

// 可选属性
interface Person1{
    name:string;
    age?:number;
}
// 用 ? 标识一个属性是可选的，这个属性可以不存在
let tom1:Person1={
    name: 'syhk'
}

// 任意属性
interface Person2{
    name:string;
    age?:number;
    [propName: string]:any;//任意属性
}
// 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集
let tom2: Person2={
    name:'syhk',
    age:21,
    gender:90
}
// 一个接口只能定义一个任意属性。如果接口中有多个类型的属性，可以在任意属性中使用联合类型
interface Person3{
    name:string;
    age?:number;
    [propName:string]:string|number;
};
let tom3:Person3={
    name:'syhk',
    age:34,
    sex:'man'
};
// 只读属性
interface Person4{
    readonly id:number;
    name:string;
    age?:number;
    [propName : string]:any;
}

// let tom4:Person4={
//     id:1028,
//     name:'syhk',
//     sex:'man'
// }
// 注释上面第一次给对象赋值，可以更改
// tom4.id=9000; //不能更改，因为它是只读的
// tom4.id=8000;//可以更改
//注意： 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候


//================数组=================================
// 定义方式：
let array1:number[] =[1,3,4,4];
let array2:Array<number> =[1,3];
// 接口方式
interface NumbersArray{
    [index:number]:number;
}
let fib:NumbersArray=[12,3,4,5];

// 函数的类型
// 函数声明
function sum(x :number, y :number): number {//在后面加 ： 然后指定返回类型
    return x+y;
}
// 函数表达式
let mySum = function(x,y){
    return x+y;
}
// 使用接口定义
interface SearchFunc{
    (source:string,subString :string):boolean;
}

let mySearch:SearchFunc;
mySearch=function(souce:string,subString:string){
    return souce.search(subString) !== -1;
}

// 可选参数  ? 标识是一个可选参数
function buildName(firstName:string , lastName?: string){
    if(lastName){
        return firstName+'  '+lastName;
    }else{
        return firstName;
    }
}
buildName("syhk",'sy');
buildName('syhk');
// 可选参数后面不允许出现必须参数

// 参数默认值 :给参数添加默认值  typescript 会识别为可选参数
function buildName1(firstName : string , lastName : string='cat'){
    return firstName+'   '+lastName;
}

cout(buildName1('sy','hk'));//syhk
cout(buildName1('sy'));//sy cat
// 它可以不在必需参数的后面
function buildName2(firstName:string = 'sy',lastName:string){
    return firstName+lastName;
}
cout(buildName2(undefined,'syhk'))
cout(buildName2('s','y'))
// 都是可以的

// 剩余参数： 多参数
function push(array,...items){
    items.forEach(function(item){
        array.push(item);
    }
    );
}

// 用数组定义上面
function push1(array:any[], ...items:any[]){
    items.forEach(function(item){
        array.push(item);
    });
}

let a :any[]=[]
push(a,1,3,4,5)
cout(a)
// 注意 ... 只能是最后一个参数

// ================重载=========================
function reverse(x:number | string):number|string|void{
    if(typeof x==='number'){
        return Number(x.toString().split('').reverse().join(''));
    }else if (typeof x ==='string'){
        return x.split('').reverse().join("");
    }
}
cout(reverse('syhk'))
cout(reverse(900))

// ==============类型断言=================
//  值  as 类型  或者  <类型>值 
// 用途：将一个联合类型断言为其中一个类型
interface Cat{
    name:string;
    run():void;
}

interface Fish{
    name:string;
    swim():void;
}

function getName(animal:Cat | Fish){
    return animal.name;//只能访问共有属性或者方法
}

function getName1(animal:Cat | Fish){
    return (animal as Fish).swim();//可以调用
}

// 将一个父类断言为更加具体的子类
class ApiError extends Error{
    code:number=0;
}
class HttpError extends Error{
    statusCode:number=200;
}

function isApiError(error:Error){
    if(typeof (error as ApiError).code == 'number'){
        return true;
    }
    return false;
}

// 将任何一个类型断言为 any
// window.foo = 1; 报错
// (window as any).foo=1;//不报错，因为any 类型的变量，可以访问任何属性

// 将 any 断言为一个具体的类型
// function getCacheData(key:string):any{
//     // return (window as any).cache[key];
// }
interface Cat{
    name:string;
    run():void;
}
// const tom5=getCacheData('tom') as Cat;
// tom5.run();
// 要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可，这也是为了在类型断言时的安全考虑，毕竟毫无根据的断言是非常危险的。
// 类型断言只会影响 Typescript 编译时的类型，类型断言语句会编译结果中会被删除
// 类型断言不是类型转换，它不会影响会真正的类型
function toBoolean(Something){
    return Something as Boolean;
}

cout(toBoolean(1));//返回值为 1 

// 类型转换
function toBoolean1(Something:any):boolean{
    return Boolean(Something);
}
cout(toBoolean1(1));//返回值为 true

// 声明文件： 声明文件必须以  .d.ts 为后缀


// 类型别名 使用 type 创建类型别名
type mystring = string;
let mys:mystring='syhk';

// 字符串字面量类型
type EventNames = 'click' | 'scroll' | 'mousemove';
// EventNames 它只能取上面三种结果中的一种
// 输出函数
function cout(v:any){
    console.log(v);
}
```

```json
{
    "compilerOptions": {
        "watch": true, //监听源代码,发生变化自动编译
        "removeComments": false,
        "target": "es6" // 指定 js 版本
    }
}
```

//====================================

TyepScipt 代码：

```typescript
// npm install -g typescript
// tsc 文件名.ts

let isDone : boolean =false;
// 数字类型都 number
let decLiteral : number=6
// 字符类型可以单引号或双引号
let name1: string  = "bob";
name1=`syhk`;

// 模板字符串:第一种被反引号包围
let stance: string=`my name is ${name1}`;
let stance1: string="my name is "+ (name1)+"";//同样的效果
let num1:number = 10
console.log(`be ${10+ num1}`) //支持运算
// 数组
let list:number[] = [1,2,3];
// 数组泛型
let list1: Array<number> =[1,2,3];
console.log(list)
console.log(list1)

// 元组 Tuple
let x:[string,number]=['hello',100];//赋值顺序要和声明顺序一样
console.log(x);
console.log(x[0].substring(1));
console.log(x[1]) //第二个是数字类型

// 枚举 默认编号从 0 开始为元素编号 
enum Color{Red,Green,Blue}
let c:Color =Color.Blue
// 也可以手动赋值编号 
enum Color1{Red =1,Green=2,Blue=4}
console.log(Color[0])
console.log(Color1[4])

//eg:动态内容 我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。
let notSure: any =4; 
//变量在声明的时候，未指定类型那么它就会被识别为任意值类型；
let notSure2 ="syhk";
//  notSure2= 200; //类型推论，上面已经被推断成了 string 类型
 
//  区别于上面：如果定义的时候没有赋值，不管之后有没有赋值，都被推断成 any 类型
 let notSure3;
 notSure3='syhk';
 notSure3=600;


notSure = 'maybe a string instead';
cout(notSure)
notSure=false;
cout(notSure)
// 和 Object 有相似的作用，但是它只允许给它赋任意值；也不能够在它上面调用任意的方法
// 而 any  可以调用任意方法，但编译为 js 就会出错，因为 boolean 类型调用了 string 类型的方法
// cout(notSure.substring(1));//不报错
let notSure1:Object =10;  
// notSure1.substring(1); //直接报错
let ayy1: any[] = [1, true, "free"];
// gbk  国标扩展码
cout(ayy1[0]) //下标从 0 开始

// void 类型与 any 类型相反： 表示没有任何类型
function warnUser(): void {
    cout("hello void");
}
warnUser();
// 它只能赋值 undefined  和 null
let s:void =undefined

// never 类型表示永远不存在的值的类型 
function error(message:string):never{
    throw new Error(message);
}

function fial(){
    return error("Something failed");
}

let someValue: any ='this is a  string';
let strLength: number = (<string>someValue).length;
cout(strLength)
// as 语法： 等价
let someValue1 : any ='this is a string';
let strLength1:number = (someValue1 as string).length;
cout(strLength1)

// let createboolean : boolean = new Boolean(1); //报错 换成 Boolean 就可以了
// new  Boolean 返回的是一个 Boolean 对象
let createboolean1:boolean = Boolean(1);//返回的是一个 boolean 类型

// 联合类型
let myFavoriteNumber : string | number;
myFavoriteNumber='syhk';
myFavoriteNumber=90;
// myFavoriteNumber 的含义是：只允许它是 string 或者是 number 类型，其他的不行
// 当不确定联合类型的变量是哪个类型的时候，只能访问联合类型的所有类型里共有的属性或方法
// 联合类型在赋值的时候会被推荐为某个类型


// ===============接口========================
interface Person{
    name:string;
    age:number;
}
// 定义变量的属性必须要和接口定义的一致
// 在赋值的时候，变量的开关必须和接口的开关保持一致
let tom : Person={
    name:'syhk',
    age:21
};

// 可选属性
interface Person1{
    name:string;
    age?:number;
}
// 用 ?: 标识一个属性是可选的，这个属性可以不存在
let tom1:Person1={
    name: 'syhk'
}

// 任意属性
interface Person2{
    name:string;
    age?:number;
    [propName: string]:any;//任意属性
}
// 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集
let tom2: Person2={
    name:'syhk',
    age:21,
    gender:90
}
// 一个接口只能定义一个任意属性。如果接口中有多个类型的属性，可以在任意属性中使用联合类型
interface Person3{
    name:string;
    age?:number;
    [propName:string]:string|number;
};
let tom3:Person3={
    name:'syhk',
    age:34,
    sex:'man'
};
// 只读属性
interface Person4{
    readonly id:number;
    name:string;
    age?:number;
    [propName : string]:any;
}

// let tom4:Person4={
//     id:1028,
//     name:'syhk',
//     sex:'man'
// }
// 注释上面第一次给对象赋值，可以更改
// tom4.id=9000; //不能更改，因为它是只读的
// tom4.id=8000;//可以更改
//注意： 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候


//================数组=================================
// 定义方式：
let array1:number[] =[1,3,4,4];
let array2:Array<number> =[1,3];
// 接口方式
interface NumbersArray{
    [index:number]:number;
}
let fib:NumbersArray=[12,3,4,5];

// 函数的类型
// 函数声明
function sum(x :number, y :number): number {//在后面加 ： 然后指定返回类型
    return x+y;
}
// 函数表达式
let mySum = function(x,y){
    return x+y;
}
// 使用接口定义
interface SearchFunc{
    (source:string,subString :string):boolean;
}

let mySearch:SearchFunc;
mySearch=function(souce:string,subString:string){
    return souce.search(subString) !== -1;
}

// 可选参数  ?: 标识是一个可选参数
function buildName(firstName:string , lastName?: string){
    if(lastName){
        return firstName+'  '+lastName;
    }else{
        return firstName;
    }
}
buildName("syhk",'sy');
buildName('syhk');
// 可选参数后面不允许出现必须参数

// 参数默认值 :给参数添加默认值  typescript 会识别为可选参数
function buildName1(firstName : string , lastName : string='cat'){
    return firstName+'   '+lastName;
}

cout(buildName1('sy','hk'));//syhk
cout(buildName1('sy'));//sy cat
// 它可以不在必需参数的后面
function buildName2(firstName:string = 'sy',lastName:string){
    return firstName+lastName;
}
cout(buildName2(undefined,'syhk'))
cout(buildName2('s','y'))
// 都是可以的

// 剩余参数： 多参数
function push(array,...items){
    items.forEach(function(item){
        array.push(item);
    }
    );
}

// 用数组定义上面
function push1(array:any[], ...items:any[]){
    items.forEach(function(item){
        array.push(item);
    });
}

let a :any[]=[]
push(a,1,3,4,5)
cout(a)
// 注意 ... 只能是最后一个参数

// ================重载=========================
function reverse(x:number | string):number|string|void{
    if(typeof x==='number'){
        return Number(x.toString().split('').reverse().join(''));
    }else if (typeof x ==='string'){
        return x.split('').reverse().join("");
    }
}
cout(reverse('syhk'))
cout(reverse(900))

// ==============类型断言=================
//  值  as 类型  或者  <类型>值 
// 用途：将一个联合类型断言为其中一个类型
interface Cat{
    name:string;
    run():void;
}

interface Fish{
    name:string;
    swim():void;
}

function getName(animal:Cat | Fish){
    return animal.name;//只能访问共有属性或者方法
}

function getName1(animal:Cat | Fish){
    return (animal as Fish).swim();//可以调用
}

// 将一个父类断言为更加具体的子类
class ApiError extends Error{
    code:number=0;
}
class HttpError extends Error{
    statusCode:number=200;
}

function isApiError(error:Error){
    if(typeof (error as ApiError).code == 'number'){
        return true;
    }
    return false;
}

// 将任何一个类型断言为 any
// window.foo = 1; 报错
// (window as any).foo=1;//不报错，因为any 类型的变量，可以访问任何属性

// 将 any 断言为一个具体的类型
// function getCacheData(key:string):any{
//     // return (window as any).cache[key];
// }
interface Cat{
    name:string;
    run():void;
}
// const tom5=getCacheData('tom') as Cat;
// tom5.run();
// 要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可，这也是为了在类型断言时的安全考虑，毕竟毫无根据的断言是非常危险的。
// 类型断言只会影响 Typescript 编译时的类型，类型断言语句会编译结果中会被删除
// 类型断言不是类型转换，它不会影响会真正的类型
function toBoolean(Something){
    return Something as Boolean;
}

cout(toBoolean(1));//返回值为 1 

// 类型转换
function toBoolean1(Something:any):boolean{
    return Boolean(Something);
}
cout(toBoolean1(1));//返回值为 true

// 声明文件： 声明文件必须以  .d.ts 为后缀


// 类型别名 使用 type 创建类型别名
type mystring = string;
let mys:mystring='syhk';

// 字符串字面量类型
type EventNames = 'click' | 'scroll' | 'mousemove';
// EventNames 它只能取上面三种结果中的一种
// 输出函数
function cout(v:any){
    console.log(v);
}

// 类
class Animal{
    public name;
    constructor(name){
        this.name=name;
    }
    sayHi(){
        return `My name is ${this.name}`;
    }
}

let sya = new Animal("syhk");
cout(sya.sayHi());
// 类的继承 
class Cat extends Animal{
    constructor(name){
        super(name);
        cout(this.name);
    }
    sayHi(): string {
        return `Meow`+super.sayHi();
    }
}

let syc = new Cat("sytom");
cout(syc.sayHi());

// 存取器 也就是 getter 和 setter
// class Animal1{
//     private name1;
//     constructor(name){
//         this.name1=name;
//     }
//     get name1(){
//         return this.name;
//     }
//     set name1(value){
//         cout("setter"+value);
//     }
// }

// /**
//  * Accessors are only available when targeting ECMAScript 5 and higher.
//  * 会出现 这个错误，意思是说访问器（存取器）只能在 ECMAScript 5 及更高版本使用
//  */

// let sya1 = new Animal1("Kitty");
// sya1.name1='shenyang';
// cout(sya1.name1)

// 静态方法
class Animal2{
    static isAnimal2(a){
        return a instanceof Animal2; //传入进来的 a 是否属于这个类
    }
}
let syan =new Animal2();
cout(Animal2.isAnimal2(syan));//返回结果为 true

// 类只有三种访问修饰符： public  private protected
// 当构造函数为 private 时，这个类不允许被继承或者实例化
// 构造函数被 protected 修饰时，这个类只允许被继承
// 参数属性：修饰符和 readonly 还可以使用构造函数中，等同于类定义这个属性同时给该属性赋值
class Animal3{
    // public name2:string;  //在下面构造函数参数中写了，构造函数外面就不用写了，
    public constructor(public name2:string) {//这个等同于定义该属性同时又给这个属性赋值
        this.name2=name2;
    }
}

// readonly : 只允许出现在属性声明或索引签名或构造函数中
class Animal4{
    readonly name;
    public constructor(name){
        this.name=name;
    }
}

let ssa=new Animal4("syhk");
cout(ssa.name);
// ssa.name="sss"; 报错，因为它是只读的不能进行修改
// 注意： readonly 和其他访问修饰符同时存在的话，需要写其后面
class Animal5{
    public constructor(public readonly name){
        this.name=name;
    }
}

// 抽象类 abstract
abstract class Animal6{
    readonly name;
    public constructor(name){
        this.name=name;
    }
    public abstract sayHi();
}
// 抽象类是不允许实例化的
// 抽象类中的抽象方法必须被子类实现

class Cat1 extends Animal6{
    public eat(){
        cout(`${this.name} is eating`);
    }
    public sayHi() {
        cout("父类中的方法");
    }
}

//类的类型
class Animal7{
    name:string;
    constructor(name:string){
        this.name=name;
    }
    sayHi():string{
        return `My name is ${this.name}`;
    }
}
let sja: Animal7=new  Animal7("syhk");
cout(sja.sayHi());

// 类与接口
interface Alarm{
    alert():void;
}

class Door{

}

class SecurityDoor extends Door implements Alarm{
alert(): void {
    cout("SecurityDoor alert");
}
}

class Car1 implements Alarm{
    alert(): void {
        cout("Cat alert");
    }
}
// 一个类可以实现多个接口
interface Alarm1{
    alert():void;
}

interface Light{
    lightOn():void;
    lightoff():void;
}

class Car implements Alarm1,Light{
    alert(): void {
        cout("Car alert");
    }
    lightOn(): void {
        cout("Car light on");
    }
    lightoff(): void {
        cout("Car light off");
    }
}

// 接口继承接口
interface Alarm11{
    alert():void;
}
interface LightableAlarm extends Alarm{
    lightOn():void;
    ligthOff():void;
}

// 接口可以继承类
class point{
    x:number;
    y:number;
    constructor(x:number,y:number){
        this.x=x;
        this.y=y;
    }
}
interface Poin3d extends point{
    z:number;
}

let point3d:Poin3d ={x:1,y:2,z:3};

// 泛型
function createArray (length:number, value:any): Array<any>{
    let result=[];
    for(let i = 0; i < length ; i++){
        result[i]=value;
    }
    return result;
}

createArray(3,"x");
cout(createArray(4,"syhk"));

// 泛型
function createArray1<T>(length:number, value:T):Array<T>{
    let result:T[] = [];
    for(let i =0; i < length ; i++){
        result[i]=value;
    }
    return result;
}
cout(createArray1<string>(5,"syhk"));

// 多个类型参数
function swap<T,U> (tuple:[T,U]):[  U,T]{//返回参数也要交换位置
    return [tuple[1],tuple[0]];
}

// 泛型约束

interface Lengthwise{
    length:number;
}

// 加上泛型约束后就不会报错了
function loggingIdentity<T extends Lengthwise> (arg:T):T{
    cout(arg.length);
    return arg;
}

// 泛型接口
interface SearchFunc1{
    (source:string, subString:string):boolean;
}

let mySearch1:SearchFunc1;
mySearch1 = function(source:string , subString:string){
    return source.search(subString) !== -1;
}

interface CreateArrayFunc{
    <T>(length:number,value:T) : Array<T>;
}

let createArray4:CreateArrayFunc;
createArray4 =function<T>(length:number,value:T):Array<T>{
    let result:T[]=[];
    for(let i = 0 ; i<length;i++){
        result[i]=value;
    }
    return result;
}
cout(createArray4(10,"x"));

// 泛型类
class GenericNumber<T>{
    zeroValue:T;
    add:(x:T,y:T)=>T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue=0;
myGenericNumber.add=function(x,y){return x+y;};

// Symbols
// let sym1 = Symbol();
// let sym2 = Symbol("key");
// let sym3 = Symbol("key");
// cout(sym2===sym3);
```

生成的 JavaScript 代码：

```java
var __extends = (this && this.__extends) || (function () {
    var extendStatics = function (d, b) {
        extendStatics = Object.setPrototypeOf ||
            ({ __proto__: [] } instanceof Array && function (d, b) { d.__proto__ = b; }) ||
            function (d, b) { for (var p in b) if (Object.prototype.hasOwnProperty.call(b, p)) d[p] = b[p]; };
        return extendStatics(d, b);
    };
    return function (d, b) {
        if (typeof b !== "function" && b !== null)
            throw new TypeError("Class extends value " + String(b) + " is not a constructor or null");
        extendStatics(d, b);
        function __() { this.constructor = d; }
        d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
    };
})();
// npm install -g typescript
// tsc 文件名.ts
var isDone = false;
// 数字类型都 number
var decLiteral = 6;
// 字符类型可以单引号或双引号
var name1 = "bob";
name1 = "syhk";
// 模板字符串:第一种被反引号包围
var stance = "my name is ".concat(name1);
var stance1 = "my name is " + (name1) + ""; //同样的效果
var num1 = 10;
console.log("be ".concat(10 + num1)); //支持运算
// 数组
var list = [1, 2, 3];
// 数组泛型
var list1 = [1, 2, 3];
console.log(list);
console.log(list1);
// 元组 Tuple
var x = ['hello', 100]; //赋值顺序要和声明顺序一样
console.log(x);
console.log(x[0].substring(1));
console.log(x[1]); //第二个是数字类型
// 枚举 默认编号从 0 开始为元素编号 
var Color;
(function (Color) {
    Color[Color["Red"] = 0] = "Red";
    Color[Color["Green"] = 1] = "Green";
    Color[Color["Blue"] = 2] = "Blue";
})(Color || (Color = {}));
var c = Color.Blue;
// 也可以手动赋值编号 
var Color1;
(function (Color1) {
    Color1[Color1["Red"] = 1] = "Red";
    Color1[Color1["Green"] = 2] = "Green";
    Color1[Color1["Blue"] = 4] = "Blue";
})(Color1 || (Color1 = {}));
console.log(Color[0]);
console.log(Color1[4]);
//eg:动态内容 我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。
var notSure = 4;
//变量在声明的时候，未指定类型那么它就会被识别为任意值类型；
var notSure2 = "syhk";
//  notSure2= 200; //类型推论，上面已经被推断成了 string 类型
//  区别于上面：如果定义的时候没有赋值，不管之后有没有赋值，都被推断成 any 类型
var notSure3;
notSure3 = 'syhk';
notSure3 = 600;
notSure = 'maybe a string instead';
cout(notSure);
notSure = false;
cout(notSure);
// 和 Object 有相似的作用，但是它只允许给它赋任意值；也不能够在它上面调用任意的方法
// 而 any  可以调用任意方法，但编译为 js 就会出错，因为 boolean 类型调用了 string 类型的方法
// cout(notSure.substring(1));//不报错
var notSure1 = 10;
// notSure1.substring(1); //直接报错
var ayy1 = [1, true, "free"];
// gbk  国标扩展码
cout(ayy1[0]); //下标从 0 开始
// void 类型与 any 类型相反： 表示没有任何类型
function warnUser() {
    cout("hello void");
}
warnUser();
// 它只能赋值 undefined  和 null
var s = undefined;
// never 类型表示永远不存在的值的类型 
function error(message) {
    throw new Error(message);
}
function fial() {
    return error("Something failed");
}
var someValue = 'this is a  string';
var strLength = someValue.length;
cout(strLength);
// as 语法： 等价
var someValue1 = 'this is a string';
var strLength1 = someValue1.length;
cout(strLength1);
// let createboolean : boolean = new Boolean(1); //报错 换成 Boolean 就可以了
// new  Boolean 返回的是一个 Boolean 对象
var createboolean1 = Boolean(1); //返回的是一个 boolean 类型
// 联合类型
var myFavoriteNumber;
myFavoriteNumber = 'syhk';
myFavoriteNumber = 90;
// 定义变量的属性必须要和接口定义的一致
// 在赋值的时候，变量的开关必须和接口的开关保持一致
var tom = {
    name: 'syhk',
    age: 21
};
// 用 ?: 标识一个属性是可选的，这个属性可以不存在
var tom1 = {
    name: 'syhk'
};
// 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集
var tom2 = {
    name: 'syhk',
    age: 21,
    gender: 90
};
;
var tom3 = {
    name: 'syhk',
    age: 34,
    sex: 'man'
};
// let tom4:Person4={
//     id:1028,
//     name:'syhk',
//     sex:'man'
// }
// 注释上面第一次给对象赋值，可以更改
// tom4.id=9000; //不能更改，因为它是只读的
// tom4.id=8000;//可以更改
//注意： 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候
//================数组=================================
// 定义方式：
var array1 = [1, 3, 4, 4];
var array2 = [1, 3];
var fib = [12, 3, 4, 5];
// 函数的类型
// 函数声明
function sum(x, y) {
    return x + y;
}
// 函数表达式
var mySum = function (x, y) {
    return x + y;
};
var mySearch;
mySearch = function (souce, subString) {
    return souce.search(subString) !== -1;
};
// 可选参数  ?: 标识是一个可选参数
function buildName(firstName, lastName) {
    if (lastName) {
        return firstName + '  ' + lastName;
    }
    else {
        return firstName;
    }
}
buildName("syhk", 'sy');
buildName('syhk');
// 可选参数后面不允许出现必须参数
// 参数默认值 :给参数添加默认值  typescript 会识别为可选参数
function buildName1(firstName, lastName) {
    if (lastName === void 0) { lastName = 'cat'; }
    return firstName + '   ' + lastName;
}
cout(buildName1('sy', 'hk')); //syhk
cout(buildName1('sy')); //sy cat
// 它可以不在必需参数的后面
function buildName2(firstName, lastName) {
    if (firstName === void 0) { firstName = 'sy'; }
    return firstName + lastName;
}
cout(buildName2(undefined, 'syhk'));
cout(buildName2('s', 'y'));
// 都是可以的
// 剩余参数： 多参数
function push(array) {
    var items = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        items[_i - 1] = arguments[_i];
    }
    items.forEach(function (item) {
        array.push(item);
    });
}
// 用数组定义上面
function push1(array) {
    var items = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        items[_i - 1] = arguments[_i];
    }
    items.forEach(function (item) {
        array.push(item);
    });
}
var a = [];
push(a, 1, 3, 4, 5);
cout(a);
// 注意 ... 只能是最后一个参数
// ================重载=========================
function reverse(x) {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    }
    else if (typeof x === 'string') {
        return x.split('').reverse().join("");
    }
}
cout(reverse('syhk'));
cout(reverse(900));
function getName(animal) {
    return animal.name; //只能访问共有属性或者方法
}
function getName1(animal) {
    return animal.swim(); //可以调用
}
// 将一个父类断言为更加具体的子类
var ApiError = /** @class */ (function (_super) {
    __extends(ApiError, _super);
    function ApiError() {
        var _this = _super !== null && _super.apply(this, arguments) || this;
        _this.code = 0;
        return _this;
    }
    return ApiError;
}(Error));
var HttpError = /** @class */ (function (_super) {
    __extends(HttpError, _super);
    function HttpError() {
        var _this = _super !== null && _super.apply(this, arguments) || this;
        _this.statusCode = 200;
        return _this;
    }
    return HttpError;
}(Error));
function isApiError(error) {
    if (typeof error.code == 'number') {
        return true;
    }
    return false;
}
// const tom5=getCacheData('tom') as Cat;
// tom5.run();
// 要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可，这也是为了在类型断言时的安全考虑，毕竟毫无根据的断言是非常危险的。
// 类型断言只会影响 Typescript 编译时的类型，类型断言语句会编译结果中会被删除
// 类型断言不是类型转换，它不会影响会真正的类型
function toBoolean(Something) {
    return Something;
}
cout(toBoolean(1)); //返回值为 1 
// 类型转换
function toBoolean1(Something) {
    return Boolean(Something);
}
cout(toBoolean1(1)); //返回值为 true
var mys = 'syhk';
// EventNames 它只能取上面三种结果中的一种
// 输出函数
function cout(v) {
    console.log(v);
}
// 类
var Animal = /** @class */ (function () {
    function Animal(name) {
        this.name = name;
    }
    Animal.prototype.sayHi = function () {
        return "My name is ".concat(this.name);
    };
    return Animal;
}());
var sya = new Animal("syhk");
cout(sya.sayHi());
// 类的继承 
var Cat = /** @class */ (function (_super) {
    __extends(Cat, _super);
    function Cat(name) {
        var _this = _super.call(this, name) || this;
        cout(_this.name);
        return _this;
    }
    Cat.prototype.sayHi = function () {
        return "Meow" + _super.prototype.sayHi.call(this);
    };
    return Cat;
}(Animal));
var syc = new Cat("sytom");
cout(syc.sayHi());
// 存取器 也就是 getter 和 setter
// class Animal1{
//     private name1;
//     constructor(name){
//         this.name1=name;
//     }
//     get name1(){
//         return this.name;
//     }
//     set name1(value){
//         cout("setter"+value);
//     }
// }
// /**
//  * Accessors are only available when targeting ECMAScript 5 and higher.
//  * 会出现 这个错误，意思是说访问器（存取器）只能在 ECMAScript 5 及更高版本使用
//  */
// let sya1 = new Animal1("Kitty");
// sya1.name1='shenyang';
// cout(sya1.name1)
// 静态方法
var Animal2 = /** @class */ (function () {
    function Animal2() {
    }
    Animal2.isAnimal2 = function (a) {
        return a instanceof Animal2; //传入进来的 a 是否属于这个类
    };
    return Animal2;
}());
var syan = new Animal2();
cout(Animal2.isAnimal2(syan)); //返回结果为 true
// 类只有三种访问修饰符： public  private protected
// 当构造函数为 private 时，这个类不允许被继承或者实例化
// 构造函数被 protected 修饰时，这个类只允许被继承
// 参数属性：修饰符和 readonly 还可以使用构造函数中，等同于类定义这个属性同时给该属性赋值
var Animal3 = /** @class */ (function () {
    // public name2:string;  //在下面构造函数参数中写了，构造函数外面就不用写了，
    function Animal3(name2) {
        this.name2 = name2;
        this.name2 = name2;
    }
    return Animal3;
}());
// readonly : 只允许出现在属性声明或索引签名或构造函数中
var Animal4 = /** @class */ (function () {
    function Animal4(name) {
        this.name = name;
    }
    return Animal4;
}());
var ssa = new Animal4("syhk");
cout(ssa.name);
// ssa.name="sss"; 报错，因为它是只读的不能进行修改
// 注意： readonly 和其他访问修饰符同时存在的话，需要写其后面
var Animal5 = /** @class */ (function () {
    function Animal5(name) {
        this.name = name;
        this.name = name;
    }
    return Animal5;
}());
// 抽象类 abstract
var Animal6 = /** @class */ (function () {
    function Animal6(name) {
        this.name = name;
    }
    return Animal6;
}());
// 抽象类是不允许实例化的
// 抽象类中的抽象方法必须被子类实现
var Cat1 = /** @class */ (function (_super) {
    __extends(Cat1, _super);
    function Cat1() {
        return _super !== null && _super.apply(this, arguments) || this;
    }
    Cat1.prototype.eat = function () {
        cout("".concat(this.name, " is eating"));
    };
    Cat1.prototype.sayHi = function () {
        cout("父类中的方法");
    };
    return Cat1;
}(Animal6));
//类的类型
var Animal7 = /** @class */ (function () {
    function Animal7(name) {
        this.name = name;
    }
    Animal7.prototype.sayHi = function () {
        return "My name is ".concat(this.name);
    };
    return Animal7;
}());
var sja = new Animal7("syhk");
cout(sja.sayHi());
var Door = /** @class */ (function () {
    function Door() {
    }
    return Door;
}());
var SecurityDoor = /** @class */ (function (_super) {
    __extends(SecurityDoor, _super);
    function SecurityDoor() {
        return _super !== null && _super.apply(this, arguments) || this;
    }
    SecurityDoor.prototype.alert = function () {
        cout("SecurityDoor alert");
    };
    return SecurityDoor;
}(Door));
var Car1 = /** @class */ (function () {
    function Car1() {
    }
    Car1.prototype.alert = function () {
        cout("Cat alert");
    };
    return Car1;
}());
var Car = /** @class */ (function () {
    function Car() {
    }
    Car.prototype.alert = function () {
        cout("Car alert");
    };
    Car.prototype.lightOn = function () {
        cout("Car light on");
    };
    Car.prototype.lightoff = function () {
        cout("Car light off");
    };
    return Car;
}());
// 接口可以继承类
var point = /** @class */ (function () {
    function point(x, y) {
        this.x = x;
        this.y = y;
    }
    return point;
}());
var point3d = { x: 1, y: 2, z: 3 };
// 泛型
function createArray(length, value) {
    var result = [];
    for (var i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
createArray(3, "x");
cout(createArray(4, "syhk"));
// 泛型
function createArray1(length, value) {
    var result = [];
    for (var i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
cout(createArray1(5, "syhk"));
// 多个类型参数
function swap(tuple) {
    return [tuple[1], tuple[0]];
}
// 加上泛型约束后就不会报错了
function loggingIdentity(arg) {
    cout(arg.length);
    return arg;
}
var mySearch1;
mySearch1 = function (source, subString) {
    return source.search(subString) !== -1;
};
var createArray4;
createArray4 = function (length, value) {
    var result = [];
    for (var i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
};
cout(createArray4(10, "x"));
// 泛型类
var GenericNumber = /** @class */ (function () {
    function GenericNumber() {
    }
    return GenericNumber;
}());
var myGenericNumber = new GenericNumber();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) { return x + y; };
// Symbols
var sym1 = Symbol();
var sym2 = Symbol("key");
var sym3 = Symbol("key");
cout(sym2 === sym3);
```

## 操作 DOM 和 BOM
文档对象模型（Document Object Model，简称DOM），是 [W3C](https://baike.baidu.com/item/W3C) 组织推荐的处理[可扩展标记语言](https://baike.baidu.com/item/%E5%8F%AF%E6%89%A9%E5%B1%95%E7%BD%AE%E6%A0%87%E8%AF%AD%E8%A8%80)（html或者xhtml）的标准[编程接口](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E6%8E%A5%E5%8F%A3)。
```typescript
<!DOCTYPE html>
  <html lang="en">
    <head>
  <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    </head>
    <style>
    div {
    width: 100px;
    height: 100px;
    background-color: tomato;
  }
</style>
  <body>
  <p id="p">p</p>
  <div id="d1"></div>

  <script lang="ts">
    document.title = "syhk";
let p = document.getElementById("p");
p.innerText = "ts";
p.style.color = "red";
p.style.fontSize = "100px";
let myts = document.createElement("script");
p.setAttribute("type", "text/typescript");
document.getElementsByTagName("body");
//  创建一个标签节点，相当于： <style></style>
let ms = document.createElement("style");
//  指定属性 相当于：  type="text/css"
ms.setAttribute("type", "text/css");
// 写相关内容
ms.innerHTML = "body{background-color:tomato;}";
//   将上面写好的加入到 head 标签中
document.getElementsByTagName("head")[0].appendChild(ms);
console.log('====================================');
console.log(document.getElementsByTagName('head')[0]);// 拿到 head 标签
</script>
  </body>
  </html>
```
打印的 Head 节点：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665316590122-06d2bf76-4e89-4f56-8a9e-0c58eee5fb58.png#averageHue=%23fefdfd&clientId=u6ba5dfc2-b583-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=179&id=u4dc9d128&margin=%5Bobject%20Object%5D&name=image.png&originHeight=224&originWidth=807&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27462&status=done&style=none&taskId=u4dc211e8-02a2-4152-92d1-4623bbd78b1&title=&width=645.6)

```html
 <p id="demo"></p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <button>点击</button>

    <script lang="ts">
      document.getElementById("demo").innerHTML = "<h1>Hello world!</h1>"; //更改元素内容 可以渲染 html
      let p = (document.getElementById("demo").innerText =
        "<h1>Hello world!</h1>"); //更改元素内容不能渲染 Html

      console.log(p);
      console.log("===============================");
      console.log(typeof p);

      let plist = document.getElementsByTagName("p");
      //   plist 获取到的动态集合，当页面新增标签时，这个集合里面也会增加
      console.log(plist);
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665318864828-85440e36-b0a9-4a4c-8c17-3c79089952d7.png#averageHue=%23fefdfd&clientId=u6ba5dfc2-b583-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=265&id=u87d93a06&margin=%5Bobject%20Object%5D&name=image.png&originHeight=331&originWidth=666&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28950&status=done&style=none&taskId=u0fce2aaa-cdf9-4fe3-ab85-58946fd50ad&title=&width=532.8)
w3c 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <style>
    .box1 {
      width: 200px;
      height: 200px;
      background-color: tomato;
    }

    .box2 {
      width: 200px;
      height: 200px;
      background-color: teal;
    }

    p {
      background-color: rebeccapurple;
    }
  </style>
  <body>
    <p id="demo"></p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <p>1</p>
    <button id="btn">点击</button>
    <div class="box1">box1</div>
    <div class="box1">box1</div>
    <div class="box2">box1</div>
    <img src="" alt="暂时没有图像" />

    <button>点击</button>
    <input type="text" value="内容" />

    <li><img src="./images/1.png" alt="" /></li>
    <li><img src="./images/2.jpg" alt="" /></li>
    <li><img src="./images/1.png" alt="" /></li>
    <li><img src="./images/2.jpg" alt="" /></li>

    <script lang="ts">
      document.getElementById("demo").innerHTML = "<h1>Hello world!</h1>"; //更改元素内容 可以渲染 html
      let p = (document.getElementById("demo").innerText =
        "<h1>Hello world!</h1>"); //更改元素内容不能渲染 Html
      // - 获取内容时的区别：
      // 	innerText会去除空格和换行，而innerHTML会保留空格和换行
      // - 设置内容时的区别：
      // 	innerText不会识别html，而innerHTML会识别
      console.log(p);
      console.log("===============================");
      console.log(typeof p);

      let plist = document.getElementsByTagName("p");
      //   plist 获取到的动态集合，当页面新增标签时，这个集合里面也会增加
      //通过下标去访问，下标从 0 开始
      console.log(plist);
      console.log(plist[0]);

      // H5 新增方式
      // querySelector 返回指定选择器的第一元素对象（第一个）
      let box1 = document.querySelector(".box1");
      console.log(box1);
      // 返回指定选择器的元素集合
      let box1list = document.querySelectorAll(".box1");
      console.log(box1list);

      // 获取特殊元素对象
      // 获取  body
      console.log(document.body);
      console.log("================================");
      // 获取 html 文档对象
      console.log(document.documentElement);

      // 事件
      let btn = document.querySelector("#btn");
      // btn.onclick = () => alert("123456");
      btn.onclick = () => {
        box1list[2].innerHTML = new Date();
      };

      // 事件步骤： 获取事件源  绑定事件   添加事件处理程序
      let img = document.querySelector("img");
      img.onclick = function () {
        // 获取元素的属性并设置值
        img.src = "./images/1.png";
        img.title = "动漫";
        img.alt = "现在有图像了";
        console.log("+1");
        console.log(img);
      };

      btnlist = document.querySelectorAll("button");
      input = document.querySelectorAll("input");
      btnlist[1].onclick = function () {
        console.log(input[0].value);
        // 禁用按钮 this 指向当前函数的调用者
        this.disabled = true;
        // 可通过 style 属性去修改样式
        this.style.backgroundColor = "tomato";
        this.style.width = "120px";
        // element.style 里面的属性是采用 驼峰命名法 ================重要=======记住这个命名法
      };
      // 功能比较简单，或者是样式结构比较简单，用上面的方式
      // 一般功能比较复杂或者结构比较复杂 用 className 比较好

      btnlist[1].onclick = function () {
        // 如果有多个使用 空格 分隔
        this.className = "box1 box2";
      };

      // 排他操作:把其他所有元素的样式去掉,只留下自己的
      btnlist[1].onclick = function () {
        for (let i = 0; i < plist.length; i++) {
          plist[i].style.backgroundColor = "";
          console.log("+2");
        }
        // this.style.backgroundColor="purple";
        plist[plist.length - 1].style.backgroundColor = "red";
      };
      // 百度换肤案例

      imglist = document.querySelectorAll("img");
      let lilist = document.querySelectorAll("li");

      for (let i = 0; i < lilist.length; i++) {
        // 这里是 i , 可以根据点击哪个更换为哪个
        imglist[i].onclick = function () {
          document.body.style.backgroundImage = "url(" + this.src + ")";
        };
      }
    </script>
  </body>
</html>

```
#### html2
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div class="nav" id="demo" index="1"></div>
    <div data-my="syhk">自定义属性</div>
    <div>
      parent

      <p>child</p>
    </div>
    

    <textarea name="nyb" id="" cols="30" rows="10"></textarea>
    <button>发布</button>
    <ul></ul>
    

    <tbody>
      <table></table>
    </tbody>

    <div class="inner"></div>
    <script>
      // 获取元素的属性值
      let div = document.querySelector("div");
      // 设置属性值
      div.id = "syhk";
      div.className = "syhkc"; //设置 class 属性要用 className
      div.setAttribute("id", "syhkid");
      //   class 特殊
      div.setAttribute("class", "syhkclass");
      // 语法 : document.属性
      console.log(div.id);
      console.log(div.className);
      // get 方式
      console.log(div.getAttribute("id"));
      // 移除属性
      div.removeAttribute("index");

      console.log(div.getAttribute("index"));

      //   H5 自定义属性： 是为了保存并使用数据，有些数据可以不用存放在数据库而是直接存放在页面中
      // H5 规定自定义属性 data- 开头做为属性名并且赋值
      let divall = document.querySelectorAll("div");
      console.log(divall[1].getAttribute("data-my"));
      //   设置
      divall[1].setAttribute("data-my1", 212);

      //   节点操作
      // 获取父级节点
      console.log(divall[2].parentNode);
      // 获取所有子级节点 这个不提倡使用 标准
      console.log(divall[2].childNodes); //返回 3个
      //   非标准 ， 是一个只读属性，返回所有的子元素节点，只返回子元素节点不包含其他的
      console.log(divall[2].children); //返回 1个
      console.log(divall[2].firstChild); //第一个
      console.log(divall[2].lastChild); //最后一个
      console.log(divall[2].firstElementChild);
      console.log(divall[2].lastElementChild);
      //   区别和上面一样

      // 创建节点
      let md = document.createElement("div");
      // 追加节点
      divall[2].append(md);
      //   简单留言板案例
      let btn = document.querySelector("button");
      let text = document.querySelector("textarea");
      let ul = document.querySelector("ul");
      btn.onclick = function () {
        if (text.value == "") {
          alert("没有输入内容");
        } else {
          let li = document.createElement("li");
          // 复制和克隆节点 node.cloneNode(true | false ) 括号里面是 false 是浅拷贝 是 true 是深拷贝
          let li1 = li.cloneNode(true);
          li.innerHTML = text.value + "<button id='ly'>删除</button>";
          ul.insertBefore(li, ul.children[0]); //插入在某个节点元素之前
          //   let rli = ul.removeChild(ul.children[0]); //移除一个节点并返回删除的节点
          //   console.log(rli);
          let lybtn = document.querySelectorAll("button");
          for (let i = 1; i < lybtn.length; i++) {
            lybtn[i].onclick = function () {
              ul.removeChild(this.parentNode); //它的父级元素是 li
            };
          }
          text.value = "";
        }
      };
      //   数组数据
      let datas = [
        {
          name: "syhk1",
          age: 29,
          sex: "男",
        },
        {
          name: "syhk2",
          age: 29,
          sex: "男",
        },
        {
          name: "syhk3",
          age: 29,
          sex: "男",
        },
        {
          name: "syhk4",
          age: 29,
          sex: "男",
        },
        {
          name: "syhk5",
          age: 29,
          sex: "男",
        },
      ];
      //   动态生成表格
      let tbody = document.querySelector("table");
      console.log(tbody);
      //   遍历数据
      for (let i = 0; i < datas.length; i++) {
        let tr = document.createElement("tr");
        //  创建表格的行
        tbody.appendChild(tr);
        // 现创建单元格，取决于对象的属性有几个需要显示
        for (let k in datas[i]) {
          let td = document.createElement("td");
          td.innerHTML = datas[i][k];
          tr.appendChild(td);
        }
        // 创建有删除的单元格
        let td = document.createElement("td");
        td.innerHTML = "<button>del<button>";
        tr.appendChild(td);
      }
      //   删除操作
      let del = document.querySelectorAll("button");
      for (let i = 0; i < del.length; i++) {
        del[i].onclick = function () {
          // button -> tr  -> td
          tbody.removeChild(this.parentNode.parentNode);
        };
      }
      //   三种创建元素方式区别
      let btn1 = document.querySelector("button");
      btn1.onclick = function () {
        document.write("<div>syhk</div>");
      };
      //二：
      //   let inner = document.querySelector(".inner");
      //   console.log(inner);
      //   for (let i = 0; i <= 100; i++) {
      //     inner.innerHTML += '<a href="#">百度</a>';
      //   }
      //    下面的数组方式要比上面拼接方式效率高
      //   let arr = [];
      //   for (let i = 0; i <= 100; i++) {
      //     arr.push('<a href="#">百度</a>');
      //   }
      //   inner.innerHTML = arr.join("");
      // 三：
      let create = document.querySelector(".inner");
      console.log(create);
      for (let i = 0; i <= 100; i++) {
        let a = document.createElement("a");
        create.appendChild(a);
      }
      //   总结 ： 不同浏览器下， innerHTML 效率要比 createElement 要高
    </script>
  </body>
</html>
```
##### 事件高级
> 给元素添加事件称为 注册事件或者绑定事件
> 注册事件有两种方式：传统方式 和 监听注册方式

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665659763026-d53c9688-493e-4d46-995a-3c90fa208083.png#averageHue=%23f9f8f7&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=214&id=u8bc6df51&margin=%5Bobject%20Object%5D&name=image.png&originHeight=268&originWidth=893&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76309&status=done&style=none&taskId=u454a5713-7a63-4ec8-bc7e-59b318835f0&title=&width=714.4)
#### 事件监听
##### addEventListener() 事件监听
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>事件监听</title>
  </head>
  <body>
    <button>传统注册事件</button>
    <button>方法监听注册事件</button>
    <button>ie9 attachEvent</button>

    <script>
      let btns = document.querySelectorAll("button");
      // 传统方式注册事件
      btns[0].onclick = function () {
        alert("hi");
      };
      btns[0].onclick = function () {
        //被点击后只会调用这个函数
        alert("hao a u");
      };
      //   传统删除事件
      btns[0].onclick = null;

      //   事件侦听注册事件 addEventListener
      //   有三个参数：第一个是事件的类型 第二个是事件处理函数 第三个后面再看
      // 里面的事件类型是字符串，必须加引号而且不带 on
      // 同一个元素 同一个事件可以添加多个侦听器
      btns[1].addEventListener("click", () => alert(22));
      btns[1].addEventListener("click", fn); //这里的 fn 不用加小括号
      function fn() {
        alert("删除事件");
      }
      //   上面两个事件都会依次触发
      //   removeEventListener 删除事件
      btns[1].removeEventListener("click", fn);

      //    attachEvent ie9 以前的版本支持
      //   btns[2].attachEvent("onclick", () => alert(10000));
      //   btns[2].detachEvent('onclick',fn1);
    </script>
  </body>
</html>
```

事件流：描述的是从页面中接收事件的顺序。
事件发生时在元素节点之间按照特定的顺序传播，这个传播过程就是 DOM 整个流
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665661086696-dd9cb38e-3c83-475a-84bd-d52872c3f3a7.png#averageHue=%23f3f3f3&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=566&id=u1a74c5ce&margin=%5Bobject%20Object%5D&name=image.png&originHeight=708&originWidth=887&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108343&status=done&style=none&taskId=ue0963034-5fb4-403f-bade-a85448f3611&title=&width=709.6)
DOM 事件流会经过 3 个阶段：

- 捕获阶段
- 当前目标阶段
- 冒泡阶段

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665661159154-3a2dcec7-3231-49c6-ae7a-5694555d8a98.png#averageHue=%23faf8f6&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=444&id=u6e35b101&margin=%5Bobject%20Object%5D&name=image.png&originHeight=555&originWidth=879&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101260&status=done&style=none&taskId=u57f32d21-5664-4f41-bf40-08101f7df6c&title=&width=703.2)
注意：

- JS代码中只能执行捕获或者冒泡其中的一个阶段。
- onclick 和 attachEvent 只能得到冒泡阶段
- addEventListener 第三个参数是 true ，表示在事件捕获阶段调用事件处理程序；如果 false （默认就是 false） 表示事件在冒泡阶段调用事件处理程序 

有些事件是没有冒泡的 eg: onblur  onfocus  onmouseenter  onmouseleave
#### 事件冒泡
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>事件冒泡</title>
  </head>
  <body>
    <div class="father">
      <div class="son">son 盒子</div>
    </div>

    <script>
      let son = document.querySelector(".son");
      son.addEventListener("click", () => alert("son"), false);
      let father = document.querySelector(".father");
      father.addEventListener("click", () => alert("father"), false);
      // 给 document 注册事件
      document.addEventListener("click", () => alert("document"));
    </script>
  </body>
</html>
```
#### 事件捕获
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>事件捕获</title>
  </head>
  <body>
    <div class="father">
      <div class="son">son盒子</div>
    </div>

    <script>
      // 第三个参数为 true
      document
        .querySelector(".son")
        .addEventListener("click", () => alert("son"), true);

      document
        .querySelector(".father")
        .addEventListener("click", () => alert("father"), true);

      document.addEventListener("click", () => alert("document"), true);
    </script>
  </body>
</html>

```
#### 事件对象
> 事件发生后 ，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象；

eg:

- 谁绑定了这个事件
- 鼠标触发事件的话，会得到鼠标的相关信息
- 键盘触发事件的话，会得到键盘的相关信息
#### 事件对象的使用
事件触发发和时就会产生事件对象，系统会以实参的形式传递给事件处理函数。
可以在事件处理函数中声明 1 个形参用来接收事件对象：
```javascript
son.onclick = function(event){
  // 这个 event 就是事件对象
}

son.addEventListener('click',function(event){

});

son.addEventListener('click',fn)
function fn(event){

}
// 在 ie6-8 中，浏览器不会给方法传递参数，如果需要的话需要到 window.event 中获取查找
// 事件对象的兼容性处理
div.addEventListener('click',function(e){
  // 事件对象
  e= e || window.event;
  console.log(e);
});
```
#### 事件对象的属性和方法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665663924739-5b43aac4-4886-4f61-b678-68c2504199f6.png#averageHue=%23f7f6f5&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=257&id=ud11d01cd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=321&originWidth=836&originalType=binary&ratio=1&rotation=0&showTitle=false&size=119760&status=done&style=none&taskId=ud2efabc5-86c2-4cf3-a723-b97ccba8a62&title=&width=668.8)
#### e.target 和 this 的区别

- this 是事件绑定的元素（绑定这个事件处理函数的元素）
- e.target 是事件触发的元素
> 通常情况下 target 和 this 是一致的。有一种特殊情况：在事件冒泡时（父子元素有相同事件，单击子元素，父元素的事件处理函数也会被触发执行），这时候 this 是指向的是父元素，它是绑定事件的元素对象；而 target 指向的是子元素，它是触发事件的那个具体元素对象

```javascript
<div>123</div>
  <script>
  let div = document.querySelector("div");
div.addEventListener("click", (e) => {
  console.log(e.target); //div
  console.log(this); //windows
});
```
事件冒泡下的 e.target 和 this
```javascript
  <ul>
      <li>abc</li>
      <li>abc</li>
      <li>abc</li>
    </ul>

    <script>
      let ul = document.querySelector("ul");
      ul.addEventListener("click", (e) => {
        console.log(this); //windows
        console.log(e.target); // li
      });
```
#### 阻止默认行为
> html 中的一些标签有默认行为，eg a 标签被单击后，默认会进行页面跳转

```html
<a href="http://www.baidu.com">baidu</a>
<script>
  let a = document.querySelector("a");

  a.addEventListener("click", (e) => {
    // 加上后就不会跳转了
    e.preventDefault(); // dom 标准写法
  });

  //   传统的注册方式
  a.onclick = (e) => {
    e.preventDefault();

    e.returnValue = false;
    // 没有兼容性问题
    return false;
  };
</script>
```

#### 阻止事件冒泡
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665664865771-ef7e57c0-60e4-470f-bd0a-2231d394a8f6.png#averageHue=%23d7d9cb&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=194&id=u1e3233a1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=242&originWidth=839&originalType=binary&ratio=1&rotation=0&showTitle=false&size=40827&status=done&style=none&taskId=u8bea3788-ea35-449e-b63a-05fa632dc72&title=&width=671.2)
```javascript
<div class="father">
  <div class="son">son</div>
  </div>

  <script>
  let son = document.querySelector(".son");
son.addEventListener(
  "click",
  (e) => {
    alert("son");
    e.stopPropagation(); //停止传播
    window.event.cancelBubble = true;
  },
  false
);

let father = document.querySelector(".father");

father.addEventListener(
  "click",
  () => {
    alert("father");
  },
  false
);

document.addEventListener("click", () => alert("document"));
</script>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665665123111-6fa55b4e-74c0-4ac3-a53d-ee82295e982b.png#averageHue=%238ab3ce&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=210&id=ue2542ea1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=262&originWidth=848&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25034&status=done&style=none&taskId=ue758f8e8-fd46-443e-8f17-12231d09bbd&title=&width=678.4)
#### 事件委托（事件代理）
> 不给子元素注册事件，给父元素注册事件，把处理代码在父元素的事件中执行。

原理： 给父元素注册事件，利用事件冒泡，当子元素的事件触发，会冒泡到父元素，然后去控制相应的子元素
#### 事件委托的作用

- 只操作了一次 DOM 提高了程序的性能
- 动态新创建的子元素，也拥有事件
```html
<ul>
  <li>我应有弹框在手！</li>
  <li>我应有弹框在手！</li>
  <li>我应有弹框在手！</li>
  <li>我应有弹框在手！</li>
  <li>我应有弹框在手！</li>
</ul>

<script>
  let ul = document.querySelector("ul");
  ul.addEventListener("click", (e) => {
    e.target.style.backgroundColor = "tomato";
  });
</script>
```
### 常见鼠标事件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665665820224-98a956f7-2dea-4a3b-9494-99733c36ade9.png#averageHue=%23f8f8f8&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=288&id=u03d8c1ee&margin=%5Bobject%20Object%5D&name=image.png&originHeight=360&originWidth=834&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71717&status=done&style=none&taskId=uc7094cf8-264e-4fb6-9a81-2d925c08be9&title=&width=667.2)
##### 禁止选中文字和禁止右键菜单
```html
<body>
  我是不可选中

  <script>
    //  contextmenu 可以禁用右键菜单
    document.addEventListener("contextmenu", (e) => {
      e.preventDefault();
    });

    // 禁止选中文字 selectstart
    document.addEventListener("selectstart", (e) => {
      e.preventDefault();
    });
  </script>
```

#### 鼠标事件对象
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665666027657-fe2352be-16a7-4457-a91a-7bffe3253064.png#averageHue=%23f7f6f5&clientId=u82e3a30b-ab12-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=290&id=ud860a1fb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=363&originWidth=858&originalType=binary&ratio=1&rotation=0&showTitle=false&size=122746&status=done&style=none&taskId=u46ed0a72-7aaf-443f-94b9-7bc2056344d&title=&width=686.4)
##### 获取鼠标在页面的坐标
```html
   <script>
      document.addEventListener("click", (e) => {
        // client 鼠标在可视区的 x 和 y 坐标
        console.log(e.clientX);
        console.log(e.clientY);
        console.log("==========================");

        // page 鼠标在页面文档的 x 和 y 坐标
        console.log(e.clientX);
        console.log(e.clientY);
        console.log("==========================");

        // screen 鼠标在电脑屏幕的 x 和 y 坐标
        console.log(e.clientX);
        console.log(e.clientY);
      });
    </script>
```
#### 图标跟随鼠标移动
核心原理： 每次鼠标移动，都会获得最新的鼠标坐标，把这个 x 和 y 坐标做为图片的 top 和 left 值就可以移动图片
```html
<img
  src="images/1.png"
  style="width: 200px; height: 200px; position: absolute"
  alt=""
  />
<!-- 图片要移动， 并且不占位置，所以要用绝对定位  -->
<script>
  let img = document.querySelector("img");
  // mousemove 鼠标移动事件
  document.addEventListener("mousemove", (e) => {
    let x = e.pageX;
    let y = e.pageY;
    console.log("x=" + x, "y=" + y);
    img.style.left = x - 50 + "px";
    img.style.top = y - 40 + "px";
  });
</script>
```

##### 常见键盘事件
```html
<body>
  <script>
    document.addEventListener("keyup", () => alert("我弹起了"));

    document.addEventListener("keypress", () => alert("我按下了 press"));

    document.addEventListener("keydown", () => alert("我按下了 down"));
  </script>
```
上面三个事件的执行顺序是 ： keydown --> keypress -->  keyup

##### 事件键盘对象
| keyCode | 返回该键的 ASCLL 值 |
| --- | --- |

> 注意：
> - onkeydown 和 onkeyup 不区分字母大小写，  onkeypress 区分字母大小写
> - keypress 不识别功能键， 但是 keyCode 属性能够区分大小写，返回不同的 ASCLL 值	


##### 利用 keyCode 属性判断用户按下哪个键
```html
<script>
  document.addEventListener("keyup", (e) => {
    console.log("up" + e.keyCode);
    // 利用 ASCLL 值判断按下了哪个键
    if (e.keyCode === 65) {
      alert("a 键");
    } else {
      alert("没有按下 a 键");
    }
  });
  document.addEventListener("keypress", (e) =>
    console.log("press" + e.keyCode)
                           );
</script>
```
#### 案例：模拟京东按键输入内容
按下 s 键，光标就定位到搜索框
> 触发焦点事件，使用元素对象.focus()


```javascript
<input type="text" />
  <script>
  let input = document.querySelector("input");

document.addEventListener("keyup", (e) => {
  if (e.keyCode === 83) {
    // 触发输入框的获得焦点事件
    input.focus();
  }
});
</script>
```
案例： 在文本框中输入内容时，文本框上面自动显示大字号的内容
```html
<div>
  <div class="con">123</div>
  <input type="text" placeholder="输入数字" />
</div>

<script>
  let con = document.querySelector(".con");
  let inp = document.querySelector("input");
  inp.addEventListener("keyup", (e) => {
    // 这里面的 this 是指  window
    if (e.target.value == "") {
      con.style.display = "none";
    } else {
      console.log(e.target.value);

      con.style.display = "block";
      con.innerText = e.target.value;
    }
  });
  // 给输入框注册失去焦点事件，隐藏放大提示盒子
  inp.addEventListener("blur", () => {
    con.style.display = "none";
  });
  // 给输入框注册获得焦点事件
  inp.addEventListener("focus", () => {
    if (this.value !== " ") con.style.display = "block";
  });
</script>
```
### BOM
> BOM Browser Object  Model  浏览器对象模型， 它的核心对象是 windos
> BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C，BOM 最初是Netscape 浏览器标准的一部分。

> DOM 是把 html 文档当作一个对象来看待
> BOM 是把 浏览器当作一个对象来看待

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665750809294-b851453c-5fdf-4bdc-b116-e7d7c203b9f9.png#averageHue=%23fbfbfb&clientId=u4f4bcf1b-7615-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=237&id=u0aafe8cb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=296&originWidth=855&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38942&status=done&style=none&taskId=u8c8a8a91-c581-420b-aef2-279c66fae03&title=&width=684)
window 对象是浏览器的顶级对象：

- 提供一个 js 访问浏览器窗口的一个接口
- 它是一个全局对象。定义在全局作用域中的变量，函数都会变成 window 对象的属性和方法。

在调用的时候可以省略 window eg: alert()  prompt() ....
window 有一个特殊属性  window.name
#### window 对象的常见事件

- `window.onload`

它是窗口加载事件，当文档内容完全加载完成会触发这个事件，并调用处理函数。它要等页面全部加载完毕，才会去执行处理函数 （包括图像，脚本文件，CSS文件等）
它传统注册方式只能写一次，如果有多个会以最后一个为准。
使用 `addEventListener` 则没有限制 

- `DOMContentLoaded`

当 DOM 加载完成才触发不包括样式表，图片，flash 等等。（适用于加载图片或其他资源太多，园加载太慢，用这个比较合适）
```html
<button>button</button>

<script>
  window.addEventListener("load", () => {
    let btn = document.querySelector("button");
    btn.addEventListener("click", () => alert("listener btn"));
  });

  window.addEventListener("load", () => alert(22));

  // 页面加载会先触发这个事件
  window.addEventListener("DOMContentLoaded", () => alert(33));
</script>
```
#### 调整窗口大小事件

- `window.onresize`
- 使用 addEventListener 添加事件时，不加 on 

```html
<div>123</div>
<script>
  window.addEventListener("load", () => {
    let div = document.querySelector("div");

    window.addEventListener("resize", () => {
      console.log("发生了变化 ");

      // window.innerWidth 获取窗口宽度
      // window.innerHeight 获取窗口高度
      if (window.innerWidth <= 800) {
        div.style.display = "none";
      } else {
        div.style.display = "block";
      }
    });
  });
</script>
```
#### 定时器

- setTimeout() ：相当于延时函数，延时完了就执行
- setInterval()：定时，每当到某个时间就去调用那个函数
```html
<!-- 5s 后关闭显示 -->

<!-- <img src="./images/1.png" alt="" /> -->

<script>
  // <!-- 定时器 -->
  // <!-- setTimeout() 和  setInterval() -->
  // setTimeout(() => alert(44), 5000); //毫秒为单位，它的函数参数是一个回调函数
  // 回调函数：函数作为参数传递到其他函数（函数指针）
  //维基百科： 在计算机程序设计中，回调函数，或简称回调（Callback 即call then back 被主函数调用运算后会返回主函数）
  // ，是指通过参数将函式传递到其它代码的，某一块可执行代码的引用。
  // 这一设计允许了底层代码调用在高层定义的子程序。

  // let img = document.querySelector("img");
  // let timer = setTimeout(() => (img.style.display = "none"), 5000);

  // 停止定时器
  // window.clearTimeout(timer);

  window.setInterval(() => console.log("闹钟"), 1000); //每隔 1s 就会输出一次
	window.clearInterval(clock); //停止定时器
</script>
```

#### this 指向问题
> this 的指向在函数定义的时候无法确定，只有在函数执行的时候才能确定 this 到底是指向谁，一般情况下是指向调用它的对象。

- 全局作用域或者普通函数中 this 指向全局对象 window ；定时器里面的 this 也是指向 windows
- 方法调用中的this 指向调用者
- 构造函数中 this 指向构造函数的实例

### location 对象
> window 对象给我们提供一个 location 属性用于获取或设置窗体的 URL，并且可以用于解析 URL。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665829673542-03176131-61b0-4aee-8d11-b28734cef58d.png#averageHue=%23cec4b8&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=377&id=ud989b97a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=471&originWidth=905&originalType=binary&ratio=1&rotation=0&showTitle=false&size=133581&status=done&style=none&taskId=uc3fcf826-e80a-4842-8f5d-9e3dd8001eb&title=&width=724)
##### location 对象的属性
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665829692868-4d15248c-07c9-4bd0-9ae8-0ba8800f4bc0.png#averageHue=%23f7f7f6&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=229&id=ue4d57814&margin=%5Bobject%20Object%5D&name=image.png&originHeight=286&originWidth=852&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79647&status=done&style=none&taskId=u33c05741-3368-43e5-afa4-8477fdb9db6&title=&width=681.6)
#### 跳转页面
```html
<body>
  <button>点击</button>
  <div></div>
  <script>
    let btn = document.querySelector("button");
    let div = document.querySelector("div");
    btn.addEventListener("click", function () {
      // 跳转到百度
      location.href = "http://www.baidu.com";
      console.log(location.href);
    });
    let timer = 5;
// 5 秒后跳转
    setInterval(() => {
      if (timer == 0) location.href = "http://www.itcast.cn";
      else div.innerHTML = "你将在" + timer + "秒之后跳转到首页";
      timer--;
    }, 1000);
  </script>
```
##### 获取URL 参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665830738189-c7fb17a0-98c2-4ea1-8ab0-ec0b5397c25b.png#averageHue=%23f7f2f0&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=231&id=u8fb72c08&margin=%5Bobject%20Object%5D&name=image.png&originHeight=289&originWidth=848&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107244&status=done&style=none&taskId=u94b9e306-4563-4eda-88c9-51781c59c5f&title=&width=678.4)
```html
<div></div>
<script>
  console.log(location.search); // ?uname=andy
  // 1.先去掉？  substr('起始的位置'，截取几个字符);
  var params = location.search.substr(1); // uname=andy
  console.log(params);
  // 2. 利用=把字符串分割为数组 split('=');
  var arr = params.split('=');
  console.log(arr); // ["uname", "ANDY"]
  var div = document.querySelector('div');
  // 3.把数据写入div中
  div.innerHTML = arr[1] + '欢迎您';
</script>
```
#### location 对象的常见方法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665830774487-fdc6efd3-39b5-4c93-8288-b8f9011bfe73.png#averageHue=%23f7f6f5&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=142&id=u0e7887f8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=177&originWidth=909&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76148&status=done&style=none&taskId=u572ebfc9-bc6c-48d1-a484-fda4db571c3&title=&width=727.2)
```html
<button>点击</button>

<script>
  document.querySelector("button").addEventListener("click", () => {
    // 记录浏览历史，可以实现后退功能
    // location.assign("http://www.baidu.com");

    // 不记录浏览历史，不能实现后退
    // location.replace("http://www.baidu.com");
    //刷新
    location.reload(true);
  });
</script>
```
#### navigator 对象
> 它包含了有关浏览器的信息，它有很多属性，最常用的是 userAgent ， 这个属性可以返回由客户机发送服务器的 user-agent 头部的值

实现判断用户使用哪个终端打开页面并跳转：
```html
 <script>
      if (
        navigator.userAgent.match(
          /(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i
        )
      ) {
        window.location.href = ""; //手机
      } else {
        window.location.href = ""; //电脑
      }
    </script>
```
#### history 对象
> history 对象与浏览器历史记录进行交互， 该对象包含用户（在浏览器窗口中）访问过的URL

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665831269724-b8b8517a-a065-475b-955f-eb19a2e04e90.png#averageHue=%23f8f8f7&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=136&id=u20063b98&margin=%5Bobject%20Object%5D&name=image.png&originHeight=170&originWidth=898&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45287&status=done&style=none&taskId=u6cddaf4a-e1c7-46ad-a44d-a85dff3a192&title=&width=718.4)
### JS 执行机制
```javascript
<script>
  // console.log(1);
  // setTimeout(() => console.log(3), 1000);
  // console.log(2);
  //  上面在行单独执行 1 2 3
  console.log("===========================");
console.log(1);
setTimeout(() => console.log(3), 0);
console.log(2);
//  上面在行单独执行 1 2 3
</script>
```

> JS 的一个特点就是单线程，同一个时间中能做一件事。

HTML5 允许 JS 创建多个线程，但是子线程完全受主线程的控制。于是 JS 中出现了 同步任务和异步任务。
> JS中所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。 同步任务指的是： 	在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务； 异步任务指的是： 	不进入主线程、而进入”任务队列”的任务，当主线程中的任务运行完了，才会从”任务队列”取出异步任务放入主线程执行。


执行机制：

- 先执行执行栈中的同步任务
- 异步任务放入任务队列中
- 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665831755484-ab5fae06-857a-4d27-b3cd-10bbc5ead3e9.png#averageHue=%23c2b5a3&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=260&id=udb6188fd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=325&originWidth=919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49677&status=done&style=none&taskId=uc1c195cf-9fe4-4cc9-8e16-b66a287af7b&title=&width=735.2)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665831779859-4cfccb6a-133f-4e60-bdb2-06315b35ae2b.png#averageHue=%23f9f9f9&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=407&id=ufa6c1996&margin=%5Bobject%20Object%5D&name=image.png&originHeight=509&originWidth=950&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91574&status=done&style=none&taskId=u4677881b-642b-4bc6-97af-779d8a2bc06&title=&width=760)

#### offset 与 style 区别

- offset 可以得到任意样式表中的样式值
- offset 系列获得的数值是没有单位的
- offsetWidth 包含 padding + border + width
- offsetWidth 等属性是只读属性，只能获取不能赋值
- 要获取元素的大小这个理适合

---

- style 只能得到行内样式表中的样式值
- style.width 获得的是带有单位的字符串
- ..... 获取不包含 padding 和 border 的值
- ....... 是可读写属性，可以获取也可以赋值
- 想要给元素更改值用这个

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665832871965-5bd91c93-ef45-4299-90f2-0ffd2e7f2c56.png#averageHue=%23f6f5f4&clientId=uf0086e5e-05d1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=135&id=u46ae867d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=169&originWidth=839&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72571&status=done&style=none&taskId=u9f4a6a71-d890-4dbe-89b8-2c9a9df2fe3&title=&width=671.2)
mouseenter  和 mouseover 的区别：

- mouseover 鼠标经过自身盒子会触发，经过子盒子还会触发。mouseenter  只会经过自身盒子触发

##### 封装简单动画函数
```html
<div
  style="
  width: 100px;
  height: 100px;
  background-color: tomato;
  position: absolute;
  "
  ></div>

<body>
  <script>
    animate(document.querySelector("div"), 1000);
    // 封装一个简单的动画函数
    function animate(obj, target) {
      clearInterval(obj.timer);
      obj.timer = setInterval(() => {
        if (obj.offsetLeft >= target) {
          // 停止动画本质就是停止定时器
          clearInterval(obj.timer);
        }
        obj.style.left = obj.offsetLeft + 1 + "px";
      }, 30);
    }
  </script>
```
> 核心原理：JS 是一门动态语言，可以方便的给当前对象添加属性


#### 判断用户页面浏览和离开页面时显示不同的网页标题
```javascript
 // 需求：用户离开和正在浏览当前页面，更改当前页面的主题
      document.addEventListener("visibilitychange", function () {
        // 用户息屏，或者切到后台运行（离开页面）
        var title = document.querySelector("title");
        if (document.visibilityState === "hidden") {
          title.innerText = "用户离开了页面！";
        }

        // 用户打开或回到页面
        if (document.visibilityState === "visible") {
          title.innerText = "欢迎回来！";
        }
      });
```


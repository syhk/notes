# JS

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>javascipt 测试</title>
</head>
<body>
    <p id="demo">这是 javascipt</p>
    

    <h1>My First Web  Page</h1>
    <p>My  frist  paragraph.</p>

    <!-- 点击后，就删除页面上所有的 html 元素，只剩下它自己的结果  -->
 <button type="button" onclick="document.write(5 ** 2)">删除所有现有的 HTML</button>

<!-- 使用  window.print() 打印当前窗口的内容 -->
<!-- 点击后，会打印这个页面，就打印机的打印 -->
<button onclick="window.print()">Print this page</button>


<button onclick="document.getElementById('demo').innerHTML = Date()">现在的时间是 </button>



    <script type="text/javascript">
        // 改变它的内容
   document.getElementById("demo").innerHTML="My First Javascript";
   cout(document.getElementById("demo").innerHTML)
   cout(document.getElementById("demo").outerHTML)//输出  <p id="demo">这是 javascipt</p>
   cout(document.getElementById("demo").innerText)
   cout(document.getElementById("demo").outerText)

// 写入 html 元素使用  innerHTML
// 写入 html 输出 document.write()
// 写入 警报框  window.alert()
// 写入 浏览器控制台 console.log()
document.write(5+6)
document.write(5+6+'<br>')//可以使用这样嵌入 html 达到换行
document.write(5+6) //会显示在网页上，但是它会拼接着，不会换行
// windows 对象是全局作用域对象。
window.alert(45);
alert(45);

/*
javascript 变量的4种方法
*/
var a = 1;// 这个可以在作用域内重新声明
let a1 = 2; //局部变量,使用let 声明的变量在作用域不能重新声明
const a2 = 3;//这个不能重新声明，不能重新分配
a3 = 4;

// javascript 求幂
let z = 5 ** 2; // 25

// typeof 运算符 返回变量或表达式的类型
cout(typeof "") //string
cout(typeof 55) //number

//  js 函数
function myFunction(p1 , p2){
    return p1*p2;
}

let x = myFunction(4,5);
// 对象
let person = {
    name:"syhk",
    age:18,
    address:"云南省",
    fullName: function(){
        this.name+ "   "+this.age;
    }
}
cout(person.address)
/*
this: 在对象方法中指的是对象
单独 this 指的是全局对象
在函数中，this 指的是全局对象
在严格模式下的函数中， this 是 undfined
在一个事件中， this 指的是接收到这个事件的元素


HTML 常见事件：
onchange
onclick
onmouseover 鼠标悬停
onmouseout  鼠标从元素上移开
onkeydown   用户按下键盘键
onload   浏览器已经完成页面加载
*/

/*
==　和 === 的区别:
不同类型比较，== 会查看值是否相等，相等就是 true　, === 如果类型不同就是 false
高级类型 == 会将高级类型转化为其他类型，进行 值比较
=== 因为类型不同就会返回 false

*/
let x1 = new String("John");
let y1 = new String("John");
cout(x1==y1) //false
cout(x1===y1) //false

    
   function cout(obj){
    console.log(obj)
   }

   let str = "Apple,Banana,Kiwi";
   let part = str.slice(7,13);
  console.log(part)


// 模板字符串  使用反引号，变量的值会被替换
let firstName = "sy";
let lastName = "hk";
cout(`Welcome ${firstName}, ${lastName}`)


// 箭头函数
let myFunction1= (a,b) => a*b;
hello = () => {
    return "Hello World";
}
hello=()=> "Hello world";
// 如果只有一个参数可以省略括号：
hello = val => "hello" + val;
    </script>
</body>
</html>
```

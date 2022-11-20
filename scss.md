# scss

比较出名的预处理器有：

scss/sass   less stylus

scss 的扩展名 .scss

它们的代码并不能被直接解析，必须将它们编译成  css 代码

```latex
cnpm i -g node-sass //使用命令安装
```

安装成功：

安装 Ruby 

![image_ggTYmJDdKq.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663901088859-d6fbc764-9da2-4710-8c48-acac7c4bf449.png#clientId=ua5497d0c-6059-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u47c981d8&margin=%5Bobject%20Object%5D&name=image_ggTYmJDdKq.png&originHeight=192&originWidth=945&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26621&status=done&style=none&taskId=u4a7abc5a-5a1b-4660-952b-be92eab60fe&title=)

编译：node-sass  文件名.sass  文件名.css

```
// sass 工作方式：需要一个预处理器，将代码转换为标准 css
// 文件扩展名 .scss
// sass 基于 Ruby
// 定义
$syhk1: #f23;
$syhk2: #f28;
$syhk3: #f78;

// 使用
h1 {
  background-color: $syhk1;
}

h2 {
  background-color: $syhk2;
}

h3 {
  background-color: $syhk3;
}

h1:hover {
  background-color: $syhk3;
}

// Sass 变量
// Sass 使用 $ 符号，加变量名声明变量
// 语法： $variableaname : value ;
$myFont: Helvetica, sans-serif;
$myColor: red;
$ntFonSize: 18px;
$myWidth: 680px;

h1 {
  $myColor: green; //这个在 h1 里面定义，只会在这里可用
  color: $myColor; //这里使用的颜色将会是 green
}

p {
  color: $myColor; //这里是上面定义的 red
}

// ============= 上面代码编译后css输出
h1 {
  color: green;
}

p {
  color: red;
}

// !global
// 可以使用它改变变量的的覆盖范围
// !global 表示变量是全局的，它在整个文件范围内都可以访问

h3 {
  $myColor: tomato !global; //全局变量
  color: $myColor;
}

h4 {
  color: $myColor; //可以使用到上面的全局变量
}

// 推荐做法： 单独一个文件 globals.scss 文件中定义所有全局变量
// 并可以使用 @include 关键字包含文件

// Sass 嵌套规则和属性

// css 代码
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}

// 以上 sass 代码
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  li {
    display: inline-block;
  }
  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}

// Sass 允许编写嵌套属性
// 就是 css 中具有相同的前缀，eg: font-family font-size 。。。
// 可以编写嵌套属性

// Sass @import 和 Partials
// 将一个文件的内容包含在另外一个文件中
// css 的@import 每次调用时都会创建一个额外的 HTTP 请求
// 但是 Sass @import 指令将文件包含在 css中，不需要额外的http 调用
// 语法 @import filename; 不需要指定文件扩展名
@import url(./globals.scss);
// @import "globals"
body {
  color: $Myglobal_color; //使用到
}

// 如果文件名以下划线开头 ,sass 不会转译它，也称为部分文件
// 语法： _filename;
// 导入时不需要加下划线 @import "filename"
// sass 知道应该导入文件 _filename.scss

// Sass @mixin  和 @include
/*
@mixin 指令允许您创建要在整个网页中重用的 css 代码
@include 是为了使用 mixin 也就是包含
*/

// @mixin 定义
@mixin syhk {
  color: tomato;
  font-size: 20xp;
  font-weight: bold;
  border: 1px solid blue;
  // 一个 mixin 还可以包含其他 mixin
  @include syhk1;
}
// 注意： 连字符和下划线被认为是相同的。意味着： @mixin important-text{} 和 @mixin improtant_text {} 被认为是同一个 mixin!

// 使用 @include 指令包含一个 mixin
select {
  @include syhk1;
  @include syhk;
}

.div {
  background-color: tomato;
  @include syhk;
}

// Mixins 接受参数，可以将变量传递给 mixin
@mixin syhk3($color, $width) {
  border: $width solid $color;
}

// 使用带有参数的 mixin
.div1 {
  @include syhk3(blue, 20px);
}

// Mixin 允许默认值
@mixin syhk4($color: blue, $width: 1px) {
  border: $width solid $color;
}
// 在使用时可以只指定一个值
.div2 {
  @include syhk4($color: tomato) //只指定一个值
;
}

// Sass @extend 继承
// @extend 指令允许您将一组 css 属性从一个选择器共享到另一个选择器
.button-basic {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}
.button-report {
  @extend .button-basic;
  background-color: red;
}

.button-submit {
  @extend .button-basic;
  background-color: tomato;
  color: white;
}

// Sass 的字符串函数下标是从 1 开始而非 0 开始
// 它的函数和其他语言差不多会傅就行
```

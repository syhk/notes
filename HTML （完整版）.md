

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>learn HTML</title>
</head>
<body>
<!-- HTML 标题是从 H1 到 H6 -->
<h1>learn HTML</h1>
<!--段落标签是 <p>  -->
<p>syhk learn HTML</p>
<!-- <a> 是链接标签;链接的地址在 href 属性上 -->
<a href="https://www.baidu.com">This is a link</a>
<!-- HTML 图像是用 <img> 标签定义的 
源文件的路径： src   ；  替代文本 alt（也就是如果图像无法显示，就用这个文本替换图像）  剩下是宽和高
-->
<img src="../images/9mjoy1.jpg"  alt="https://wallhaven.cc/" width="104" height="142">

<!-- <br> 定义了一个换行符 -->

<!-- 标题属性： title 
也就是当鼠标悬停在元素上时， title 属性的值将显示出来，像提示一样
-->
<p title="I am a student">This is a pargraph.</p>

<!-- 总结 ：
所有HTML 元素都可以有属性
-->
<!-- 
    <hr> 标记定义 HTML 水平线
    <br> 是一个换行符
 -->
<hr>
<br>

<!-- 
    
<pre> 元素预格式的文本,它保留了两个空格，换行符
 -->

<pre>
    I name 
    is syhk , so cool;
</pre>
<!-- 
    color 颜色
    font-family 定义元素的字体
    font-size  定义文本的大小
    text-align 定义元素的水平文本对齐方式
 -->

 <!-- 
<b>  粗体字体
<strong>  重要文字
<i>  斜体文字
<em> 强调文字
<mark> 标记文本
<small> 较小的文字
<del> 删除的文字
<ins>  插入文本
<sub> 下标文本
<sup> 上标文字
  -->
<b>syhk</b>
<strong>syhk</strong>
<i>syhk</i>
<em>syhk</em>
<mark>syhk</mark>
<small>syhk</small>
<del>syhk</del>
<ins>syhk</ins>
<sub>syhk</sub>
<sup>syhk</sup>

<!-- HTML 引用和引用元素 -->
<!-- 
    <blockquote> 通常是缩进它
    <q> 在会在周围插入引号
    <abbr>
 -->
<p>My name is syhk: <blockquote cite="http://www.baidu.com"></blockquote></p>
<p>My name is syhk:<q>shen yang</q></p>
<p>The <abbr title="World Health Organization">WHO</abbr>was founded in 1948.</p>
<address>my name is syhk</address>
<p>My name is syhk:<cite>The Scream</cite></p>
<bdo dir="rtl">My name is syhk</bdo>


<!-- HTML 颜色 
RGB,HEX,HSL,RGBA,HSLA 值都可以指定 还有十六进制颜色编号 
-->
<!-- 设置边框颜色  -->
<h1 style="border:2px solid Tomato; background-color: rgba(255,99,71,0.5);" >My name is syhk</h1>
<!-- RGBA 和 hsla 两个颜色值可以设置透明度 
RGBA (255,255,255,0,5) 最后一个是设置透明度  值为范围为 0.0-1.0
hsla (255,100%,64%,0.5) 0-360 0%-100%  0%-100% 0.0-1.0
HSL 代表色相，饱和度和亮度 而 hsla 也是增加透明度的设置
-->
<h1 style="border:3px solid Tomato; background-color:  hsla(255,100%,100%,0.8)">My name is syhk</h1>

<!-- css 可以通过 3 种方法使用：
元素内部嵌入
<head> 头部总合
link 链接外部文本
-->


</body>
</html>
```

[test1.html](file/test1_1tERyz3-b2.html)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML learn two</title>
    <!-- 
    HTML 设置网站图标
 -->
    <link rel="icon" type="image/x-icon" href="../images/syh.ico">



    <style>
        /*
        padding  定义文字和边框之间的距离，也就是边界内部
        margin 属性定义边框外的边距

        
        */
        p {
            border: 2px solid powderblue;
            padding: 30px;
            margin: 50px;
        }

        /* 链接颜色更改 */
        /* a:link {

            color: green;
            background-color: transparent;
            text-decoration: none;
        }

        a:visited {
            stop-color: pink;
            background-color: transparent;
            text-decoration: none;
        }

        a:hover {
            color: red;
            background-color: transparent;
            text-decoration:underline;
        }
        a:active {
            color: yellow;
            background-color: transparent;
            text-decoration: underline;
        } */

        /* 链接按钮 */
        /* a:link , a:visited {
            background-color: #f44336;
            color: white;
            padding: 15px 25px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
        }
        a:hover, a:active {
            background-color:red;
        } */


        /* 为页面添加背景图片 */

        body {
            background-image: url("../images/syhk.gif");
            background-repeat: no-repeat;
            /* 上面这个属性是避免背景图像重复 */
            background-size: 100%;
            /* 这个属性设置 背景图片覆盖整个元素 ； 也可以设置为 100% 拉伸为适合整个元素 */
            background-attachment: fixed;
            /* 这个让背景图像没有拉伸 */
        }
    </style>

    <!-- 

样式暂时注释

        /* 表格相关 */
        table,
        th,
        td {
            border: 1px solid orangered;
            border-collapse: collapse;
            /* 使边框折叠成为单个边框 */
            border-radius: 10px;
            /* 为表格设置 圆角 */
            border-style: outset;
            /* 设置虚线表格
            可选值 ： dotted dashed solid double groove ridge inset outser none hidden
            */
            border-color: blue;
            /* 设置边框的颜色  */
            border-spacing: 30px;
            /* 设置单元格间距 */
        }

        /* 为每个单元格设置颜色  */
        td,
        th {
            background-color: #96d4d4;
            border-radius: 10px;
            padding: 15px;
            /* 在单元格上添加填充 */
            padding-top: 15px;
            /* 仅在上方添加填充 */
            padding-left: 30px;
            padding-right: 40px;
            padding-bottom: 20px;
        }

        /*      表格样式 */

        tr:nth-child(even) {
            /* 使用 odd 替换 even 就是奇数行 */
            background-color: chartreuse;
        }

        /* 设置表格以列为单位设置 */
        td:nth-child(even),
        th:nth-child(even) {
            background-color: crimson;
        }

        /* 鼠标悬停时突出显示表格行 */
        tr:hover {
            background-color: darkorange;
        }



 -->






    <!-- 链接到样式表 -->
    <link rel="stylesheet" href="https://www.w3schools.com/html/styles.css">

</head>

<body>
    <p> My name is syhk</p>

    <!-- 超链接 
默认情况下： 未访问的链接带有下划线和蓝色，访问过的带有下划线和紫色  ，活动链接有下划线和红色
target 属性指定打开链接文档的位置
它的值有： _self  默认 在单击时在同一窗口/选项卡中打开文档
_blank 在新窗口或选项卡中打开文档
_parent 在父框架中打开文档
_top 在整个窗口中打开文档
它们的区别就是在哪里打开链接的目的地
-->
    <a href="https://www.w3schools.com/" target="_self">My name is syhk</a>


    <!-- 使用图片作为超链接 在 a 标签里面套上  img 标签 -->
    <a href="http://www.baidu.com">
        <img src="../images/9mjoy1.jpg" alt="HTML tutorial " style="width:42px; height:42px;">
    </a>


    <!-- 链接到电子邮件地址  唤醒邮件服务-->
    <a href="mailto:725482520@qq.com">Send email</a>

    <!-- 按钮做为超连接 -->
    <button onclick="document.location='http://www.baidu.com'">HTML Tutorial</button>
    <br>

    <!-- 链接标题 鼠标在链接上面会显示 title 的内容 -->
    <a href="https://www.baidu.com" title="跳转到百度">Syhk</a>


    <!-- HTML 链接的不同颜色 在上面 css 中  -->


    <!-- 在HTML 中创建书签
使用 id 属性来创建一个书签，然后在 a 标签中可以引用并跳转注意引用时要带上 # 号
-->
    <h2 id="c4">Chapter 4</h2>
    <a href="#c4">Jump to Chapter 4</a>

    <!-- HTML 图片  也可以添加动画 GIF APNG ICO JPEG PNG SVG -->
    <img src="../images/9mjoy1.jpg" alt="这是图片的描述，它应该描述图像">
    <!-- 引用在线网址图像 -->
    <img src="https://www.w3schools.com/images/w3schools_green.jpg" alt="W3Schools.com">
    <!-- GIF -->
    <img src="../images/syhk.gif" alt="gif and HTML">
    <!-- 将图像作为链接 GIF 图像也可以 -->
    <a href="https://www.baidu.com">
        <img src="../images/syhk.gif" alt="HTML 图像">
    </a>

    <!-- 图像浮动
使用 css float 属性可以让图像浮动到文本的左侧或左侧
其实目前理解就是，放在文本左侧或右侧 
-->
    <P><img src="../images/9mjoy1.jpg" alt="Smiley face" style="float:right;width:42px;height:42px;">
        The image will float to the right of the text.
    </P>
    <p>
        <img src="../images/9mjoy1.jpg" alt="Smiley face" style="float:left;width: 42px;height: 42px;">
        The image will float to the left of the text.
    </p>

    <!-- HTML  map 标签 HTML 图像映射 
也就图像的各个区域，点击后发生不同的动作
图像和 map 映射起来
-->
    <img src="../images/9mjoy1.jpg" alt="Workplace" usemap="#syhk">
    <map name="syhk">
        <area shape="rect" coords="34,44,270,350" alt="Computer" href="http://www.baidu.com">
        <area shape="rect" coords="290,172,333,250" alt="Phone" href="https://taobao.com">
        <area shape="circle" coords="337,300,44" alt="Coffee" href="https://github.com">
    </map>
    <!-- 
    shape 形状：
    rect 定义一个矩形区域
    circle 定义一个圆形区域
    poly 定义一个多边形区域
    default 定义整个区域
 -->

    <!-- HTML 元素上背景图像 
使用 background-image:url(‘路径’);
-->

    <!-- HTML picture 允许为不同的用户显示不同的图片尺寸
source 的 media 属性，用于定义图像什么时候最合适
-->
    <picture>
        <source media="(min-width: 650px)" srcset="../images/9mjoy1.jpg">
        <source media="(min-width: 465px)" srcset="../images/9mjoy1.jpg">
        <img src="../images/9mjoy1.jpg">
    </picture>
    <!-- 也可以设置兼容问题，如果不支持显示  gif ，可以显示  jpg 或其他格式source 的 media 属性，用于定义图像什么时候最合适 -->
    <picture>
        <source srcset="../images/9mjoy1.jpg">
        <source srcset="../images/syhk.gif">
        <img src="../images/syhk.gif" alt="Beatles" style="width:auto;">
    </picture>

    <picture>
        <source srcset="img_avatar.png">
        <source srcset="img_girl.jpg">
        <img src="img_beatles.gif" alt="Beatles" style="width:auto;">
    </picture>


    <!-- HTML 表格 
    td 代表表格数据 里面也可以装其他东西，它只是一个容器
    th 让第一行成为表格标题

    -->

    <table>
        <tr>
            <th>Company</th>
            <th>Contact</th>
            <th>Country</th>
        </tr>
        <tr>
            <td>Alfreds Futterkiste</td>
            <td>Maria Anders</td>
            <td>Germany</td>

        </tr>
        <tr>
            <td>Centro comercial Moctzuma</td>
            <td>Francisco Chang</td>
            <td>Mexico</td>
        </tr>

    </table>

    <!-- HTML 表头
垂直表头
-->
    <table>
        <tr>
            <th>Firstname</th>
            <td>Jill</td>
            <td>Eve</td>
        </tr>
        <tr>
            <th>Lastname</th>
            <td>Smith</td>
            <td>Jackson</td>
        </tr>
        <tr>
            <th>Age</th>
            <td>94</td>
            <td>50</td>
        </tr>
    </table>
    <!-- 多行标题 
使用 colspan 属性横向合并单元格 ，值是合并几个单元格
添加表格标题： caption 标签 它紧跟在 <table> 标签之后 
-->
    <table>
        <caption>My name is syhk</caption>
        <tr>
            <th colspan="2">Name</th>
            <th>Age</th>
        </tr>
        <tr>
            <td>Jill</td>
            <td>Smith</td>
            <td>50</td>
        </tr>
        <tr>
            <td>Eve</td>
            <td>Jackson</td>
            <td>94</td>
        </tr>
    </table>

    <!-- 单元格属性： Colspan 和 Rowspan
colspan 属性要跨越多少列
rowspan 属性要跨越多少行
-->

    <!-- HTML 表格组合 
<colgroup>元素用于设置表格特定列的样式 
这个值的意思就是 前面几个单元格的样式，依次下去
注意：这个标签必须是 <table> 的子元素 ， 并且应该设置在任何其他表格元素之前，
    如 <thead> <tr> <td>  但是如果 <caption> 如果存在，就要放在它的后面
-->
    <table>
        <colgroup>
            <col span="2" style="background-color: #D6EE;">
        </colgroup>
        <tr>
            <th>MON</th>
            <TH>WED</TH>
            <th>TUE</th>
        </tr>

    </table>

    <table>

        <colgroup>
            <col span="2" style="background-color: #0ee8e8">
            <col span="2" style="background-color: pink">
        </colgroup>
        <tr>
            <th>MON</th>
            <th>TUE</th>
            <th>WED</th>
            <th>THU</th>
        </tr>


    </table>
    <!-- 如果前面几个不想有样式，可以不写样式就没有了，然后继续写后面的 -->
    <table>
        <colgroup>
            <col span="3">
            <col span="1" style="background-color: pink">
        </colgroup>
        <tr>
            <th>MON</th>
            <th>TUE</th>
            <th>WED</th>
            <th>THU</th>
        </tr>
    </table>

    <!-- 隐藏列 
    visibility: collapse 可以设置隐藏哪些列
    -->
    <table>
        <colgroup>
            <col span="2">
            <col span="3" style="visibility: collapse">
        </colgroup>
        <tr>
            <th>MON</th>
            <th>TUE</th>
            <th>WED</th>
            <th>THU</th>
        </tr>
    </table>

    <!-- HTML 列表 
ol 有序
ul 无序

描述列表： dl 标签定义描述列表 dt 定义术语 dd 描述每个术语
-->
    <ul>
        <li>Coffee</li>
        <li>Tea</li>
        <li>Milk</li>
    </ul>
    <ol>
        <li>Coffee</li>
        <li>Tea</li>
        <li>Milk</li>
    </ol>
    <dl>
        <dt>Coffee</dt>
        <dd>- black hot drink</dd>
        <dt>Milk</dt>
        <dd>- white cold drink</dd>
    </dl>

    <!-- 无序列表选项 
disc 默认值 圆黑点
circle 圆形
square 矩形
none 没有
列表也支持嵌套
-->
    <ul style="list-style-type:none">
        <li>syhk
            <!-- 它父亲的样式不会影响到它 -->
            <ul>
                <li>syhk1</li>
                <li>syhk2</li>
            </ul>

        </li>
        <li>shen yang</li>
        <li>Milk</li>
    </ul>

    <!-- 接 test3.html -->
    <a href="./test3.html">点击前往下一章节</a>
</body>

</html>
```

[test2.html](file/test2_Tl0g-NLY4i.html)

```html
<!DOCTYPE html>
<html>

<head>
    <title>第三章节</title>
    <link rel="icon" type="image/x-icon" href="../images/syh.ico">
    <style>
        .city {
            background-color: tomato;
            color: white;
            border: 2px solid black;
            margin: 20px;
            padding: 20px;
        }
    </style>

    <!-- 

暂时注释样式，用于其他样式测试


    ul {
        list-style-type: none;
        margin: 0;
        padding: 0;
        overflow: hidden;
        background-color: #333333;
    }

    li {
        float: left;
    }

    li a {
        display: block;
        color: white;
        text-align: center;
        padding: 16px;
        text-decoration: none;
    }

    li a:hover {
        background-color: #111111;
    }

    
 -->

</head>

<body>

    <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#news">News</a></li>
        <li><a href="#contact">Contact</a></li>
        <li><a href="#about">About</a></li>
    </ul>

    <!-- 有序列表
    type 指定序号的类型： 1 A a  I    i 
    start 属性用于指定开始的序号
    列表都可以嵌套
    -->
    <ol type="1" start="100">
        <li>Coffee</li>
        <li>Tea</li>
        <li>Milk</li>
    </ol>

    <!-- HTML 描述列表 -->
    <dl>
        <dt>Coffee</dt>
        <dd>black hot drink</dd>
        <dt>Milk</dt>
        <dd>white cold drink</dd>
    </dl>

    <!--HTML 块和内联元素 
常见的块级元素有： p  div
所有块级元素： address article aside blockquote canvas
dd div  dl dt  fieldset  figcaption figure footer  form 
h1-h6   header  hr li main  nav   noscript  ol p   pre 
section   table tfoot  ul video 
内联元素有  : span 
所有的： a abbr  acronym  b bdo   big br  button  cite
code   dfn   em i  img   input   kbd   label   map  object   
output   q  samp   script  select   small  span   strong  
sub   sup   textarea    time tt var
-->

    <!-- HTML 类属性 -->

    <div class="city">
        <h2>London</h2>
        <p>London is the caption of England.</p>
    </div>
    <!-- id 属性
#  类和id 的区别： 类名可以被多个元素使用 ，而一个 id 名只能被页面中的一个 元素使用 
-->

    <!-- HTML iframe 
在文档中嵌入一个 html 文档
border:none 移除边框
链接目标： target  属性必须引用 name iframe 属性
-->

    <iframe src="http://www.taobao.com" width="200" height="200" style="border:none;" title="百度"></iframe>

    <iframe src="http://www.taobao.com" name="iframe_a" title="淘宝"></iframe>
    <P><a href="https://www.w3schools.com" target="iframe_a">W3schools</a></P>


    <!-- HTML 与 JS
使用 JS 可以改变样式，属性等

-->

    <h1>My First JavaScript</h1>

    <button type="button" onclick="document.getElementById('demo').innerHTML = Date()">
        Click me to display Date and Time.</button>

    <p id="demo"></p>

    <!-- noscript 标签
这个标签定义了要向浏览器中禁用脚本或浏览器不支持脚本的用户显示的替代内容
<script>
document.getElementById("demo").innerHTML = "Hello JavaScript!";
</script>
<noscript>Sorry, your browser does not support JavaScript!</noscript>
-->

    <!-- HTML 布局 -->

    <header>定义文档或部分的标题</header>
    <nav>定义一级导航链接</nav>
    <section>定义文档中一个部分</section>
    <aside>定义内容之外的内容（侧边栏）</aside>
    <footer>定义文档或部分的页脚</footer>
    <details>定义用户可以按需打开和关闭的其他详细信息</details>
    <summary>定义标题 <details>元素</summary>

</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- HTML 响应式网页设计
   将自动调整不同的屏幕尺寸和视口。
   需要加下面这个 meta
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>


    <!-- 画布相关 -->
    <script>
        var c = document.getElementById("myCanvas");
        var ctx = c.getContext("2d");

        // Create gradient
        var grd = ctx.createLinearGradient(0, 0, 200, 0);
        grd.addColorStop(0, "red");
        grd.addColorStop(1, "white");

        // Fill with gradient
        ctx.fillStyle = grd;
        ctx.fillRect(10, 10, 150, 80);
    </script>



</head>

<body>
    <!-- 响应式文本大小
vw  单位设置文本大小 ， 视口宽度 ， 文本大小将跟随浏览器窗口的大小
-->
    <h1 style="font-size:10vm">Hello World</h1>


    <!-- HTML 计算机代码元素

code 不会保留额外的空格和换行符，如果需要保留可以放在元素 pre内
-->

    <code>
        x = 5;
        y = 6;
        z = x + y;
    </code>

    
<pre>
<code>
x = 5;
y = 6;
z = x + y;
</code>
</pre>
<p>Save the document by pressing <kbd>Ctrl + S</kbd></p>

    <!-- 
samp 用于程序输出 
 -->


    <p><samp>File not found.<br>Press F1 to continue</samp></p>


    <!-- HTML var 用于变量 -->
    <p>1/2 x <var>b</var> x <var>h</var></p>

    <!-- 
文件扩展名 : .htm 和 .html 是一样的 web 服务器都视为HTML

   一些字符实体: 
 -->
    &nbsp;
    &It;
    &gt;
    &euro;
    &spades;
    &clubs;
    &hearts;
    &diams;

    <!-- HTML 表情符号 -->
    <p>&#128512;</p>
    <p style="font-size:48px">
        &#128512; &#128516; &#128525; &#128151;
    </p>


    <!-- HTML 与 XHTML
XHTML 是更严格，更基于 xml 的 HTML 版本
与 HTML 最重要的区别
<!DOCTYPE> 是强制性的
<html> 中的 xmlns 属性是必需的
<html>、<head>、<title> 和 <body> 是必需的
元素必须始终正确嵌套
元素必须始终关闭
元素必须始终为小写
属性名称必须始终为小写
必须始终引用属性值
禁止属性最小化
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN"
"http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <title>Title of document</title>
</head>
<body>

  some content here...

</body>
</html>
-->

    <!-- form 元素
用于为用户输入创建的HTML 表单
type 的类型有：
text   radio   checkbox   submit   button
-->

    <form>
        <label for="fname">First name:</label><br>
        <input type="text" id="fname" name="fname"><br>
        <label for="lname">Last name:</label><br>
        <input type="text" id="lname" name="lname">
    </form>

    <!-- 表单属性
action 属性定义了提交表单时要执行的操作
target  属性指定在哪里显示提交表单后收到的响应
可选值 ： _blank  _self(默认在当前窗口中打开)  _parent  _top  framename
-->
    <form action="../images/syhk.txt" target="_blank">
        <input type="text" value="syhhk">
        <br>
        <input type="text" value="shenyang">
        <br>
        <input type="submit" value="submit">
    </form>

    <!-- 方法属性
在提交表单数据时使用 get 方法； 为 post 就是 POST 方法
-->
    <form action="../syhk.txt" method="get">

    </form>

    <!-- 自动完成属性
autocomplete 属性指定表单是否应该打开或关闭自动完成功能
启用后，浏览器会根据用户之前输入的值自动完成值 
-->
    <form action="../images/syhk.txt" autocomplete="on">
        <input type="text" value="syhk">
        <input type="submit" value="submit">
    </form>

    <!-- Novalidate 属性
它是一个布尔值属性。
当存在时，它指定表单数据输入在提交时不应该被验证
-->
    <form action="../images/syhk.txt" novalidate>

    </form>

    <!-- HTML 表单元素
<form> 元素可以包含以下一个或多个表单元素：
    <input>  <label> <select> <textarea> <button>
        <fieldset>  <legend> <datalist> <output> <option> <optgroup>
    
label 定义了几个表单元素的标签
select 定义了一个下拉列表
option 元素定义了一个可以选择的选项

-->
    <!-- 
    size 属性指定可见值的数量 
    使用 multiple 属性允许 用户选择多个值 
 -->
    <label for="cars">Choose a car:</label>
    <select id="cars" name="cars" size="3" multiple>
        <option value="volvo">Volvo</option>
        <option value="saab">Saab</option>
        <option value="fiat">Fiat</option>
        <option value="audi">Audi</option>
    </select>

    <!-- 
textarea 元素
rows 属性指定文本区域中的可见的行数
cols 属性指定文本区域中的可见的宽度

     -->
    <textarea name="message" rows="10" cols="30">
The cat was playing in the garden.
</textarea>

    <!-- button 元素  -->
    <button type="button" onclick="alert('Hello World!')">Click Me!</button>


    <!-- 
    fieldset  和 legend 元素
    fieldset 元素用于对表单中的相关数据进行分组
    legend 元素定义了该元素的标题
 -->
    <form action="../images/syhk.txt">
        <fieldset>
            <legend>Personalia:</legend>
            <legend for="fname">First name:</legend>
            <input type="text" id="fname" value="John">
            <br>
            <label for="Iname">Last name:</label>
            <input type="text" id='lname' name="lname" value="Doe">
            <br>
            <input type="submit" value="submit">
        </fieldset>
    </form>

    <!-- 
datalist 元素指定元素的预定义选项列表 input
用户在输入数据时将看到预先定义的下拉列表
元素的 list 属性 <input> 必须引用元素的 id 属性 <datalist>
也就是输入选项框
 -->
    <form action="/action_page.php">
        <input list="browsers">
        <datalist id="browsers">
            <option value="Internet Explorer">
            <option value="Firefox">
            <option value="Chrome">
            <option value="Opera">
            <option value="Safari">
        </datalist>
    </form>

    <!-- 
    output 元素
    用于表示计算的结果(如执行脚本的计算)
 -->
    <form action="/action_page.php" oninput="x.value=parseInt(a.value)+parseInt(b.value)">
        0
        <input type="range" id="a" name="a" value="50">
        100 +
        <input type="number" id="b" name="b" value="50">
        =
        <output name="x" for="a b"></output>
        <br><br>
        <input type="submit">
    </form>


    <!-- HTML 输入类型
<input> 元素的不同类型
button  checkbox    color  date   datetime-local
email    file   hidden   image   month  number
password   radio   range    reset    search   
submit    tel test   time   url   week
type 属性的默认值为 text 
-->
    <!-- 输入密码时不显示数字，显示 ... -->
    <form>
        <label for="username">Username:</label><br>
        <input type="text" id="username" name="username"><br>
        <label for="pwd">Password:</label><br>
        <input type="password" id="pwd" name="pwd">
    </form>

    <!-- 输入复位类型 -->
    <form action="../images/syhk.txt">
        <label for="fname">First name:</label><br>
        <input type="text" id="fname" name="fname" value="John"><br>
        <label for="lname">Last name:</label><br>
        <input type="text" id="lname" name="lname" value="Doe"><br><br>
        <input type="submit" value="Submit">
        <input type="reset">
    </form>

    <!-- radio 表示 单选 -->

    <!-- checkbox 复选框  -->


    <!-- 输入类型颜色  
也就是颜色选项框，可以选择颜色 
-->

    <form>
        <label for="favcolor">Select your favorite color:</label>
        <input type="color" id="favcolor" name="favcolor">
    </form>
    <!-- 
    日期类型
    也就是可以选择日期
    min 和 max 属性可以为日期添加限制 
 -->
    <form>
        <label for="birthday">Birthday:</label>
        <input type="date" id="birthday" name="birthday">
    </form>


    <!-- Datetime-local
    指定日期和时间输入字段 
    和上面那个功能差不多
 -->
    <form>
        <label for="birthdaytime">Birthday (date and time):</label>
        <input type="datetime-local" id="birthdaytime" name="birthdaytime">
    </form>

    <!-- email 类型 -->
    <form>
        <label for="email">请输入你的 email:</label>
        <input type="email" id="email" name="email">
    </form>

    <!-- image
图像路径在 src 中指定
 -->
    <form>
        <input type="image" src="../images/syhk.gif" alt="Submit" width="48" height="48">
    </form>

    <!-- file
文件上传定义了一个文件选择字段 和一个浏览按钮
可以弹出文件上选择窗口然后选择上传
-->

    <form>
        <label for="myfile">Select a file:</label>
        <input type="file" id="myfile" name="myfile">
    </form>


    <!-- hidden 隐藏
定义了一个隐藏输入字段对用户不可见
虽然这个值在页面内容中不会显示出来，但是可以使用任何浏览器的开发人员工具或查看源代码阿勒，就可以看到并且可以编辑，是不安全的

-->
    <form>
        <label for="fname">First name:</label>
        <input type="text" id="fname" name="fname">
        <br>
        <input type="hidden" id="custId" name="custId" value="3487">
        <input type="submit" value="submit">
    </form>

    <!-- month  
就是只能选择月份
-->
    <form>
        <label for="bdaymonth">Birthday :</label>
        <input type="month" id="bdaymonth" name="bdaymonth">
    </form>

    <!-- number
只能输入数字 
min max 属性可以限制输入的范围
-->

    <form>
        <label for="quantity">Quantity (1-5):</label>
        <input type="number" id="quantity" name="quantity" min="1" max="5">
    </form>

    <!-- tel 电话号码  -->
    <form>
        <label for="phone">Enter your phone number:</label>
        <input type="tel" id="phone" name="phone" pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}">
    </form>


    <!-- 
input 元素的 value 属性可以指定输入字段的初始值 
readonly 属性指定输入字段是只读的不能修改输入字段
disabled 属性指定禁用输入字段 ,禁用的输入字段不可用且不可点击，提交表单不会发送禁用输入字段的值 
size  属性指定输入字的可见宽度（以字符为单位）默认值为 20
maxlength 属性指定输入字段中允许的最大字符数
min max 属性指定输入字段的最小值 和最大值 
multiple 属性指定允许用户在输入字段中输入多个值 
pattern 属性指定了一个正则表达式，当提交表彰时，输入字段的值将被 检查 
placeholder 属性指定描述输入字段的预期值简短提示
required 属性指定在提交表单之前必须填写输入字段
step 属性指定输入字段的合法数字间隔
autofocus 属性输入字段应在页面加载时自动获得焦点
height  width 属性指定高度和宽度
list 属性是指 <datalist> 包含 <input> 元素的预定义选项的元素
autocomplete 属性指定表单或输入字段是否应该打开或关闭自动完成功能
（当用户输入字段时，浏览器应根据之前输入的值显示填充字体的选项）
 -->
    <form>
        <label for="phone">Enter a phone number:</label>
        <input type="tel" id="phone" name="phone" placeholder="123-45-678" pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}">
    </form>

    <!-- 就是页面加载时，光标自动跳到输入框中 -->

    <form>
        <label for="fname">First name:</label><br>
        <input type="text" id="fname" name="fname" autofocus><br>
        <label for="lname">Last name:</label><br>
        <input type="text" id="lname" name="lname">
    </form>


    <!-- 表单属性
form 属性指定 <input> 元素所属的表单
这个属性的值必须等于它所属的 <form> 元素的 id 属性

    formaction 属性 指定提交表彰时将进行处理输入的文件的 URL
    这个属性会覆盖元素的 action 属性。 适用于输入类型为： submit 和 image
-->
    <form action="/action_page.php">
        <label for="fname">First name:</label>
        <input type="text" id="fname" name="fname"><br><br>
        <label for="lname">Last name:</label>
        <input type="text" id="lname" name="lname"><br><br>
        <input type="submit" value="Submit">
        <input type="submit" formaction="/action_page2.php" value="Submit as Admin">
    </form>

    <!-- 
formctype属性指定表单数据在提交时应如何编码（仅适用于带有 method="post" 的表单)
这个属性会覆盖 <form> 元素的 enctype 属性
 -->

    <form action="/action_page_binary.asp" method="post">
        <label for="fname">First name:</label>
        <input type="text" id="fname" name="fname"><br><br>
        <input type="submit" value="Submit">
        <input type="submit" formenctype="multipart/form-data" value="Submit as Multipart/form-data">
    </form>

    <!-- 
formmethod 属性定义了将表单数据发送到操作 URL 的 HTTP 方法
这个属性 会覆盖  <form> 元素的 method 属性
 -->

    <!-- 
formtarget 属性
它指定一个名称或关键字，指示在哪里提示提交表彰后收到的响应
它会覆盖 <form> 元素的目标属性
 -->

    <!-- 
formnovalidate 属性
它指定提交时不应验证<input> 元素

novalidate 元素 当它存在时提交时不应验证所有表单数据 


  -->

    <!-- HTML 画布
canvas 元素用于通过 js 动态绘制图形
它只是图形的容器。
默认情况下画布没有边框，也没有内容
id 属性 在脚本中引用
-->
    <!-- 
<canvas id="myCanvas" width="200" height="100"></canvas> -->


    <canvas id="myCanvas" width="200" height="100" style="border:1px solid #000000;">
    </canvas>



    <!-- HTML svg 元素
svg 元素是svg 图形的容器
-->

    <svg width="100" height="100">
        <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />

        <br>
        <svg width="300" height="200">
            <polygon points="100,10 40,198 190,78 10,78 160,198"
                style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;" />
        </svg>


        <!-- HTML 多媒体 -->

        <!-- video 显示视频元素
source 指定浏览器可以选择的替代视频文件。
controls 属性添加了视频控件，eg：播放暂停等
autoplay 属性指定视频自动播放
muted 视频自动播放并静音
-->

        <video width="320" height="240" controls>
            <source src="movie.mp4" type="video/mp4">
            <source src="movie.ogg" type="video/ogg">
            Your browser does not support the video tag.
        </video>


        <!-- audio 元素用于在网页上播放音频文件
controls 属性添加了 音频控件，播放暂停等
source 指定浏览器可以选择的替代音频文件。
autoplay 自动播放
muted 静音处理
-->

        <audio controls autoplay muted>
            <source src="horse.ogg" type="audio/ogg">
            <source src="horse.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
        </audio>

        <!-- HTML 插件 
大多数浏览器不支持 java 小程序 和插件
任何浏览器不支持 activeX  控件
object 元素 定义了 HTML 文档中的嵌入对象。
-->
        <object width="100%" height="500px" data="./test1.html" type="">这是第一个页面</object>
        <!-- 上面这个相当于在页面中嵌入中 test1.html 页面 -->

        <object data="../images/syhk.gif"></object>

        <!-- 
    embed 元素 定义了 HTML 文档中的嵌入对象
    它没有结束标记,它不能包含替代文本
    也可以包含 HTML
 -->
        <embed src="../images/9mjoy1.jpg">

        <!-- 
HTML YouTube 视频 

 -->
        <iframe width="420" height="315" src="https://www.youtube.com/embed/tgbNymZ7vqY">
        </iframe>

        <!-- 
    使元素可以拖动 
    将 draggable 属性设置为 true
 -->
        <img src="../images/9mjoy1.jpg" alt="可搬运" draggable="true">




</body>

</html>
```

[test4.html](file/test4_ALeZZksHaM.html)


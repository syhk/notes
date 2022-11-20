盒子模型

![image_IYsaR_GHBI.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663901119232-623d7c28-c3c6-4ff3-b997-bdf91b0a4c16.png#averageHue=%23cec3b9&clientId=u0ec0a435-99b6-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=ui&id=u487b84c5&margin=%5Bobject%20Object%5D&name=image_IYsaR_GHBI.png&originHeight=509&originWidth=895&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27669&status=error&style=none&taskId=ueb5d47fb-e293-4228-a3a8-fbbbad7dd61&title=)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CSS 测试</title>
    <link rel="stylesheet" href="test.css" />
    <!-- 引入成功 -->

    <!-- 引入  google 字体 -->
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css?family=Sofia"
    />

    <!-- CSS icon 图标 -->
    <script
      src="https://kit.fontawesome.com/yourcode.js"
      crossorigin="anonymous"
    ></script>
  </head>

  <style>
    body {
      font-family: "Sofia", sans-serif;
      font-size: 30px;
      text-shadow: 3px 3px 3px #ababab;
    }
  </style>

  <body>
    <h1>这是一级标题</h1>
    <p>这是二级标题</p>
    <h1 style="background-color: rgb(255, 99, 71)">...</h1>
    <h1 style="background-color: #ff6347">...</h1>
    <h1 style="background-color: hsl(9, 100%, 64%)">...</h1>

    <h1 style="background-color: rgba(255, 99, 71, 0.5)">...</h1>
    <h1 style="background-color: hsla(9, 100%, 64%, 0.5)">...</h1>

    <p>my name is syhk</p>
    <p>123456 6567 5654</p>

    <!-- 启用 google 字体效果 -->
    <p class="font-effect-fire">My name is syhk.</p>
    <h1 class="font-effect-neon">Neon Effect</h1>
    <h1 class="font-effect-outline">Outline Effect</h1>
    <h1 class="font-effect-emboss">Emboss Effect</h1>
    <h1 class="font-effect-shadow-multiple">Multiple Shadow Effect</h1>

    <!-- 使用 CSS 图标 -->
    <i class="fas fa-cloud"></i>
    <i class="fas fa-heart"></i>
    <i class="fas fa-car"></i>
    <i class="fas fa-file"></i>
    <i class="fas fa-bars"></i>
  </body>
</html>
```

```css
body {
  /* background-color: lightblue;  */
  /* 指定元素的背景颜色  */
  /* opacity: 0.3; */
  /* 指定元素的不透明度/透明度  0.0 - 1.0  元素的的子元素会继承相同的透明度*/
  /* 使用 RGBA 的透明度 */
  /* background: rgba(46, 210, 239,01); */
  /* background-image: url("../images/9mjoy1.jpg"); */
  /* 指定图片为背景  默认情况下这个图片会水平和垂直重复图像*/
  /* background-repeat: repeat-x; */
  /* 指定只水平重复  -y 是垂直显示 图像 */
  /* background-repeat: no-repeat; */
  /* 指定只显示一次背景图像 */
  /* ======================================== */
  /* background-position: right top; */
  /* 指定图像的位置,右上角 */

  /* 指定背景图像应该滚动还是固定  fixed 固定  scroll 滚动 */
  background-attachment: scroll;

  /* 速记属性  : 可以使用这一个代替上面几个 
    它的值是顺序为 : color  image   repeat  attachment  position
    */
  background: #ff3422 url("../images/syhk.gif") no-repeat right top;
}

h1 {
  color: white;
  text-align: center;
}

p {
  font-family: verdana;
  font-size: 20px;

  /* ============ css 边框  ==========================*/
  border-style: 1px inset;
  /* style 属性有 1 到 4 个值分别是 上 右 下 左 */
  /* 
    dotted 定义虚线框
    dashed 定义虚线框
    solid  实心边框
    double 双边框
    groove  定义3D 凹槽
    ridge  3D 脊状边界
    inset 3D插入边框
    outset 3D起始边界
    以上4个 3D 的效果取决于 border-color 的值 
    none 无边界
    hidden 隐藏边框    
    */

  border-width: 25px 10px 4px 35px;
  /* 上 右 下 左 */

  /* 边框颜色 
    可以使用: name   HEX  RGB  HSL  透明度
    */

  /* 单独指定每个边框 */

  border-top-style: dotted;
  border-right-style: solid;
  border-bottom-style: dotted;
  border-left-style: solid;

  border-style: dotted solid;
  /* 这个和上面 4 条语句的效果是一样的 */

  /* CSS 圆角边框 */
  border-radius: 5px;

  /* css 边距 */
  margin-top: 100px;
  margin-bottom: 100px;
  margin-right: 100px;
  margin-top: 100px;
  /* 速记属性   上 右 下  左 
    有三个值是 : 上 左  下
    有两个值是 :  顶部和底部是第一个值   左右 是第二值 
    有一个值是 : 四边都是一样的
    auto 自动值,将左右边距之间平均分配

    */
  margin: 100px 100px 100px 100px;
  margin: auto;
}

h2 {
  /* 轮廓
有围绕元素绘制的线,在边界之外,使元素突出
outline-style 的样式和边框的一样  dotted ...
outline - width 指定轮廓的宽度
outline 速记:它的值是顺序不重要
outline-offset 指定轮廓和元素的边界之间添加的空间大小,这个空间是透明的


*/
}

.text {
  color: red;
  /* 文本对齐 */
  text-align: center;

  /* 指定如何对齐文本的最后一行 */
  text-align-last: center;

  /* 用于更改文本方向  */
  direction: rtl;
  unicode-bidi: bidi-override;

  /* 设置元素的垂直对齐方式 */
  vertical-align: auto;

  /* 文字装饰 */

  /* 用于向文本添加装饰线  */
  text-decoration-line: overline;

  /* 指定装饰线的颜色  */
  text-decoration-color: aqua;

  /* 指定装饰线的样式 */
  text-decoration-style: dashed;

  /* 指定装饰线的粗细 */
  text-decoration-thickness: 5px;

  /* 速记属性  line 属性是必须的,其他都是可选的 */
  text-decoration: underline red double 5px;

  /* 文本转换
    
    */

  /* 大写 */
  text-transform: uppercase;
  text-transform: lowercase;

  /* 文本间距 */

  /* 指定文本第一行的缩进 */
  text-indent: 50px;

  /* 指定文本中字符之间的空格 */

  letter-spacing: 5px;

  /* 指定行间距 */
  line-height: 0.8;

  /*字间距 */

  word-spacing: 10px;

  /* 白色空间 */
  white-space: nowrap;

  /* ===========  文字阴影======================= */
  text-shadow: 2px 2px 5px red;

  /* 这个用于指定文本的字体 */

  font-family: Verdana, Geneva, Tahoma, sans-serif;

  /* 指定字体样式   
    normal  正常显示   italic 斜体显示   oblique 倾斜
    */
  font-style: normal;

  /* 指定字体的粗细 */
  font-weight: bold;

  /* 指定文本是否应以小型大写字母显示  */
  font-variant: small-caps;

  /* 字体大小 */
  font-size: 40px;
}

/* google 字体 */
.google-text {
  /* 
有 1000 多种字体可以使用
只需要在 head 标签中添加一个特殊的样式表链接
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Sofia">

*/
}

/* CSS 链接 */
a {
  color: hotpink;
  /* 
    a:link 一个正常的,未被访问的链接
    a:visited 用户访问过的链接
    a:hover 用户将鼠标悬停在链接上时的链接
    a:active 点击时的链接
    这几个设置样式时,有一些顺序规则, a:hover 必须在 a:link 和 a:visited 之后 
    a:active 必须在 a:hover 之后 
    */
}

/* CSS 列表 */
ul {
  /* 指定标记的样式 */
  list-style-type: circle;

  /* 使用图片作为标记 */
  list-style-image: url("../images/syh.ico");

  /* 指定列表项标记的位置也就是标记符号的位置 */
  list-style-position: inside;

  /* 速记属性 */
  list-style: square inside url("../images/syh.ico");
}

/* 
css 布局
*/
div {
  /* none 用来隐藏和显示元素 
    hidden 隐藏
    */

  /* 
inline-block 和 inline的区别:  
inline-block 允许在元素上设置宽度和高度
block 和 inline-block 元素后不添加换行符
*/

  display: inline;

  /* 位置属性
    static  静态
    relative  相对
    fixed  固定
    absolute  绝对
    sticky   粘性
    
    */
  position: static;

  /* 指定元素的堆栈顺序 也就是哪个元素放在其他元素的前面或后面
    如果两个元素没有指定这个属性的时候,相互重叠,HTML 代码中最后定义的元素将显示在顶部
    */
  z-index: -1;

  /* CSS 溢出 
    visible 默认,溢出没有被剪裁.内容在元素的框外
    hidden 被剪裁,不可见
    scroll 被剪裁,并添加滚动条以查看其余内容
    auto 类似  scroll 但公在必须时添加滚动条
    这个属性只适用于指定高度的块元素上
     overflow-y 和 ...-x 用于指定是否水平或垂直或两者更改内容溢出

*/
  overflow: visible;
}

/* CSS 浮动和清晰 */
.tdonhua {
  /* 
float 指定元素应该如何浮动
left 元素浮动在容器的左侧
right 元素浮动在容器的右侧 
none 元素不浮动
inherit 元素继承父元素的浮点值 
使用 float 属性,可以轻松并排浮动内容框

clear 指定哪些元素可以浮动在被清除的元素旁边以及在哪一侧.
none 元素不会推到右或左浮动元素的下方,默认的
left 左浮动元素下方
right 右浮动元素下方
both 元素被推左右浮动元素的下方
inherit 元素从其父元素继承明确的值 
*/
}

/* CSS 选择器 */

/* 后代选择器
指定元素的后代的所有元素
*/
div p {
  /* .... */
}
/* 子代选择器
指定元素的子元素的所有元素
*/
div > p {
  /* .... */
}
/* 相邻兄弟选择器  
兄弟元素必须有相同的父元素
*/
div + p {
  /* .... */
}
/* 通用兄弟选择器 
指定元素的下一个兄弟的所有元素
*/
div ~ p {
  /* .... */
}

/* CSS 边框图像 */
div {
  /* 指定元素的边框为图像 */
  border-image: url("../images/syh.ico") 30 round;

  /* CSS 多背景 */
  /* 可以添加多个背景图片,用 ,  隔开 */
  background-image: url(../images//9mjoy1.jpg), url("../images/syhk.gif");
  /* 分别指定位置,也  , 隔开 */
  background-position: right bottom, left top;
  /* 属性之间也是用 ,  隔开 */
  background-repeat: no-repeat, repeat;
  /* 指定背景图像的大小
contain  图像放到尽可能大
cover  使用内容区域完全被图像覆盖
指定多个图像的大小,也是用 ,  隔开
*/
  background-size: auto, 50px, 10px;

  /* 属性指定 背景图像的位置
    padding-box 默认背景图像从填充边缘的左上角开始
    border-box 背景图像从边框的左上角开始
    content-box 背景图像从内容的左上角开始

    */
  background-origin: border-box;

  /*  指定颜色透明,用于为元素制作透明的背景颜色 
 currentcolor 关键字像一个变量,保存元素颜色属性的当前值 
 inherit 指定属性应从父元素继承值
*/
  background-color: transparent;
}

/* CSS 渐变  */
body {
  /* 
    有三种渐变类型: 
    线性渐变  向下 向上 向左 向右 对角线
    径向渐变  由它们的中心点定义
    圆锥渐变  围绕中心旋转

    */
  /* background-image: linear-gradient(direction,color1 , color 2,.....); */

  background-image: linear-gradient(red, yellow);

  /* 方向: 从左到右 */
  background-image: linear-gradient(to right, red, yellow);

  /* 方向: 从左上到右下 */
  background-image: linear-gradient(to bottom right, red, yellow);

  /* 使用渐变 使用角度 */
  /* background-image: linear-gradient(角度,颜色1 ,颜色 2....); */
  background-image: linear-gradient(180deg, red, yellow);

  /* 使用透明度 */

  background-image: linear-gradient(
    to right,
    rgba(255, 0, 0, 0),
    rgba(255, 0, 0, 1)
  );

  /* 重复性渐变  */
  background-image: repeating-linear-gradient(red, yellow 10%, green 20%);

  /* 径向渐变 由中心定义 */
  background-image: radial-gradient(red, yellow, green);
  background-image: radial-gradient(red 5%, yellow 15%, green 60%);

  /* 使用形状 */
  background-image: radial-gradient(circle, red, yellow, green);

  /* 圆锥渐变  围绕中心旋转的渐变 也可以指定度数 */
  background-image: conic-gradient(red 45deg, yellow, green);
}

.box {
  /* 盒子阴影 
可用来创建卡片
*/
  box-shadow: 10px 10px lightblue;
}

/* CSS 动画
@keyframes 规则中指定 css 样式时,动画会在特定时间逐渐从当前样式更改为新样式


*/

/* 例子: */
@keyframes syhk {
  /* from 代表 0%   to 代表 100%  */

  from {
    background-color: red;
  }
  to {
    background-color: yellow;
  }
  /* 可以设置 0%  25%  50%  100% */
  0% {
    background-color: red;
  }
  25% {
    background-color: yellow;
  }
  50% {
    background-color: blue;
  }
  100% {
    background-color: green;
  }
}
/* 使用上面的动画 */
#div1 {
  background-color: red;
  animation-name: syhk;
  /* 定义了动画需要多长时间才能完成,不指定这个属性,不会出现动画,默认值 0s */
  animation-duration: 4 s;

  /* 指定动画开始的延迟,允许使用负值,动画开始播放,就好像已经播放了 N 秒一样 */
  animation-delay: 2s;

  /* 指定动画应该运行的次数  infinite 使用动画永远持续 */
  animation-iteration-count: infinite;

  /* 指定动画是否应该向前,向后,或交替循环播放
    normal 正常播放向前,默认的
    reverse 反向播放
    alternate 动画先向前播放,再向后播放
    alternate-reverse 动画先后播放,再向前播放
    */
  animation-direction: normal;

  /* 指定动画的速度曲线
    ease 指定一个慢开始,然后快,然后慢慢结束的动画,默认的
    linear 从开始到结束速度相同的动画
    ease-in 一个慢启动的动画
    ease-out 一个缓慢结束的动画
    ease-in-out 具有缓慢开始和结束的动画
    cubic-bezier(n,n,n,n) 在函数中定义自己的值

*/
  animation-timing-function: ease;

  /* 指定动画末播放时目标元素的样式
    none 默认值,不会对元素应用任何样式
    forwards 元素将保留最后一个关键帧设置的样式值
    backward 元素将获取第一个关键帧设置的样式值
    both 将遵循向前或向后的规则,在两个方向上扩展动画属性
    */
  animation-fill-mode: none;

  /* =============动画速记属性============= */
  /* 分别指定的是: name   */
  animation-name: syhk;
  animation-duration: 5 s;
  animation-timing-function: liner;
  animation-delay: 2s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  /* 等价于下面这一行 */
  animation: syhk 5s linear 2s infinite alternate;
}

/* 样式图片 */
img {
  /* 椭圆形图片 */
  border-radius: 50%;
}

/* css 图像反射 
box-reflect 指定创建图像反射
值有： below  above  left  right

*/
img {
  -webkit-box-reflect: left;
}

/* CSS button*/
button {
  /* 指定颜色  */
  background-color: red;

  /* 指定按钮字体大小 */
  font-size: 4px;

  /* 椭圆形按钮 */
  border-radius: 50%;

  /* 彩色边框 */
  border: 2px solid tomato;

  /* 阴影按钮 */
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);

  /* 禁用按钮 */
  cursor: not-allowed;

  /* 属性来确定悬停效果的速度 */
  transition-duration: 0.4s;
}

/* 可悬停按钮 : 当鼠标移动到按钮上时更改样式*/
button:hover {
  background-color: aquamarine;
  color: white;
}

/* CSS 创建多列 */
div {
  /* 指定一个元素应该分成的列数 */
  column-count: 3;

  /* 属性指定列之间的间隙 */
  column-gap: 40px;

  /* 指定列之间规则的样式 */
  column-rule-style: solid;

  /* 属性指定列之间的规则宽度 */
  column-rule-width: 1px;

  /* 指定列之间规则的颜色 */
  column-rule-color: lightblue;

  /* 速记属性 */
  column-rule: 1px solid lightblue;

  /* 指定一个元素应该跨越多少列 */
  column-span: all;

  /* 指定建议的最佳宽度 */
  column-width: 100px;

  /* 调整大小 */
  /* 指定用户是否可以调整元素的大小 */
  resize: horizontal;
}

/* var（） 函数 
用于插入 css 变量的值
var (--name,value)
变量名必须以 -- 开关；区分大小写
CSS变量具有全局变量和局部变量
全局变量在 :root 中声明 
局部变量在使用它的选择器中声明它
*/

/* 定义 全局变量 */
:root {
  --blue: #f71904;
  --syhk: rgba(250, 24, 148, 0.2);
}
/* 使用 */
h3 {
  /* 使用上面定义的全局变量 */
  background-color: var(--syhk);
}

button {
  /* 局部变量 */
  --syhk: #f08;
  /* 局部变量会覆盖全局变量，使用更具体的 */
  background-color: var(--syhk);
}

/* css 框大小 */
box {
  /* 允许我们在元素的总宽度和高度中包含填充和边框 */
  /* border-box 填充和边框包含在宽度和高度中 */
  box-sizing: border-box;
}

/* CSS flexbox 布局模块 */
```

### 知识点

```css
  /* 
css  单位:
 vw : 浏览器可见视口宽度的百分比 (1vw 代表视窗 的 1%)
 vh : 浏览器可见视口高度的百分比 (1vh 代表视窗高度的 1%)
 vmin : 当前 vw  和 vh 较小的一个值
 vmax : 较大的一个值 
 与 % 的区别：
 % 是基于父元素的宽度/高度的百分比； vw,vh 是根据视窗的宽度和高度的百分比
 视口的优势在于 vh 能够直接获取高度,而用 % 在没有设置 body 的情况下，无法正确获得可视区域的高度
*/
```

#### display
它规则是否/如何显示元素（元素被视为块元素还是内联元素）
每个 html 元素都有一个默认的 display 值，具体值取决于它的类型
```css
display: none; 用来隐藏和显示元素（默认的），好像这个元素不在页面中存在

visibility:hidden 也可以隐藏元素，隐藏后仍然会占用与之前相同的空间

/*==========================================================  */
/* precomposed values */
display: block;
display: inline;
display: inline-block;
display: flex;
display: inline-flex;
display: grid;
display: inline-grid;
display: flow-root;

/* box generation */
display: none;
display: contents;

/* two-value syntax */
display: block flow;
display: inline flow;
display: inline flow-root;
display: block flex;
display: inline flex;
display: block grid;
display: inline grid;
display: block flow-root;

/* other values */
display: table;
display: table-row; /* all table elements have an equivalent CSS display value */
display: list-item;

/* Global values */
display: inherit;
display: initial;
display: revert;
display: revert-layer;
display: unset;
```
#### postion 属性
值： static relative fixed  absolute  sticky

- static 默认

它不受 top  bottom  left  right 属性的影响

- relative 元素相对于其正常位置进行定位 （top , left , right , bottom)
- fixed : 元素相对于视口定位的，页面滚动，它也始终位于同一个位置
- absolute：元素相对于最近的定位祖先元素进行定位，如果没有祖先使用 body 作为，随着页面一起滚动
- sticky ：元素根据用户的滚动位置进行定位 
#### z-index 指定元素的堆栈顺序（哪个元素放置在其他元素的前面或后面）

要使块元素水平居中使用 `margin:auto`
如果没有设置 width 属性 （或设置为 100%），居中对齐就无效

居中对齐也可以使用
```css
margin-left:auto;
margin-right:auto;
```
 css 一个元素/组件可以应用多个动画，多个动画之间用逗号分隔，前提是要不冲突

CSS 属性书写顺序：

- 布局定位属性
- 自身属性
- 文本属性
- 其他属性

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664587042490-9ac1877d-1452-4bda-8497-7726a6ee9390.png#averageHue=%23f3f4f3&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=190&id=u7ce5cad0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=237&originWidth=756&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75968&status=error&style=none&taskId=uf858e5a8-d382-4d96-bbe8-5597e155fd5&title=&width=604.8)
### CSS 三大特性
层叠性，继承性，优先级
层叠性：样式冲突（前提：样式冲突，相同选择器给相同的样式），遵循的原则是就近原则 ，哪个样式离结构近，就执行哪个样式。
继承性：子元素可以继承父元素的样式（text- , font- , line- 这些元素开头的可以继承及 color 属性）
行高的继承 特性：
```html
  <style>
      /* div {
        color: tomato;
        font-size: 14px;
      }
      p {
        color: blue;
        font-size: 4px;
      } */
      body {
        color: aqua;
          /* 1.5 不带单位是倍数，子元素字体大小的 1.5 倍 */
        font: 20px/1.5 "Microsoft YaHei";
      }
      div {
        /* 子元素继承了父元素的 body 的行高 1.5  */
        /* 1.5 是当前元素文字大小的 font-size 的 1.5 倍 当前div 的行高是 1.5 * 14*/
        font-size: 14px;
      }
      p {
        /* 1.5 * 16  */
        font-size: 16px;
      }
    </style>
  </head>
  <body>
    <div>这是三大特性</div>
    <p>这是子</p>
    <ul>
      <!-- 20 * 1.5  向上找父亲 -->
      <li>没有文字大小</li>
    </ul>
  </body>
```
优先级:当一元素指定了多个选择器，就会有优先级的问题

- 选择器相同，执行层叠性
- 选择器不同，根据权重执行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664588674763-298186b8-d68c-4c9a-bd6e-f81a683586c5.png#averageHue=%23f8f9f8&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=147&id=uecebd0f1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=184&originWidth=540&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29301&status=error&style=none&taskId=u157bed8e-9e9b-4e25-9657-ecdbaeaeba4&title=&width=432)
权重不会有进位
继承是权重是 0
```html
  <style>
        /* 权重 100 */
        #father{
            color: red;
        }

        /* p 继承的权重为 0 */
        p{
            color: burlywood;
        }
  
    </style> 
  </head>
  <body>
   <!-- burlywood -->
    <div id="father">    <p >这是子</p> </div>
  </body>
```
```html
 <style>
      
      /* 权重为 0,0,0,1 */
      li {
        color: red;
      }

      /* 复合选择器权重为 ： 0,0,0,1 + 0,0,0,1 = 0,0,0,2   所以显示这个颜色 */
      ul li{
        color: teal;
      }
  
/* 权重会叠加，永远不会有进位 */
      /* 0,0,1,0 + 0,0,0,1 = 0,0,1,1 */
      .nav li {
        color: blueviolet;
      }

    </style> 
  </head>
  <body>
   
    <ul class="nav">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664589217239-bb6fd3b1-7a40-4e91-b597-891ac1bb0a53.png#averageHue=%23f6f7f6&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=161&id=ucfd65540&margin=%5Bobject%20Object%5D&name=image.png&originHeight=201&originWidth=424&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29036&status=error&style=none&taskId=u8331941c-dc0a-4654-85fd-bc1961058d4&title=&width=339.2)

案例：
```html
  .nav{
        height: 41px;
        background-color: #fcfcfc;
        border-top:3px solid tomato ;
        line-height: 41px;
        border-bottom: 1px solid #edeef0;
      }
    
      .nav a{
        display: inline-block;
        height: 41px;
        font-size: 12px;
        /*文本的修饰线外观  */
        text-decoration: none;
        padding: 0 10px;
      }

     .nav  a:hover{
        background-color: turquoise;
        color: rgb(212, 104, 32);
      }
    </style> 
  </head>
  <body>
   
<div class="nav">
    <a href="1">1</a>
    <a href="2">1</a>
    <a href="3">1</a>
    <a href="4">1</a>
    <a href="5">1</a>
</div>
```
#### padding 不会撑开盒子的情况
如果盒子本身没有指定 width/height 属性，padding 不会撑开盒子大小

#### margin 可以让块级例子水平剧中
前提：

- 盒子必须指定了宽度 width
- 盒子左右的外边距设置为 auto
```html
  <style>
      
        .box1{
            width: 100px;
            height: 100px;
            /* 上下  左右  实现左右居中*/
            margin: 0 auto;
            background-color: tomato;
        }
     
    </style> 
  </head>
  <body>
  
    <div class="box1"></div>
  </body>
```
上面只适用用让块线元素水平居中，行内元素或行内块元素水平居中给其父元素添加 text-align:center 即可
```html
<style>
      
        .box1{
            width: 100px;
            height: 100px;
            /* 上下  左右  实现左右居中*/
            margin: 0 auto;
            background-color: tomato;
            text-align: center;
        }
     
    </style> 
  </head>
  <body>
  
    <div class="box1">
        <p>这是子元素</p>

    </div>
```
##### 外边距塌陷：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664590923936-3be49849-c57f-4f90-9089-dce242fc7d81.png#averageHue=%23f6f7f6&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=280&id=uc13cab06&margin=%5Bobject%20Object%5D&name=image.png&originHeight=350&originWidth=725&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81497&status=error&style=none&taskId=ubbd0dd81-dac6-4fe6-b6fa-30bdbd323b2&title=&width=580)
解决方法：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664590946354-c1c4db17-9416-41b4-8827-3f9d730f958f.png#averageHue=%23f2f3f2&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=132&id=u5d90c0ba&margin=%5Bobject%20Object%5D&name=image.png&originHeight=165&originWidth=286&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21735&status=error&style=none&taskId=u9fcd2284-9fdb-4a9a-b8b1-db74ec57d4e&title=&width=228.8)
```html
<style>

  .box1{
    width: 300px;
    height: 300px;
    background-color: aqua;
    margin-top: 50px;
    /* 解决 */
    /* border: 1px solid transparent; */
    /* padding: 1px; */
    overflow: hidden;
  }
  .box2 {
    width: 100px;
    height: 100px;
    background-color: tomato;
    padding-top: 100px;
  }
</style> 
</head>
<body>

  <div class="box1">
    <div class="box2"></div>
  </div>
```

#### 清除内外边距 （布局前）
```html
<!-- css 代码的第一行 -->
*{
margin: 0;
padding: 0;
}
      
```
行内元素为了照顾兼容性，尽量只设置左右外边距，不要设置上下内外边距。但是转换为块级和行内块元素就可以了

#### 圆角边框原理
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664591516837-93369860-0354-4069-bef4-e5c095b2cf0c.png#averageHue=%23f8f9f8&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=304&id=u819580eb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=380&originWidth=551&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71005&status=error&style=none&taskId=ued4a5dba-62ce-4b60-99ee-aaf328ec92e&title=&width=440.8)
```html

    <style>
        *{
            margin: 0;
            padding: 0;
        }
      
        div {
            width: 400px;
            height: 400px;
            background-color: tomato;
            /* border-radius: 50%; */
            /* 两个值就是对角线关系 */
            /* 四个值： 左上角  右上角  右下角  左下角 */
            /* border-radius: 10px 50px 100px 10px; */
            border-top-left-radius: 50%;
            margin: 0 auto;
        }
     
    </style> 
  </head>
  <body>
  
    <div></div>
```

#### 盒子阴影
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664591804663-15b7bdd1-1e85-4f91-9556-359ed4b5f6bc.png#averageHue=%23f9faf9&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=236&id=u27495a4c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=295&originWidth=600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=52512&status=error&style=none&taskId=ufc126105-db68-44d2-9bf6-67fb6e11408&title=&width=480)
默认是外阴影 
盒子阴影不占用空间，不会影响其他盒子排列

#### 传统网页布局三种方式

- 普通流（标准流）（标签默认的排列方式）
- 浮动（可以改变标签默认的排列方式） float
- 定位 

多个块级元素纵向排列用标准流，多个块级元素横向排列用浮动。

浮动：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664592480682-ab01f034-66ff-4528-b5a5-0c445430a574.png#averageHue=%23fafbfa&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=187&id=u65b77071&margin=%5Bobject%20Object%5D&name=image.png&originHeight=234&originWidth=632&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24764&status=error&style=none&taskId=ueb1ba7a3-f27b-4691-a366-f1b4a892b55&title=&width=505.6)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664592645016-d79b9fee-0ee4-4aff-8422-bc80aa1a7eb4.png#averageHue=%23729468&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=863&id=u8d99f56a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1079&originWidth=1902&originalType=binary&ratio=1&rotation=0&showTitle=false&size=157112&status=error&style=none&taskId=udbd46b07-da7a-40f5-8925-ae5353b4eba&title=&width=1521.6)
```html
* {
margin: 0;
padding: 0;
}

div {
width: 200px;
height: 200px;
background-color: tomato;
margin: 0 auto;
box-shadow: 10px 5px 5px 5px rgb(245, 120, 98);
}
.d1{
float: left;
}
.d2{
float: right;
}
</style>
</head>
<body>
  <div class="d1">1</div>
  <div class="d2">2</div>
</body>
```
浮动特性：

- 浮动元素会脱离标准流
- 浮动的元素会一行内显示并且元素顶部对齐
- 浮动的元素会具有行内块元素的特性

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664592779621-4313c0b8-1e5c-43a0-a48e-66ea0b0f5f16.png#averageHue=%23f1f2f1&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=270&id=u215ec6b6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=338&originWidth=482&originalType=binary&ratio=1&rotation=0&showTitle=false&size=98057&status=error&style=none&taskId=u578b94b2-3e7d-4c37-9f78-93331176a77&title=&width=385.6)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664592830918-502bb8de-e65b-4e6a-9556-07231f5dea06.png#averageHue=%23f4f5f4&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=220&id=u9c9c14aa&margin=%5Bobject%20Object%5D&name=image.png&originHeight=275&originWidth=549&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80126&status=error&style=none&taskId=uc93c784c-d1c6-4ef2-a5d5-4843681a914&title=&width=439.2)
不占用原有的位置
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664592839514-ba028ecb-cfef-4a10-b726-a234b127953f.png#averageHue=%23f2f3f2&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=218&id=u2286683a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=273&originWidth=467&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78422&status=error&style=none&taskId=u378d1f0a-577a-4923-9af1-0d502a5f6cb&title=&width=373.6)


:::danger
如果多个盒子都设置了浮动，它们会按照属性值一行内显示并且顶端对齐排列。
注意：浮动的元素是互相贴靠在一起的（不会有缝隙），父级宽度装不下这些浮动的盒子，多出的盒子会另起一行对齐
:::

浮动元素具有行内块元素特性：
`任何元素都可以浮动，不管原来是什么元素，添加浮动之后 具有行内块元素相似的特性`
行内元素是不可以直接指定高度和宽度属性的（p 是块级元素）
如果块级盒子没有设置宽度，默认和父级一样，添加浮动后，它的大小是根据内容来决定

#### 标签分类

- 行级元素（内联元素）
   - 宽高根据内容自动撑开，不可以设置宽高
      - a  b  span  img   input   select   strong   label 
- 块级元素
   - 宽度默认是 100% ，高度默认自动撑开，可以设置宽高
      - 常见块级元素： div h*  hr  menu    ol ul li dl dt dd table  p  form 
- 行内块元素
   - 结合了行内和块级的优点，可以对宽高属性赋值，可以多个标签存在一行显示
      - img input td  

先用标准流的父元素排列上下位置之后 内部子元素采用浮动排列左右位置。

例子：
```html
 <style>
      * {
        margin: 0;
        padding: 0;
      }

      .box {
        width: 1200px;
        height: 460px;
        background-color: aqua;
        margin: 0 auto;
      }
      .left {
        float: left;

        width: 230px;
        height: 460px;
        background-color: tomato;
      }

      .right {
        float: right;
        width: 970px;
        height: 460px;
        background-color: blueviolet;
      }
    </style>
  </head>
  <body>
    <div class="box">
      <div class="left">小</div>
      <div class="right">大</div>
    </div>
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664594405489-43c78d42-94ea-4720-9440-132a87ff8dcd.png#averageHue=%23e6e7e8&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=510&id=u35e437bc&margin=%5Bobject%20Object%5D&name=image.png&originHeight=637&originWidth=1627&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6281&status=error&style=none&taskId=u5962baca-b79d-4f38-be6b-4d0dcff5b9d&title=&width=1301.6)
例子2:
```html
    li {
        list-style: none;
        width: 296px;
        height: 285px;
        float: left;
        background-color: aqua;
        margin-right: 14px;

    }

    ul {
        width: 1226px;
        height: 285px;
        background-color: tomato;
        margin: 0 auto;
    }

    .last{
        margin-right: 0;
    }

    </style>
  </head>
  <body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li class="last">4</li>
    </ul>
  </body>
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664594931746-7e23ebdc-6087-4e03-8178-317b6ffde3a3.png#averageHue=%2300fefe&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=329&id=u79cb9472&margin=%5Bobject%20Object%5D&name=image.png&originHeight=411&originWidth=1577&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4876&status=error&style=none&taskId=u7c2f38b9-3475-4228-a6c4-c80d29c258b&title=&width=1261.6)


:::danger
只要是通栏的盒子（和浏览器一样宽）不需要指定宽度
:::

```html
 <style>
    :root{
        --mycolor:tomato;
    }

      * {
        margin: 0;
        padding: 0;
    }
  
  .top,.footer{
    height: 50px;
    background-color: var(--mycolor);
  }

  .banner{
    height: 300px;
    width: 900px;
    margin: 10px auto;
    background-color: var(--mycolor);
  }

  .box{
    width: 900px;
    height: 500px;
    background-color: var(--mycolor);
    margin: 20px auto;
  }


    </style>
  </head>
  <body>
    <div class="top">top</div>
    <div class="banner">banner</div>
    <div class="box">box</div>
    <div class="footer">footer</div>
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664595690847-d4acf146-3c51-4d67-ac40-3af8c25e4754.png#averageHue=%23fea392&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=734&id=u839afb4f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=918&originWidth=1917&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12756&status=error&style=none&taskId=ua7846584-52d3-445c-b258-b438050850e&title=&width=1533.6)


> 浮动例子只会影响浮动盒子后面的标准流不会影响前面的标准流（一个元素浮动了，理论上其余的兄弟元素也要浮动）


#### 清除浮动
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664597907057-6744e371-64e9-40bc-b81c-f476181d0dca.png#averageHue=%23d6f0e1&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=161&id=u60e8eea4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=201&originWidth=596&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47117&status=error&style=none&taskId=u020ba8a5-b391-4d7f-be01-d86501c1ab6&title=&width=476.8)

浮动元素不同占用原来的位置，它会对后面的元素排版产生影响。
满足的三个条件：

- 父级没高度
- 子盒子浮动了
- 影响下面布局了

清除浮动元素的本质：就是清除浮动元素造成的影响
清除浮动之后 ，父级就会根据浮动的子盒子自动检测高度，父级有了高度，就不会影响下面的标准流了

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664598110515-477a638e-0d90-4702-b163-145b5fdc7caa.png#averageHue=%23fafbfa&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=186&id=u190c92c3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=232&originWidth=657&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38777&status=error&style=none&taskId=u1190fe07-6e08-4b0f-b941-81810011ac4&title=&width=525.6)
策略：闭合策略
清除浮动方法：

- 额外标签法（隔墙法）
- 父级添加 overflow 属性
- 父级添加 alter 伪元素
- 父级添加双伪元素

 额外标签法：
就是放一个空标签占着位置
```html
  .box{
    width: 500px;
    background-color: var(--mycolor);
    margin: 0 auto;
    
  }

  .d1{
    float: left;
    width: 100px;
    height: 100px;
    background-color: purple;

  }

  .d2{
    float: left;
    width: 100px;
    height: 100px;
    background-color: rgb(230, 14, 230);

  }
  .box1{
    width: 900px;
    height: 100px;
    background-color: aqua;
  }

  .clear{
    clear: both;
    /* 清除浮动 */
  }

    </style>
  </head>
  <body>
    <div class="box">
        <div class="d1">1</div>
        <div class="d2">2</div>
        <!-- 清除浮动加个空标签 -->
        <!-- 这个新的空标签必须是块级元素 -->
     <div class="clear"></div>
    </div>
    <div class="box1"></div>
```
父级添加 overflow:
```html
  .box{
    width: 500px;
    background-color: var(--mycolor);
    margin: 0 auto;
    /* 清除浮动 */
    overflow: hidden;
  }

  .d1{
    float: left;
    width: 100px;
    height: 100px;
    background-color: purple;

  }

  .d2{
    float: left;
    width: 100px;
    height: 100px;
    background-color: rgb(230, 14, 230);

  }
  .box1{
    width: 900px;
    height: 100px;
    background-color: aqua;
  }

    </style>
  </head>
  <body>
    <div class="box">
        <div class="d1">1</div>
        <div class="d2">2</div>
    </div>
    <div class="box1"></div>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664599234915-ff3609f8-8c04-4f73-b7fe-8c3a3ead909f.png#averageHue=%238dc9d7&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=246&id=u7cb7a224&margin=%5Bobject%20Object%5D&name=image.png&originHeight=307&originWidth=968&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2744&status=error&style=none&taskId=ub25b58de-4df8-4872-8668-f922e4a50f3&title=&width=774.4)
无法显示溢出的部分

after伪元素：
```css
    .clearfix::after{
        content: "";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }
/* IE6,7专有 */
    .clearfix {
        *zoom: 1;
    }
```
使用：
```html
    <div class="box clearfix">
        <div class="d1">1</div>
        <div class="d2">2</div>
    </div>
    <div class="box1"></div>
```
是 额外标签法的升级版。给父元素添加

双伪元素（父元素添加）：
```css
    .clearfix::after,
    .clearfix::before{
        content: "";
        display: table;
    }
/* IE6,7专有 */
    .clearfix::after {
        clear: both;
    }
    .clearfix{
        *zoom: 1;
    }
```
使用和上一个一样的
  ![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664604323626-d4065c20-0f54-4670-8199-dc1cf6617c41.png#averageHue=%23f7f8f7&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=242&id=uc20bcf64&margin=%5Bobject%20Object%5D&name=image.png&originHeight=302&originWidth=605&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75406&status=error&style=none&taskId=u8aa181ae-63ec-417b-b2b6-0834cf04972&title=&width=484)
#### 定位 

- 定位模式： position 属性来设置
   - static   静态定位 （默认定位，无定位，按照标准流摆放位置）
   - relative    相对定位
   - absolute    绝对定位
   - fixed   	固定定位 

四个属性决定位置： top   bottom   left   right 
##### 相对定位 relative
元素在移动位置的时候，相对于它原来的位置进行定位的。
```css
posrtion: relative
```
特点：

- 移动是参照自己原来的位置进行移动的。
- 原来的位置继续占有，不脱标，位置还保留
```html
.d1{

width: 100px;
height: 100px;
background-color: purple;
position: relative;
top: 60px;
left: 50px;

}

.d2{
width: 100px;
height: 100px;
background-color: rgb(230, 14, 230);
}


</style>
</head>
<body>

  <div class="d1">1</div>
  <div class="d2">2</div>
```
##### 绝对定位 absolute
元素在移动位置时，是相对于它的祖先元素来说的。
特点：

- 如何没有祖先或父元素或祖先元素没有定位（需要给父亲加定位 的时候，什么定位都行，只要加上），以浏览器为准定位 （DOCUMENT 文档）
- 如果祖先元素有定位，以最近一级的有定位祖先元素为参考点移动位置
- 绝对定位不再占有原来的位置，脱标
```html
 .d1 {
        width: 500px;
        height: 500px;
        background-color: purple;
        margin-top: 50px;
        /* 把子元素的绝对定位，移动的参照物为父元素 */
        position: absolute;
      }

      .d2 {
        width: 100px;
        height: 100px;
        background-color: rgb(230, 14, 230);
      }

      .d1  div {
        width: 50px;
        height: 50px;
        background-color: tomato;
        position: absolute;
        bottom: 50px;
      }
    </style>
  </head>
  <body>
    <div class="d1"><div>son</div></div>
    <div class="d2">2</div>
```

#### 子绝父相
> 子级是绝对定位，父级要用相对定位 

父级需要占有位置，所以是相对定位，子盒子不需要占有位置，所以绝对定位。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664606534918-c090828f-e4ca-4750-a6bc-1edec0e6031f.png#averageHue=%2372ac7d&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=148&id=u450f3959&margin=%5Bobject%20Object%5D&name=image.png&originHeight=185&originWidth=217&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36467&status=error&style=none&taskId=u24fc910d-cd7d-48fe-ac84-657bd4f5df4&title=&width=173.6)
父级元素占位置，子元素可以在里面移动到任何位置
#### 固定定位 fixed
> 元素固定于浏览器可视区的位置。应用场景：浏览器页面滚动时元素的位置不会改变

- 以浏览器的可视窗口为参照点来定位的
- 跟父元素没有任何关系
- 不随滚动条在滚动而滚动（固定的）
- 固定定位不占有原先的位置（脱标）
```html
  .d1 {
        width: 500px;
        height: 500px;
        background-color: purple;
        margin: 0 auto;
        /* 把子元素的绝对定位，移动的参照物为父元素 */
        /* position: absolute; */
      }
      .d2{
        width: 40px;
        height: 100px;
        position: fixed;
        left: 50%;
        margin-left: 250px;
        top: 0;
        background-color: tomato;

      }
    </style>
  </head>
  <body>
  <div class="d1"></div>
  <div class="d2"></div>
```
##### 粘性定位  sticky (粘性的，湿热的)
> 是相对定位和固定定位的混合

特点：

- 以浏览器的可视窗口为参照点移动元素（固定定位特点）
- 它占有原先的位置（相对定位特点）

跟页面滚动搭配使用，兼容性较差，IE不支持
```html
 body{
        height: 3000px;
      }

      .nav{
        width: 400px;
        height: 100px;
        background-color: tomato;
        margin: 100px auto;
        position: sticky;
<!-- 当元素到达可视区顶部距离为 0 时，就固定在那里了不动了 -->
        top: 0px;
      }

    </style>
  </head>
  <body>
    <div class="nav">导航栏</div>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664608990246-da581013-8e99-4bfa-98d7-d2162eadebe6.png#averageHue=%23c1cde6&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=236&id=ueeb147e0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=295&originWidth=592&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91850&status=error&style=none&taskId=uf5a45ede-2d04-47ee-9c0c-363e7159ac5&title=&width=473.6)
##### 定位的叠放次序  z-index
```html
     .box{
        width: 200px;
        height: 200px;
        position: absolute;
      }
     
      .d1{
        background-color: tomato;
        /* 值越大就越靠上 */
        z-index: 10;
        top: 3px;
        left: 5px;
      }
      .d2{
        background-color: darkcyan;
        z-index: -1;
      }
     
    </style>
  </head>
  <body>
    <div class="d1 box">d1</div>
    <div class="d2 box">d2</div>
```

- 数值可以是正整数，负数，0，默认是 auto ，数值越大，盒子越靠上
- 如果属性相同，按照书写顺序，后来者居上
- 只有定位的盒子才有 z-index 属性（普通的标准流没有）

##### 绝对定位盒子的居中方法
加了绝对定位的盒子不能通过 margin: 0 auto 水平居中。
一个元素的属性里面写了相同的多个，那么也会遵循后面的会覆盖掉前面的。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664609852590-1f4bca67-9ae4-41f2-a409-94ba71180082.png#averageHue=%23f6f4ec&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=327&id=udd4e29db&margin=%5Bobject%20Object%5D&name=image.png&originHeight=409&originWidth=631&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22702&status=error&style=none&taskId=ud8d0ac65-e9fb-4b16-98e2-af68c8a4e4a&title=&width=504.8)
```html
  .box{
        width: 200px;
        height: 200px;
        position: absolute;
        background-color: tomato;
        /* 走到中间 */
        left: 50%;
        /* 再向左走 100px  达到绝对*/
        margin-left: -100px;
        /* 加了绝对定位的盒子，不能使用这个来居中定位 */
        /* margin: 0 auto; */
        /* 垂直居中 */
        top: 50%;
        margin-top: -100px;
      }
    
    </style>
  </head>
  <body>
    <div class="box">box</div>
```
##### 定位特殊性
绝对/相对 也和浮动类似

- 行内元素添加绝对或者固定定位，可以直接设置高度和宽度
- 块级元素添加绝对或者固定定位，如果不给宽度或者高度，默认大小是内容的大小

脱标的盒子不会触发外边距塌陷。

 `绝对定位（固定定位）会完全压住盒子`
浮动元素不同：它不会压住下面标准流盒子里面的文字，只会压住下面标准流的盒子
绝对定位（固定定位）会完全压住
> 浮动之所以不会压住文字，它产生的目的最初是为了做文字环绕效果的，文字会围绕浮动元素

```html
 div{
        float: left;
        width: 300px;
        height: 100px;
        background-color: tomato;
      }
    </style>
  </head>
  <body>
    <div>这是文字</div>
    <p>这是 p 标签</p>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664610423297-e56454ae-f314-42aa-b4a2-056c89596d7f.png#averageHue=%23fdbbaf&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=146&id=u489aff34&margin=%5Bobject%20Object%5D&name=image.png&originHeight=182&originWidth=651&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10811&status=error&style=none&taskId=u9be77d05-1312-4961-9982-1fef227eb02&title=&width=520.8)
```html
      .box{
        width: 500px;
        height: 500px;
        background-color: var(--mycolor);
        /* position: absolute;
        left: 50%;
        margin-left: -250px;
        top: 10%; */
        margin: 10% auto;
        position: relative;
      }

      .d1{
        width: 30px;
        height: 30px;
        background-color: rgb(124, 124, 144);
        position: absolute;
        top: 50%;
        margin-top: -25px;
        border-radius: 10px;
        text-align: center;
        line-height: 25px;
      }

      .d2{
        width: 30px;
        height: 30px;
        background-color: rgb(100, 100, 123);
        position:absolute;
        right: 0;
        top:50%;
        margin-top: -25px;
        border-radius: 10px;
        text-align: center;
        text-decoration: none;
        line-height: 25px;
      }

      .d3{
        width: 100px;
        height: 30px;
        text-align: center;
        background-color: green;
        position: absolute;
        bottom: 0;
        left: 50%;
        margin-left: -50px;
      }

 
    </style>
  </head>
  <body>
    <div class="box">
        <!-- 两个小箭头 -->
        <div class="d1"><span>></span></div>
        <div class="d2"><span><</span></div>
        <!-- 圆点框 -->
        <div class="d3"><span>......</span></div>
    </div>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664611547384-2648b3f3-cf51-47aa-b429-0b36179cb090.png#averageHue=%23fe9683&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=589&id=u8e3a129d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=736&originWidth=786&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5596&status=error&style=none&taskId=ud2c6c0ed-a822-43c8-a257-baff8fcde3a&title=&width=628.8)
总结 ：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664611646062-c5b4a4dd-0db7-4e7e-80d4-78d2612c0840.png#averageHue=%23f8f9f8&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=179&id=uebb66285&margin=%5Bobject%20Object%5D&name=image.png&originHeight=224&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59351&status=error&style=none&taskId=u0247f3ea-779e-45a2-b4f1-78ebbd89f51&title=&width=512)
##### display 

- none 隐藏对象（隐藏元素后，不同占有原来的位置）
- block 除了转换为块级元素外，同时还有显示元素的意思
- inline-block 转换为行内块元素

##### visibility 可见性
指定一个元素应可见还是隐藏
visible 元素可见
hidden 元素隐藏，隐藏后继续占有原来的位置

##### overflow 溢出隐藏

- 默认 visible 可见
- hidden 隐藏
- scroll 溢出的部分显示滚动条，不溢出也显示滚动条
- auto 在需要的时候添加滚动条，不溢出就不显示滚动条

#### 精灵技术
> 用于背景图片使用，把多个小背景图片整合到一张大图片中，服务器请求时，可以减少请求次数

- 主要针对于小的背景图片使用。
- 借助于背景的位置来实现- background-position
- 一般情况下精灵图是负值， X 轴右边走是正值，y轴往下边走是正值

多个小背景图片组成一张大小，通过改变图片的位置来显示不同的图标
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664612633947-5c7772d9-61c3-4488-9906-ce7bfebc7bd2.png#averageHue=%23f3f4f3&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=222&id=u3f77babd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=277&originWidth=266&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36924&status=error&style=none&taskId=uac7a3239-6a7f-4787-bd14-28a986bd512&title=&width=212.8)
```html
  .box{
        width: 200px;
        height: 200px;
        margin: 10% auto;
        /* 一般都是负的 */
        /* background-position: X轴 y轴; */
        background: url('../images/infinity-2810593.jpg') no-repeat -180px -700px;
      }
    </style>
  </head>
  <body>
    <div class="box">
    </div>
```

##### 字体图标使用
下载：

##### 三角形制作
```html

      .box{
        width: 0;
        height: 0;
        border-top: 100px solid pink ;
        border-bottom:100px solid tomato;
        border-left:  100px solid rgb(15, 132, 234) ;
        border-right:  100px solid sandybrown;
      }

      /* 三角形制作 */
      .box1{
        width: 0;
        height: 0;
        border: 100px solid transparent;
        border-bottom: 100px solid tomato;
        margin-top: 100px;
      }

      
    </style>
  </head>
  <body>

    <div class="box"></div>
    <div class="box1"></div>

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664614820282-5f13208a-bf8c-4116-a761-86f429d0b29e.png#averageHue=%23fde7e0&clientId=u7b58f5b4-1d9c-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=601&id=u250fa54a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=751&originWidth=420&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5805&status=error&style=none&taskId=ue0afa75e-5f93-48e2-8b09-3c71861ef5a&title=&width=336)


##### 用户界面--鼠标样式
```html
 <ul>
    <li style="cursor: pointer;">11111</li>
    <li style="cursor: move;">22222</li>
    <li style="cursor: text;" >33333</li>
    <li style="cursor: not-allowed;" >44444</li>
    <li style="cursor: default;" >55555</li>
   </ul>
```
移动到不同的位置显示不同的效果

```html
   input{
        /* 取消表单轮廓 */
        outline: none;
      }
      textarea{
        /* 防止文本域被拖拽 */
        resize: none;
      }
    </style>
  </head>
  <body>
   <input type="text">
   <textarea name="syhk" id="" cols="30" rows="10"></textarea>
```

#### BOM 和 DOM
DOM 文档对象模型：处理网页内容的方法和接口  核对对象是 documet
BOM 浏览器对象模型：与浏览器交互的方法和接口 核心对象是 window

##### 让图片（行内元素或行内块元素）和文字垂直居中
```html
  <style>
    img {
      /* vertical-align: bottom;
    vertical-align: top; */
      /* 让图片和文字垂直居中 */
      vertical-align: middle;
    }
  </style>
  <body>
    <img src="../../images/1.png" alt="" />这是代码
  </body>
```
如果是块级元素可以使用 ` display: inline-block;`转换为行内块元素。
默认是基线对齐。有四条线：底线，基线，中线，顶线
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664692770166-87030b3e-c215-4d6d-9021-be035676ca29.png#averageHue=%23f9faf9&clientId=uf1835ff2-b002-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=309&id=u4ceee982&margin=%5Bobject%20Object%5D&name=image.png&originHeight=386&originWidth=669&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65702&status=error&style=none&taskId=u3aeb4f29-a53d-4f1e-80c9-8cfb8918287&title=&width=535.2)

##### 图片底侧空白间隙解决
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664692998787-4d4b94f5-7270-4e4b-8767-01ae82bcd676.png#averageHue=%23797e7b&clientId=uf1835ff2-b002-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=306&id=u17500810&margin=%5Bobject%20Object%5D&name=image.png&originHeight=383&originWidth=953&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15413&status=error&style=none&taskId=ued9515ed-789a-47cc-9e78-36f2a70e371&title=&width=762.4)

- 给图片添加  `vertical-align:middle /  top / bottom` 等 （提倡）

- 把图片转换为块级元素 `display:block`

##### 溢出的文字用省略号表示 

- 单行文本溢出：
```html
  <style>
    div {
      width: 100px;
      height: 80px;
      background-color: tomato;
      /* normal 是如果文字显示不开自动换行 */
      /* white-space: normal; */
      /* 必须在一行内显示 */
      white-space: nowrap;
      /* 超出的部分隐藏 */
      overflow: hidden;
      /* 用省略号替代超出的部分 */
      text-overflow: ellipsis;
    }
  </style>
  <body>
    <div>这里有好多字，还有好多字</div>
```

- 多行文本溢出
```html
   div {
      width: 100px;
      height: 80px;
      background-color: tomato;
      /* 兼容性不太行 */
      display: -webkit-box;
      /* 限制一个块内显示的行数，也就相当于在第几行显示省略号 */
      -webkit-line-clamp: 2;

      -webkit-box-orient: vertical;
    }
  </style>
  <body>
    <div>这里有好多字，还有好多字</div>
```

##### 常见布局技巧

- margin 负值运用
```html
  <style>
    ul li {
      width: 150px;
      height: 200px;
      border: 1px solid tomato;
      list-style: none;
      float: left;
      margin-left: -1px;
    }
  </style>
  <body>
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
    </ul>
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664694409419-3657a5af-14cd-4f4e-8d7b-bdd4832c37b8.png#averageHue=%23fefbfb&clientId=uf1835ff2-b002-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=318&id=ub81e2dc6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=397&originWidth=1015&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13823&status=error&style=none&taskId=u0ad643e1-9504-4a94-8709-73b64022178&title=&width=812)
```html
    ul li {
      position: relative;
      float: left;
      width: 150px;
      height: 200px;
      border: 1px solid tomato;
      list-style: none;
      margin-left: -1px;
    }
    /* 如果没有定位可以使用相对定位，保留位置 */
    li:hover {
      /* 如果 li 有定位 ，可以使用这个提高层级，压住其他的 */
      z-index: 1;
      border-color: aqua;
    }
  </style>
  <body>
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
    </ul>
  </body>
```

##### 文字围绕浮动元素
```html
 <style>
    /* 文字围绕浮动元素 */
    div {
      width: 300px;
      height: 100px;
      background-color: tomato;
    }
    div img {
      float: left;
      width: 100px;
      height: 100px;
      margin-right: 5px;
    }
  </style>
  <body>
    <div>
      <img src="../../images/1.png" alt="" />
      <p>这里以是好多文字，这里以是好多文字，这里以，这里以是好多文字，</p>
    </div>
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664695186870-b88d0c8e-5376-451d-9b2d-ea66811505e3.png#averageHue=%23f8d1ca&clientId=uf1835ff2-b002-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=190&id=u83fdcdd1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=238&originWidth=545&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21217&status=error&style=none&taskId=ua624c2b2-9f37-4295-b64d-0fb54c495f7&title=&width=436)

##### 三角利用
```html
  .box {
      width: 0;
      height: 0;
      border-color: transparent red transparent transparent;

      border-style: solid;

      border-width: 100px 50px 0 0;
    }

    .price {
      width: 160px;
      height: 24px;
      line-height: 24px;
      border: 1px solid red;
      margin: 10% auto;
    }

    .miaosha {
      float: left;
      /* 子绝父相 */
      position: relative;
      width: 90px;
      height: 100%;
      background-color: red;
      text-align: center;
      color: #fff;
      font-weight: 700;
    }

    .miaosha i {
      position: absolute;
      width: 0;
      right: 0;
      top: 0;
      height: 0;
      border-color: transparent rgb(255, 255, 255) transparent transparent;
      border-style: solid;
      border-width: 24px 10px 0 0;
    }

    .origin {
      font-size: 12px;
      color: gray;
      text-decoration: line-through;
    }
  </style>
  <body>
    <div class="box"></div>
    <div class="price">
      <span class="miaosha">1650<i></i></span>
      <span class="origin">5600</span>
    </div>
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664697658220-1d9302a4-d9a2-4e93-a1b3-df0c6660f356.png#averageHue=%23fef8f8&clientId=uf1835ff2-b002-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=317&id=u11ab2de7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=396&originWidth=754&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5705&status=error&style=none&taskId=u28d4a40e-37ae-4960-b429-7b146edf5ec&title=&width=603.2)


##### css 初始化

```html

*{
    padding: 0;
    /* 清除内边距*/
    margin: 0;
    /*清除外边距*/
}

li {
    list-style: none;
}

a {
    text-decoration: none;
    font-size: 12px;
    color: gray;
    font-weight: 600;
}

/* 双伪元素清除浮动 */
.clearfix:before,
.clearfix:after {
    content: "";
    display: table;
}

.clearfix:after {
    clear: both;
}

.clearfix {
    *zoom: 1;
}
<!-- 斜体的文字不倾斜 -->
em,
i {
    font-style: normal
}

img {
    border: 0;
<!-- 取消图片底侧有空白缝隙问题 -->
    vertical-align: middle
}

button {
<!-- 当鼠标经过按钮时变成 小手样子 -->
    cursor: pointer
}

button,
input {
    font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif
}


/* 全局定义 */
body {
    /* 文字抗锯齿性 */
    -webkit-font-smoothing: antialiased;
    font: 12px/1.5 'Source Code Pro',Microsoft YaHei,
        Heiti SC,
        tahoma,
        arial,
        Hiragino Sans GB,
        "\5B8B\4F53",
        sans-serif;
    background-color: rgb(240,240,240）；
} 

```

##### css3 盒子模型
css3中通过 `box-sizeing`来指定盒子模型，两个值：

- content-box :盒子大小为： width+padding+border （以前默认的）
- border-box ：盒子大小为： width

盒子模型改为了 border-box ，那么 padding 和 border 就不会撑大盒子（前提是 padding 和 border 不会超过 width 宽度）
```html
  div {
      width: 200px;
      height: 200px;
      background-color: red;
      padding: 15px;
      border: 15px solid rebeccapurple;
      box-sizing: content-box;
      /* 这是默认是的 200+15+15 */
    }

    p {
      width: 200px;
      height: 200px;
      background-color: red;
      padding: 15px;
      border: 15px solid tomato;
      /* css3 盒子模型的最终大小：就是 width 200 大小 */
      box-sizing: border-box;
    }
  </style>
  <body>
    <div>小猪天下</div>
    <p>这是什么</p>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664699450635-25311de3-a666-42ce-a5d1-5ab04aaaf37c.png#averageHue=%23feaead&clientId=u9cc21a49-d07e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=530&id=u7b090752&margin=%5Bobject%20Object%5D&name=image.png&originHeight=663&originWidth=701&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6786&status=error&style=none&taskId=u62719e61-920e-47ad-b265-8c3190a7c2e&title=&width=560.8)
```html
*{
	padding:0;
  margin:0;
   box-sizeing:border-box;
<!-- 这样可以设置所有的模型 -->
}
```


#### css 滤镜 filter
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664699558401-5d2e3b9e-f3b4-4dd4-bfa0-d90aa143a772.png#averageHue=%23fbfdfc&clientId=u9cc21a49-d07e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=64&id=u590bf8e3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=80&originWidth=684&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15541&status=error&style=none&taskId=u642ac26b-4e8d-4df9-a0c3-5e7055649ec&title=&width=547.2)
```html
 <style>
    img {
      /* 数值要加 px 单位 */
      filter: blur(5px);
    }
  </style>
  <body>
    <img src="../../images/1.png" alt="" />
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664699718507-df3c5aad-8daa-407f-b07c-87b46b7ec7d6.png#averageHue=%23e5e6e7&clientId=u9cc21a49-d07e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=292&id=ue2464d0a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=365&originWidth=810&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12024&status=error&style=none&taskId=u2d396095-289b-4e29-a297-8d756e702ef&title=&width=648)

##### calc 函数
```html
  <style>
    .father {
      width: 300px;
      height: 200px;
      background-color: pink;
    }
    .son {
      /* 注意两个计算数值之间需要有空格，不然会失效 - + * /  四个运算符 */
      width: calc(100% - 150px);
      height: 30px;
      background-color: tomato;
    }
  </style>
  <body>
    <div class="father">
      <div class="son"></div>
    </div>
```
宽度和高度可以动态变化。

##### CSS3 过渡
经常和 :hover 搭配使用
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664700177328-62945f5d-ec8c-4b4d-8ce9-9c58cd2a08fb.png#averageHue=%23f0f1f0&clientId=u9cc21a49-d07e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=186&id=udf0203f3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=232&originWidth=652&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80119&status=error&style=none&taskId=u297a260e-00c6-4fea-8e4a-7801e1ca325&title=&width=521.6)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664700457816-653404a7-47ae-4e92-8d21-60245346bd73.png#averageHue=%23fafbfa&clientId=u9cc21a49-d07e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=219&id=u91241454&margin=%5Bobject%20Object%5D&name=image.png&originHeight=274&originWidth=867&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80054&status=error&style=none&taskId=u4a8d8918-5dcc-4fd1-9729-9960e8fb47f&title=&width=693.6)
谁做过渡给谁加

```html
    .father {
      width: 300px;
      height: 200px;
      background-color: pink;
      transition: all 1s ease 0s;
    }
    .father:hover {
      width: 400px;
      height: 100px;
      background-color: tomato;
    }
  </style>
  <body>
    <div class="father"></div>
  </body>
```
```html
 transition: width 1s ease 0s, height 1s;
```
多个属性用`逗号`分隔。

##### 进度条案例
```html
  .father {
      width: 150px;
      height: 15px;
      border: 1px solid red;
      border-radius: 7px;
    }
    .son {
      width: 50%;
      height: 100%;
      background-color: red;
      /* 谁做过渡给谁加 */
      transition: width 1s;
    }

    .father:hover .son {
      width: 100%;
    }
  </style>
  <body>
    <div class="father">
      <div class="son"></div>
    </div>
  </body>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664700890730-bedbcae8-d24f-45c1-9bf1-012fb38413a9.png#averageHue=%23fef7f7&clientId=u9cc21a49-d07e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=137&id=u0ebda9a2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=171&originWidth=439&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1722&status=error&style=none&taskId=ud6e9fe70-f58f-4ef2-88cf-5568bcb5ca0&title=&width=351.2)



##### 网站部署免费空间
##### 文本缩进
```css
text-indent:20px;
```
指定文本的第一行的缩进。

##### 背景
大图片插入完整
```css
background-position: center top;
```
背景平铺：
```css
background-repeat: repeat | no-repeat | repeat-x  | repeat-y
```

- repeat 背景图像在纵向和横向上平铺 默认
- no-repeat  背景图像不平铺
- repeat-x 在横向上平铺
- repeat-y  在纵向上平铺

背景位置
```html
div {
width: 900px;
height: 900px;
background-image: url("../../images/1.png");
background-repeat: no-repeat;
/* background-position: center top; */
/* 页面元素既可以添加背景图片也可以添加背景颜色，背景图片会压住背景颜色  */
}
</style>
<body>
  <div></div>
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664761406836-438e1075-3d6b-4cd6-b554-e326eb6b8322.png#averageHue=%23f8f9f8&clientId=u49e63ace-e18b-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=194&id=u3f06142c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=243&originWidth=668&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48518&status=error&style=none&taskId=uafb9b98e-85dc-4f5c-b7d6-4604f1e3bff&title=&width=534.4)
居中对齐
```html
  div {
      width: 900px;
      height: 900px;
      background-image: url("../../images/1.png");
      background-repeat: no-repeat;
      /* 居中对齐 */
<!--       background-position: center; -->
      background-position: center center;
      /* 页面元素既可以添加背景图片也可以添加背景颜色，背景图片会压住背景颜色  */
    }
  </style>
  <body>
    <div></div>
```
方位参数第二个参数省略，默认是居中的。
方位参数的顺序参数不影响 ： top left  / left top 是一样的
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664762399638-a4862897-ee0f-4ef0-bef8-39f35f373477.png#averageHue=%23f3f4f3&clientId=uc3dcabf1-05aa-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=245&id=ud4801502&margin=%5Bobject%20Object%5D&name=image.png&originHeight=306&originWidth=672&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70710&status=error&style=none&taskId=u8f719530-db63-41b0-b5e8-384c4e875f9&title=&width=537.6)

固定背景图片位置：
```html
   /*把背景图片固定住 */
      background-attachment: fixed;
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664762849610-26850eb5-61c9-4301-a988-b52d4e8f683d.png#averageHue=%23fafbfa&clientId=uc3dcabf1-05aa-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=154&id=uc068987a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=192&originWidth=691&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19137&status=error&style=none&taskId=u71037f7e-72be-41c0-9326-937ddf03520&title=&width=552.8)


##### 查看一个网站的网站图标
eg:
[https://www.bilibili.com/favicon.ico](https://www.bilibili.com/favicon.ico)
在网站后面加了 `favicon.ico` 访问就可以看到了
网站的图标是存放在项目的根目录下，然后在 html 中引入 
```html
<link rel="shortcut icon" href="/favicon.ico"/>
```

#### SEO 搜索引擎优化
目的是对网站进行深度的优化，从而帮助网站获取免费流量，在搜索引擎上提升网站的排名，提高知名度。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664849017201-92ac8f6f-6d29-4439-96f7-bbe228d59d51.png#averageHue=%23f9fbf9&clientId=ubd07e526-169a-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=190&id=ua71a70b2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=237&originWidth=708&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27153&status=error&style=none&taskId=u4b55cc33-093f-4247-8ae6-78c983f544d&title=&width=566.4)

- title 网站标题
- description 网站说明
- keywords 页面关键字 （6-8）个
```html
<meta name="description" content="这是网站的说明">
<meta name="keywords" content="这里这网站的搜索关键字">
<title>大图片插入完整</title>
```



##### 2D 转换
transform 可以实现元素的位移，旋转，缩放等效果

- 移动 translate
- 旋转 rotate
- 缩放 scale

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664850375641-4b48ca6e-dc29-43e1-9558-c29bcc548bde.png#averageHue=%23e8ecef&clientId=ubd07e526-169a-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=104&id=ua9df9292&margin=%5Bobject%20Object%5D&name=image.png&originHeight=130&originWidth=597&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17209&status=error&style=none&taskId=u102f771e-7e28-4e6c-9ef4-ea72ee9d7d4&title=&width=477.6)

```html
/* 盒子的位置： 定位  盒子的外边距  2d */
div {
width: 200px;
height: 200px;
background-color: tomato;
/* x,y   要带有单位 */
/* transform: translate(100px, 200px); */
/* 只移动 x 轴/y 轴 */
/* transform: translateX(100px); */
transform: translate(200px, 0);
/* transform: translateY(200px); */
}
</style>
<body>
  <div></div>
```
优点： 不会影响到其他元素的位置
```html
/* 盒子的位置： 定位  盒子的外边距  2d */
div {
width: 200px;
height: 200px;
background-color: tomato;
/* x,y   要带有单位 */
/* transform: translate(100px, 200px); */
/* 只移动 x 轴/y 轴 */
/* transform: translateX(100px); */
/* transform: translate(200px, 0); */
/* transform: translateY(200px); */
}
div:first-child {
background-color: tomato;
transform: translate(30px, 30px);
}
div:last-child {
background-color: aqua;
}
</style>
<body>
  <div></div>
  <div></div>
```


translate 中的百分比单位是相对于自身元素的
让一个盒子水平垂直居中
```html
/* 盒子的位置： 定位  盒子的外边距  2d */
div {
width: 500px;
height: 500px;
background-color: tomato;
/* translate 里面的参数可以用百分号 */
/* 按 % 移动的距离是盒子自身的宽度或者高度来为参照的 */
/* 这是就是移动 100px */
/* transform: translateX(50%); */
position: relative;
}
p {
position: absolute;
top: 50%;
left: 50%;
width: 200px;
height: 200px;
background-color: aquamarine;
transform: translate(-50%, -50%);
}
</style>
<body>
  <div>
    <p></p>
  </div>
```

对于行内标签没有效果。
##### 旋转

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664851681717-c5d4058a-78e2-4b27-91a4-0de40b303812.png#averageHue=%23f7f8f7&clientId=ubd07e526-169a-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=307&id=u15bb2a58&margin=%5Bobject%20Object%5D&name=image.png&originHeight=384&originWidth=801&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74441&status=error&style=none&taskId=ucd99d88f-9b42-4736-b610-a46f58542aa&title=&width=640.8)
默认旋转中心是元素的中心
```html
<style>
  /* 盒子的位置： 定位  盒子的外边距  2d */
  div {
    width: 500px;
    height: 500px;
    background-color: tomato;
    margin: 20% auto;
    /* transform: rotate(100deg); */
    transition: all 1s;
  }
  div:hover {
    transform: rotate(120deg);
  }
</style>
<body>
  <div></div>
```
```html
/* 给 div 添加内容 */
/* div::before div::after 可以将内容插入到页面中，不需要在 HTML 中 */

div:after {
content: "";
position: absolute;
top: 3px;
right: -1px;
width: 10px;
height: 10px;
border-right: 5px solid tomato;
border-bottom: 5px solid tomato;
transform: rotate(135deg);
transition: all 1s;
}
```
##### 设置转换中心
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664852946601-9676e500-77ae-4938-a88e-274904091748.png#averageHue=%23f6f7f6&clientId=ubd07e526-169a-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=274&id=uba974315&margin=%5Bobject%20Object%5D&name=image.png&originHeight=342&originWidth=614&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54377&status=error&style=none&taskId=u9d7cacf9-f39f-4117-82a0-9c7137c769f&title=&width=491.2)
```html
    /* 盒子的位置： 定位  盒子的外边距  2d */
    div {
      width: 100px;
      height: 100px;
      background-color: tomato;
      margin: 20% auto;
      /* transform: rotate(50deg);
      border-top: transparent;
      border-right: transparent; */
      position: relative;
      /* 谁做动画/过渡就给谁加 */
      transition: all 1s;
    }
    div:hover {
      transform: rotate(360deg);
      /* 默认是 50%  50% 等价于 center center */
      /* transform-origin: left bottom; */
      transform-origin: 50px 50px;
    }
  </style>
  <body>
    <div></div>
```

##### 2d 转换 scale 缩放
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664853459611-4b75cbb6-3e08-47b8-9ba1-5efa1760c204.png#averageHue=%23f4f5f4&clientId=ubd07e526-169a-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=222&id=u63ae30b8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=277&originWidth=663&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57046&status=error&style=none&taskId=uecf8dd3b-3075-4f88-bad6-36d6ecd38e5&title=&width=530.4)
```html
/* 盒子的位置： 定位  盒子的外边距  2d */
div {
width: 100px;
height: 100px;
background-color: tomato;
margin: 20% auto;
position: relative;
/* 谁做动画/过渡就给谁加 */
transition: all 1s;
}
div:hover {
/* 里面写数字不跟单位，是倍数是意思  */
/* 宽度为原来的 2 倍， 高度不变 */
/* transform: scale(2, 1); */
/* 等比例缩放  一个值，高宽同时蛮*/
/* transform: scale(2); */

/* 缩小 */
/* transform: scale(0.5, 0.5); */
/* transform: scale(0.5); */

/* 优势
不会影响其他的盒子，而可以设置缩放的中心点
*/
transform: scale(2);
transform-origin: right top;
}
</style>
<body>
  <div></div>
```
案例：
```html
<style>
  li {
    height: 30px;
    width: 30px;
    float: left;
    margin: 10px;
    border: 1px solid tomato;
    text-align: center;
    line-height: 30px;
    border-radius: 50%;
    list-style: none;
    cursor: pointer;
    transition: all 0.4s;
  }

  li:hover {
    transform: scale(1.5);
  }
</style>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
  </ul>
</body>
```
###### 2d 转换综合写法
```html
    div {
      width: 100px;
      height: 100px;
      background-color: tomato;
      transition: all 1s;
    }

    div:hover {
      transform: translate(50px, 50px) rotate(180deg) scale(2);
      /* 代码顺序：同时有位移和其他属性，需要把位移放到最前面 */
    }
  </style>
  <body>
    <div></div>
```

#### 动画
```html
/* 动画使用： 定义  使用 */
div {
width: 200px;
height: 200px;
background-color: tomato;
/* 动画名称  时长  */
animation: move 3s infinite;
/* animation-name: move;
animation-duration: 2s; */
}

/* 定义动画 */
/*动画序列： 0% 开始 100% 完成 等价于 from  to
可以写任意多的次数
  /* 百分比要是整数
     百分比是总的时间的划分，就是上面定义的时间长度
   */
*/
@keyframes move {
0% {
transform: translate(0px, 0px);
}
25% {
transform: translate(200px, 0);
}
50% {
transform: translate(200px, 200px);
}
75% {
transform: translate(0, 200px);
}
100% {
transform: translate(0px, 0px);
}
}
</style>
</head>

<body>
  <div></div>
</body>
```
##### 动画属性
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664856213217-5ac96e16-d083-48e0-96c7-0673bd2b67d8.png#averageHue=%23bad5ef&clientId=ubd07e526-169a-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=332&id=u30230e52&margin=%5Bobject%20Object%5D&name=image.png&originHeight=415&originWidth=700&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107425&status=error&style=none&taskId=u761ca6f8-03b5-424e-b12e-c5fb1551fcf&title=&width=560)
```html
      /* 动画使用： 定义  使用 */
      div {
        width: 200px;
        height: 200px;
        background-color: tomato;
        /* 动画名称  时长  */
        animation: move 3s;
        /* animation-name: move;
        animation-duration: 2s; 以上这两个属性是必须要写的*/
        /* 常见属性 */

        /* 运动曲线 */
        animation-timing-function: ease;
        /* 何时开始 */
        animation-delay: 1s;
        /* 重复次数 iteration 重复的总次数  infinite 无限循环 */
        /* animation-iteration-count: infinite; */

        /* 是否反方向播放 */
        animation-direction: alternate;

        /* 结束后的状态，默认的是 backwards 回到起始状态  forwards 停留在结束状态*/
        animation-fill-mode: forwards;

        /* 规定动画是否运行或暂停 默认 running ； 另一个参数 paused 暂停 */
        animation-play-state: running;
      }

      /* 定义动画 */
      /*动画序列： 0% 开始 100% 完成 等价于 from  to
      可以写任意多的次数
      */
      @keyframes move {
        /* 百分比要是整数
          百分比是总的时间的划分
          */
        0% {
          transform: translate(0px, 0px);
        }
        25% {
          transform: translate(200px, 0);
        }
        50% {
          transform: translate(200px, 200px);
        }
        75% {
          transform: translate(0, 200px);
        }
        100% {
          transform: translate(0px, 0px);
        }
      }
    </style>
  </head>

  <body>
    <div></div>
```
```html
/* 简写 */
/* 参数： 名称 时间  运动曲线 何时开始  播放次数 是否反方向 动画起始或结束的状态 */
animation: move 2s linear 2s infinite alternate;
animation:name duration timing-function delay  iteration-count  direction fill-mode;
    
```

##### 速度曲线细节
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664933284993-373a2cf5-5344-4b5b-a5ac-6eba8a0da7c0.png#averageHue=%23b6d3ef&clientId=u6e1aa0ba-39f1-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=302&id=u616af6b4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=377&originWidth=714&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57961&status=error&style=none&taskId=u2bc2c59c-bbea-441e-bb40-1340c57e5b8&title=&width=571.2)
```html
<style>
  /* 打字机效果 */
  div {
    overflow: hidden;
    font-size: 20;
    width: 0;
    height: 30px;
    background-color: tomato;
    /* 文字强制一行显示 */
    white-space: nowrap;
    /* steps 就是分几步来完成动画，有了它就不要写 ease 或者 linear */
    animation: w 4s steps(10) forwards;
  }

  @keyframes w {
    0% {
      width: 0;
    }
    100% {
      width: 200px;
    }
  }
</style>
</head>

<body>
  <div>这是什么这是什么沈沈</div>
```

##### 3d 转换

- 3d 移动 translate3d

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664933887755-7c4ef768-b57e-4a5e-9148-34ecbd900e1c.png#averageHue=%23f1f2f1&clientId=u6e1aa0ba-39f1-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=149&id=ue145741b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=186&originWidth=608&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49412&status=error&style=none&taskId=uda1fe7ff-a99d-4bcd-a7c2-8499c4512b7&title=&width=486.4)
```html
div {
width: 200px;
height: 200px;
margin: 10% auto;
background-color: tomato;
/* transform: translateX(100px) translateY(100px) translateZ(100px); */
/* translateZ 沿着 z 轴移动 */
/* 后面的单位一般跟 px */
/* translateZ(100px) 向外移动 100px  */
/* 简写 */
/* transform: translate3d(100px, 100px, 100px); */
transform: translateZ(100px);
/* xyz 是不能省略的，如果没有就写 0 */
/* transform: translate3d(0,100px,100px); */
/* transition:all 1s  ; 过渡  通常与 :hover 配合使用*/

/* 透视 perspective : 如果想在网页中产生 3d 效果需要透视，单位是像素 */
/* 透视写被观察的父元素上面 */
/* z 轴越大，看到的物体越大 */
}
body {
/* 透视写被观察元素的父盒子上 */
perspective: 500px;
}
</style>
</head>

<body>
  <div></div>
</body>
```

#### 3d 转换
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664935437918-33fa69d9-461e-4b5d-a7cf-9e0c601deeb3.png#averageHue=%23f0f1f0&clientId=u6e1aa0ba-39f1-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=187&id=u1f4a6b61&margin=%5Bobject%20Object%5D&name=image.png&originHeight=234&originWidth=565&originalType=binary&ratio=1&rotation=0&showTitle=false&size=60423&status=error&style=none&taskId=ub9d733c9-0d54-47c9-8d5c-0619b95b96b&title=&width=452)

```html
<!-- 3d 转换 -->
<style>
  body {
    /* 加在转换元素的父元素身上 */
    perspective: 200px;
  }

  img {
    display: block;
    margin: 100px auto;
    transition: all 1s;
  }

  img:hover {
    /* transform: rotateX(360deg); */
    /* transform: rotateY(360deg); */
    /* transform: rotateZ(360deg); */
    transform: rotate3d(100, 0, 0, 100deg);
  }
</style>
</head>

<body>
  <img src="../../images/1.png" alt="" />
```
###### transfrom-style
```html
<!-- 3d 转换 -->
<style>
  body {
    /* 加在转换元素的父元素身上 */
    perspective: 400px;
  }

  .box {
    width: 200px;
    height: 200px;
    margin: 100px auto;
    transform-style: preserve-3d;
    transition: all 2s;
  }

  .box:hover {
    transform: rotateY(60deg);
  }

  /* transfrom-style  控制子元素是否开启三维立体环境 
  参数： flat 默认不开启   preserve-3d 开启
  写父级元素上，影响的是子元素
  */
  .box div {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: tomato;
  }

  .box div:last-child {
    background-color: teal;
    width: 100%;
    height: 100%;
    transform: rotateX(60deg);
  }
</style>
</head>

<body>
  <div class="box">
    <div></div>
    <div></div>
  </div>
```
###### 3d 导航栏案例
```html
* {
margin: 0;
padding: 0;
}

ul {
margin: 100px;
}
ul li {
width: 120px;
height: 35px;
/* box 也要旋转，直接给 li 加，里面的盒子都有透视效果 */
perspective: 400px;
float: left;
margin-left: 10px;
list-style: none;
}

/* 子绝父相 */
.box {
position: relative;
width: 100%;
height: 100%;
/* margin: 100px auto; */
/* 保留子元素立体空间 */
transform-style: preserve-3d;
/* perspective: 400px; */
}
.box:hover {
transform: rotateX(90deg);
transition: all 0.4s;
}

.front,
.bottom {
position: absolute;
top: 0;
left: 0;
width: 100%;
height: 100%;
}

.front {
background-color: tomato;
z-index: 1;
transform: translateZ(17.5px);
}

.bottom {
background-color: teal;
/* 有移动或者其他样式，必须先写移动 */
transform: translateY(17.5px) rotateX(-80deg);
}
</style>
</head>

<body>
  <ul>
    <li>
      <div class="box">
        <div class="front"></div>
        <div class="bottom"></div>
      </div>
    </li>
    <li>
      <div class="box">
        <div class="front"></div>
        <div class="bottom"></div>
      </div>
    </li>
    <li>
      <div class="box">
        <div class="front"></div>
        <div class="bottom"></div>
      </div>
    </li>
    <li>
      <div class="box">
        <div class="front"></div>
        <div class="bottom"></div>
      </div>
    </li>
  </ul>
```

##### 旋转木马案例
```html
<style>
  * {
    margin: 0;
    padding: 0;
  }
  body {
    perspective: 800px;
  }

  section {
    position: relative;
    width: 300px;
    height: 200px;
    margin: 150px auto;
    transform-style: preserve-3d;
    animation: roate 10s linear infinite;
    background: url(../../images/1.png) no-repeat;
  }

  /* 鼠标放上去停止动画 */
  section:hover {
    animation-play-state: paused;
  }

  @keyframes roate {
    0% {
      transform: rotateY(0);
    }
    100% {
      transform: rotateY(360deg);
    }
  }

  section div {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url(../../imageS/infinity-2810593.jpg) no-repeat;
  }

  section div:nth-child(1) {
    transform: translateZ(300px);
  }

  section div:nth-child(2) {
    /* 先旋转好了， 再移动 */
    transform: rotateY(60deg) translateZ(300px);
  }
  section div:nth-child(3) {
    /* 先旋转好了， 再移动 */
    transform: rotateY(120deg) translateZ(300px);
  }
  section div:nth-child(4) {
    /* 先旋转好了， 再移动 */
    transform: rotateY(180deg) translateZ(300px);
  }
  section div:nth-child(5) {
    /* 先旋转好了， 再移动 */
    transform: rotateY(240deg) translateZ(300px);
  }
  section div:nth-child(6) {
    /* 先旋转好了， 再移动 */
    transform: rotateY(300deg) translateZ(300px);
  }
</style>
</head>

<body>
  <section>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
  </section>
</body>
```

##### 浏览器私有前缀

- -moz- 代表 firefox 
- -ms- 代表 ie
- -webkit- 代表 safari chrome
- -o-  代表 Opera

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664940517552-e216322b-8bd7-4bb5-9644-c3194223110c.png#averageHue=%23f3f6f8&clientId=u6e1aa0ba-39f1-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=157&id=u3f9fa575&margin=%5Bobject%20Object%5D&name=image.png&originHeight=196&originWidth=703&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17427&status=error&style=none&taskId=u172058d6-e3f0-418a-b20b-74f816a581a&title=&width=562.4)
## 移动端WEB 布局

#### 视口
视口就是浏览器显示页面内容和屏幕区域；分为布局视口，视觉视口，理想视口

- 布局视口，默认通过手动缩放网页，将分辨率设置为 980px
- 视觉视口，用户正在看到的网站的区域
- 理想视口：设备有宽，布局的视口就有多宽
##### 理想视口
meta 标签：告诉浏览器，设备有多宽视口就有多宽
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664941455923-fb33b8d0-09fa-47fd-868f-f8ca202aaec4.png#averageHue=%23f7f8f7&clientId=u6e1aa0ba-39f1-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=107&id=uce144573&margin=%5Bobject%20Object%5D&name=image.png&originHeight=134&originWidth=610&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27852&status=error&style=none&taskId=ub1e98ac7-fb54-49fc-bdba-5b82795004f&title=&width=488)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664941493241-ee25152e-6966-45ba-bda8-ae1ffb290cd7.png#averageHue=%23b4cfea&clientId=u6e1aa0ba-39f1-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=158&id=ua843ac9c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=197&originWidth=624&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46016&status=error&style=none&taskId=ua3111e4c-d421-43d0-8766-be985616ad0&title=&width=499.2)
```html
 <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale-1.0 , minimum-scale=1.0,user-scalable=no"  />
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664941899793-0991c216-8784-403d-8faf-c4b4aae2a40f.png#averageHue=%23f2f3f2&clientId=u6e1aa0ba-39f1-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=172&id=u1cecc74e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=215&originWidth=388&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31483&status=error&style=none&taskId=u89275244-b5d2-40cf-b63a-ea8bb8bd363&title=&width=310.4)
##### 二培图
```html
<style>
  * {
    margin: 0;
    padding: 0;
  }
  /* 准备的实际图片的大小要大 2 倍，这种方式就是 2 倍图 */
  img {
    width: 50px;
    height: 50px;
  }
</style>
</head>

<body>
  <!-- 假设图片本身大小为 100*100 -->
  <img src="../../images/1.png" alt="" />
</body>
```
###### background-size 背景缩放
规则了背景图像的尺寸
background-size（宽度，高度）;
```html
    * {
        margin: 0;
        padding: 0;
      }
      div {
        width: 500px;
        height: 500px;
        border: 1px solid tomato;
        background: url(../../images/1.png) no-repeat;
        /* 只写一个参数 是宽度，高度等比例缩放 */
        /* 里面的单位是 % 是参照父盒子来说的 */
        /* background-size: 500px 500px; */
        /* cover 完整覆盖 div 盒子，部分图片可能 显示不全 */
        /* background-size: cover; */
        /* 宽度到了高度就不拉伸了；或者高度到了宽度就不拉伸了；等比例拉伸 */
        background-size: contain;
      }
    </style>
  </head>

  <body>
    <div></div>
```

#### 移动端开发选择

- 单独开发一套
- 响应式（兼容PC和 移动端）

初始化选择：
```css
/*! normalize.css v8.0.1 | MIT License | github.com/necolas/normalize.css */

/* Document
========================================================================== */

/**
* 1. Correct the line height in all browsers.
* 2. Prevent adjustments of font size after orientation changes in iOS.
*/

html {
  line-height: 1.15; /* 1 */
  -webkit-text-size-adjust: 100%; /* 2 */
}

/* Sections
========================================================================== */

/**
* Remove the margin in all browsers.
*/

body {
  margin: 0;
}

/**
* Render the `main` element consistently in IE.
*/

main {
  display: block;
}

/**
* Correct the font size and margin on `h1` elements within `section` and
* `article` contexts in Chrome, Firefox, and Safari.
*/

h1 {
  font-size: 2em;
  margin: 0.67em 0;
}

/* Grouping content
========================================================================== */

/**
* 1. Add the correct box sizing in Firefox.
* 2. Show the overflow in Edge and IE.
*/

hr {
  box-sizing: content-box; /* 1 */
  height: 0; /* 1 */
  overflow: visible; /* 2 */
}

/**
* 1. Correct the inheritance and scaling of font size in all browsers.
* 2. Correct the odd `em` font sizing in all browsers.
*/

pre {
  font-family: monospace, monospace; /* 1 */
  font-size: 1em; /* 2 */
}

/* Text-level semantics
========================================================================== */

/**
* Remove the gray background on active links in IE 10.
*/

a {
  background-color: transparent;
}

/**
* 1. Remove the bottom border in Chrome 57-
* 2. Add the correct text decoration in Chrome, Edge, IE, Opera, and Safari.
*/

abbr[title] {
  border-bottom: none; /* 1 */
  text-decoration: underline; /* 2 */
  text-decoration: underline dotted; /* 2 */
}

/**
* Add the correct font weight in Chrome, Edge, and Safari.
*/

b,
strong {
  font-weight: bolder;
}

/**
* 1. Correct the inheritance and scaling of font size in all browsers.
* 2. Correct the odd `em` font sizing in all browsers.
*/

code,
kbd,
samp {
  font-family: monospace, monospace; /* 1 */
  font-size: 1em; /* 2 */
}

/**
* Add the correct font size in all browsers.
*/

small {
  font-size: 80%;
}

/**
* Prevent `sub` and `sup` elements from affecting the line height in
* all browsers.
*/

sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}

sub {
  bottom: -0.25em;
}

sup {
  top: -0.5em;
}

/* Embedded content
========================================================================== */

/**
* Remove the border on images inside links in IE 10.
*/

img {
  border-style: none;
}

/* Forms
========================================================================== */

/**
* 1. Change the font styles in all browsers.
* 2. Remove the margin in Firefox and Safari.
*/

button,
input,
optgroup,
select,
textarea {
  font-family: inherit; /* 1 */
  font-size: 100%; /* 1 */
  line-height: 1.15; /* 1 */
  margin: 0; /* 2 */
}

/**
* Show the overflow in IE.
* 1. Show the overflow in Edge.
*/

button,
input { /* 1 */
  overflow: visible;
}

/**
* Remove the inheritance of text transform in Edge, Firefox, and IE.
* 1. Remove the inheritance of text transform in Firefox.
*/

button,
select { /* 1 */
  text-transform: none;
}

/**
* Correct the inability to style clickable types in iOS and Safari.
*/

button,
[type="button"],
[type="reset"],
[type="submit"] {
  -webkit-appearance: button;
}

/**
* Remove the inner border and padding in Firefox.
*/

button::-moz-focus-inner,
[type="button"]::-moz-focus-inner,
[type="reset"]::-moz-focus-inner,
[type="submit"]::-moz-focus-inner {
  border-style: none;
  padding: 0;
}

/**
* Restore the focus styles unset by the previous rule.
*/

button:-moz-focusring,
[type="button"]:-moz-focusring,
[type="reset"]:-moz-focusring,
[type="submit"]:-moz-focusring {
  outline: 1px dotted ButtonText;
}

/**
* Correct the padding in Firefox.
*/

fieldset {
  padding: 0.35em 0.75em 0.625em;
}

/**
* 1. Correct the text wrapping in Edge and IE.
* 2. Correct the color inheritance from `fieldset` elements in IE.
* 3. Remove the padding so developers are not caught out when they zero out
*    `fieldset` elements in all browsers.
*/

legend {
  box-sizing: border-box; /* 1 */
  color: inherit; /* 2 */
  display: table; /* 1 */
  max-width: 100%; /* 1 */
  padding: 0; /* 3 */
  white-space: normal; /* 1 */
}

/**
* Add the correct vertical alignment in Chrome, Firefox, and Opera.
*/

progress {
  vertical-align: baseline;
}

/**
* Remove the default vertical scrollbar in IE 10+.
*/

textarea {
  overflow: auto;
}

/**
* 1. Add the correct box sizing in IE 10.
* 2. Remove the padding in IE 10.
*/

[type="checkbox"],
[type="radio"] {
  box-sizing: border-box; /* 1 */
  padding: 0; /* 2 */
}

/**
* Correct the cursor style of increment and decrement buttons in Chrome.
*/

[type="number"]::-webkit-inner-spin-button,
[type="number"]::-webkit-outer-spin-button {
  height: auto;
}

/**
* 1. Correct the odd appearance in Chrome and Safari.
* 2. Correct the outline style in Safari.
*/

[type="search"] {
  -webkit-appearance: textfield; /* 1 */
  outline-offset: -2px; /* 2 */
}

/**
* Remove the inner padding in Chrome and Safari on macOS.
*/

[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}

/**
* 1. Correct the inability to style clickable types in iOS and Safari.
* 2. Change font properties to `inherit` in Safari.
*/

::-webkit-file-upload-button {
  -webkit-appearance: button; /* 1 */
  font: inherit; /* 2 */
}

/* Interactive
========================================================================== */

/*
* Add the correct display in Edge, IE 10+, and Firefox.
*/

details {
  display: block;
}

/*
* Add the correct display in all browsers.
*/

summary {
  display: list-item;
}

/* Misc
========================================================================== */

/**
* Add the correct display in IE 10+.
*/

template {
  display: none;
}

/**
* Add the correct display in IE 10.
*/

[hidden] {
  display: none;
}
```

##### 移动端常见布局
单独制作：

- 流式布局
- flex 弹性布局
- less + rem + 媒体查询布局
- 混合布局

响应式：

- 媒体查询
- bootstarp

#### flex 布局
flex 布局原理: flexible（灵活的，可变通的） Box 任何一个容器都可以指定为 flex 布局

- 父盒子设为 flex 布局以后，子元素的 float , clear , vertical-align 属性将失效
- 伸缩布局 = 弹性布局 = 伸缩盒布局 = 弹性布局 =  flex 布局

采用 flex 布局的元素称为flex 容器，它的所有子元素自动成为容器成员。
通过给父盒子添加 flex 属性，来控制子盒子的位置和排列方式。

常见父元素属性：

- flex-direction  设置主轴方向
- justify-content 设置主轴上的子元素排列方式
- flex-wrap 设置子元素是否换行
- align-content 设置侧轴上的子元素的排列方式（多行）
- align-items  设置侧轴上的子元素排列方式（单行）
- flex-flow 重复属性，相当于同时设置了 flex-direction 和 flex-wrap

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664947963044-86b848a1-75c3-44f9-81b3-7483f2067b65.png#averageHue=%23f8f9f8&clientId=u945af55f-b0a3-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=309&id=u3b62d273&margin=%5Bobject%20Object%5D&name=image.png&originHeight=386&originWidth=725&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46601&status=error&style=none&taskId=uf82a53f8-1810-4729-96de-4e78b2cf5eb&title=&width=580)
属性值：

- row 默认值从左到右
- row-reverse 从右到左
- column  从上到下
- column-reverse 从下到上

```html
div {
display: flex;
width: 80%;
height: 300px;
background-color: tomato;
/* 默认的主轴是 x 轴 row 那么 y 轴就是侧轴 */
/* 元素是跟着主轴来排列的 */
/* flex-direction: row; */
/* flex-direction: row-reverse; 翻转 */
/* 把主轴设为 y 轴，侧轴就是 x 轴 */
/* flex-direction: column; */
/* flex-direction: column-reverse; 翻译 */

}
div span {
/* width: 150px; */
height: 100px;
background-color: teal;
margin-right: 10px;
flex: 1;
}
</style>
</head>

<body>
  <div>
    <span>1</span>
    <span>2</span>
    <span>3</span>
  </div>
```

##### justify-content 设置主轴上子元素的排列方式
| 属性值 | 说明 |
| --- | --- |
| flex-start | 默认值从头部开始，如果主轴是x轴，从左到右 |
| flex-end1 | 从尾部开始排列 |
| center | 在主轴居中对齐；主轴是 x 轴，水平居中 |
| space-around | 平分剩余空间 |
| space-between | 先两边贴边，现平分剩余空间 |

```html
div {
display: flex;
width: 80%;
height: 300px;
background-color: tomato;
flex-direction: row;
/* justify-content: flex-start; */
/* justify-content: flex-end; */

/* 让子元素居中对齐 */
/* justify-content: center; */

/* 平分剩余空间 */
/* justify-content: space-around; */

/* 两边贴边现平分剩余空间 */
justify-content: space-between;
}
div span {
height: 100px;
width: 40px;
background-color: rgb(11, 212, 253);
margin-right: 5px;
}
</style>
</head>

<body>
  <div>
    <span>1</span>
    <span>2</span>
    <span>3</span>
  </div>
```

##### flex-wrap 控制子元素是否换行

- wrap 换行
- nowrap 默认不换行
```html

      div {
        display: flex;
        width: 300px;
        height: 300px;
        background-color: tomato;
        flex-direction: row;

        /* 默认情况下，默认是不换行的，所有元素显示在行，如果装不下就会缩小子元素的宽度，来存放进去 */
        /* 控制子元素是否换行  nowrap 不换行 ， 默认的 */
        /* flex-wrap: wrap; */
      }
      div span {
        height: 50px;
        width: 100px;
        background-color: rgb(11, 212, 253);
        margin-right: 5px;
      }
    </style>
  </head>

  <body>
    <div>
      <span>1</span>
      <span>2</span>
      <span>3</span>
      <span>4</span>
      <span>5</span>
    </div>
```
#### align-items 设置侧轴上的子元素排列方式（单行）
| flex-start | 从上到下 |
| --- | --- |
| flex-end | 从下到上 |
| center | 挤在一起居中（垂直居中） |
| stretch | 拉伸 默认值 |

```html
   div {
        display: flex;
        width: 80%;
        height: 300px;
        background-color: tomato;
        flex-direction: row;
        /* 主轴居中 */
        justify-content: center;
        /* 侧轴居中 */
        align-items: center;
        /* 拉伸： 子元素不要给高度值 */
        /* align-items: stretch; */
      }
      div span {
        /* height: 50px; */
        width: 100px;
        background-color: rgb(11, 212, 253);
        margin: 5px;
      }
    </style>
  </head>

  <body>
    <div>
      <span>1</span>
      <span>2</span>
      <span>3</span>
    </div>
```

##### align-content 设置侧轴上子元素的排列方式（多行）
只能用于出现换行的情况，在单行下是没有效果的。
```html
div {
        display: flex;
        width: 300px;
        height: 300px;
        background-color: tomato;
        flex-direction: row;
        /* 换行 */
        flex-wrap: wrap;

        /* 有了换行才可以使用 */
        align-content: center;
      }
      div span {
        /* height: 50px; */
        width: 100px;
        background-color: rgb(11, 212, 253);
        margin: 5px;
      }
    </style>
  </head>

  <body>
    <div>
      <span>1</span>
      <span>2</span>
      <span>3</span>
      <span>1</span>
      <span>2</span>
      <span>3</span>
    </div>
```
> 单行（没有换行）找 algin-items  多行换行找  align-content


![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664951964990-77d2dde4-596f-456d-9819-d9f8cebb7f9f.png#averageHue=%23fcfdfd&clientId=uf3c5fe14-904e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=168&id=ue507bfe1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=210&originWidth=773&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16534&status=error&style=none&taskId=u18a7e3f3-721c-4d97-ba7d-582fd07af3e&title=&width=618.4)
##### flex-flow 
```css
}

      div {
        display: flex;
        width: 300px;
        height: 300px;
        background-color: tomato;

        /* flex-direction: column;
        flex-wrap: wrap; */
        /* 上面两个的简写 */
        flex-flow: column wrap;
      }
      div span {
        height: 50px;
        width: 100px;
        background-color: rgb(11, 212, 253);
        margin: 5px;
      }
```

#### 子项 flex 常见属性

- flex 子项目占的份数
- align-self 控制子项目自己在侧轴的排列方式
- order 属性定义子项的排列顺序（前后顺序 ）
###### flex 属性  
```html
 section {
        display: flex;
        width: 60%;
        height: 150px;
        background-color: tomato;
        margin: 0 auto;
      }

      section div:nth-child(1) {
        /* width: 100px;
        height: 150px; */
        flex: 1;
        background-color: teal;
      }
      section div:nth-child(2) {
        background-color: tan;
        flex: 2;
      }
      section div:nth-child(3) {
        flex: 1;
        background-color: thistle;
      }

      p {
        display: flex;
        width: 60%;
        height: 150px;
        background-color: tomato;
        margin: 100px auto;
        /* text-align: center; */
      }

      p span {
        flex: 1;
      }
    </style>
  </head>

  <body>
    <section>
      <div></div>
      <div></div>
      <div></div>
    </section>

    <p>
      <span>1</span>
      <span>2</span>
      <span>3</span>
    </p>
```
#### algin-slef  order
```html
     p {
        display: flex;
        width: 60%;
        height: 150px;
        background-color: tomato;
        margin: 100px auto;
      }

      p span:nth-child(3) {
        /* align-self: flex-end;
        background-color: aqua; */
        order: -1;
        /* 默认是 0 ， -1 比 0 小所以在前面 */
      }
    </style>
  </head>

  <body>
    <p>
      <span>1</span>
      <span>2</span>
      <span>3</span>
    </p>
  </body>
```


#### 媒体查询
@media 可以针对不同的屏幕设置不同的样式
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1664959450602-8915ea61-292a-427d-8fca-02165a290da3.png#averageHue=%23f8f9f8&clientId=uf86823aa-fa0a-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=218&id=u826ed72d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=272&originWidth=651&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38667&status=error&style=none&taskId=u61435026-a6e4-4b1a-b6c5-9de5bf611f3&title=&width=520.8)
```html
  /* 在屏幕上 并且最大的宽度是 800 px 设置我们想要的样式 */
      @media screen and (max-width: 800px) {
        body {
          background-color: tomato;
        }
      }

      @media screen and (max-width: 500px) {
        body {
          background-color: teal;
        }
      }
      /* 可以根据不同的尺寸来设置不同的样式 */
    </style>
  </head>

  <body>
    <div>
      <p></p>
    </div>
```

##### 让元素在一行内显示方法 
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title></title>
<style>
dl{
/*    display:flex;*/
/*flex-direction: row;*/
/*justify-content: center;*/
        }

 .dt,.dd {
    display:inline-block;
/* width : 50px;*/
/*    backdground-color:tomato;*/
/* float:left;*/
         }
</style>
</head>
<body>
<dl>
<dt class="dt">title</dt>
<dd class="dd">1</dd>
<dd class="dd">2</dd>
<dd class="dd">3</dd>
<dd class="dd">4</dd>
</dl>
</body>
</html>
```
#### 让一个块级元素水平垂直居中
子绝父相

- 在不给子元素指定 width 的情况下
   - 绝对定位+ translate
```html
<style>
  * {
    padding: 0;
    margin: 0;
  }

  .father {
    position: relative;
    min-height: 500px;
    background: pink;
  }
  .son {
    position: absolute;
    background: red;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>

<body>
  <div class="father">
    <div class="son">son</div>
  </div>
</body>
```

- 绝对定位+margin
   - 必须要给子元素指定宽高才可以。
- 绝对定位+top,left,bottom,right = 0 + margin:auto
- flex 布局+ margin:auto 
```html
<style>
  * {
    padding: 0;
    margin: 0;
  }

  .father {
    display: flex;
    min-height: 500px;
    background: pink;
  }
  .son {
    margin: auto;
    background: red;
  }
</style>
<body>
  <div class="father">
    <div class="son">son</div>
  </div>
</body>
```






























































































































































































































































































































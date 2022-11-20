# VUE学习

npm install jquery -s  在终端中使用这个命令安装 jquery

npm install webpack[@5.42.1 ](/5.42.1 ) webpack-cli[@4.7.2 ](/4.7.2 ) -D 

在项目中安装webpack



-s 是 –save 的简写

-D 是 –save-dve 的简写



我的名字是 sy1

就使用 npm run sy1 命令进行项目打包

第二个步骤那里 dev 的名字可以 自己取，只要符合要求

执行成功生成 dist 目录（里面会生成 main.js 文件）

我遇到的问题：安装高版本的 nodejs 执行时会失败，解决方法安装旧的版本。

webpack.config.js 文件的配置

```javascript
mode:'development'
// 开发时候使用  development （追求打包速度快）
// 发布上线的 时候使用 production （追求体积小）
```



自己定义打包的入口和出口：



```javascript
const path = require('path');

// 使用 node.js 中的导出语法，向外导出一个 webpack 的配置对象
module.exports = {
// 代表 webpack 运行的模式，可选值为 development 和 production
    mode:'development',
// 开发时候使用  development （追求打包速度快）
// 发布上线的 时候使用 production （追求体积小）
// 指定要处理哪个文件
entry: path.join(__dirname, 'src/index.js'),
// 指定打包后的文件名
output: {//可以自己改
    filename: 'main.js',//输出文件名称
    path: path.join(__dirname, './dist')//输出路径
},
}
```

> 问题：每次修改源代码，就是npm run dev 一次，麻烦！！！


解决：



终端安装 npm install webpack-dev-server[@3.11.2 ](/3.11.2 ) -D 



# less

```bash
在 Node.js 环境中使用 Less ：

npm install -g less
> lessc styles.less styles.css

在浏览器环境中使用 Less ：

<link rel="stylesheet/less" type="text/css" href="styles.less" />
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/3.11.1/less.min.js" ></script>
```

> Less （Leaner Style Sheets 的缩写） 是一门向后兼容的 CSS 扩展语言。这里呈现的是 Less 的官方文档（中文版），包含了 Less 语言以及利用 JavaScript 开发的用于将 Less 样式转换成 CSS 样式的 Less.js 工具。


> TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准（[ES6 教程](https://www.runoob.com/w3cnote/es6-tutorial.html)）。

TypeScript 由微软开发的自由和开源的编程语言。

TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上。


```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Vue</title>
    <!-- <script src="https://unpkg.com/vue@next"></script> -->
    </head>
  <body>
  
    <div id="app">
      <h2>{{ message }}</h2>
    </div>
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          message:"hello,vue"
        }
      });
    </script>
  </body>
</html>
```

## vue 的基本使用



```html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <!-- 希望 Vue 能够控制下面这个 div ，帮助我们把数据填充到 div 内部  -->
    <div id="app">
      {{ username }}
      {{ age }}  
    </div>

    <!-- 导入 Vue 的库文件  两种版本-->
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <!-- <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script> -->
    <!-- 生产环境版本，优化了尺寸和速度 -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    
    <!-- 2. 创建 Vue 的实例对象 -->
    <script>
    // 创建 vue 的实例对象
    const vm = new Vue({
    // el 属性是固定的写法，给发当前 vm 实例要控制页面上的哪个区域，
    // 接收的值是一个选择器
      el:"#app",
      // data 对象就是要渲染到页面上的数据
      data:{
        username:"张三",
        age:"98"
      }
    })

    </script>

  </body>
</html>
```

### 内容渲染指令

1. 
v-text 指令的缺点：会覆盖元素内原来的内容！

2. 
**{{}} 插值表达式**：用的最多，解决覆盖原来的内容的缺点！

3. 
v-html  需要把**包含 HTML 标签的字符串渲染为页面的 HTML 元素.**


第一个和第二个只能渲染文本内容

```html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <!-- 希望 Vue 能够控制下面这个 div ，帮助我们把数据填充到 div 内部  -->
    <div  id="app">
      {{ username }}
      {{ age }}  
      
      <!-- 指令（是vue为开发者提供的模板语法）：内容渲染、属性绑定、事件绑定、双向绑定、条件渲染、列表渲染 -->
    <!-- 常用内容渲染指令： v-text  {{}}  v-html -->
  
  <!-- v-text 把对应的值渲染到页面上 -->
      <p v-text="username"></p>
      <p v-text="likecolor"></p>
      <!-- v-text 不太好用缺点（在开发中用的不多）：因为它会覆盖标签里面原本的内容 -->
      
      <!-- {{}}  插值表达式 （用来解决内容覆盖问题） 这个是用的最多的语法 -->
      <p>姓名：{{username}}</p>
      <p>喜欢的颜色：{{likecolor}}</p>
    
    <!-- v-html  需要把包含 HTML 标签的 字符串渲染为页面的 HTML 元素-->
    <p v-text="info"></p>
    <p>{{info}}</p>

    </div>

    <!-- 导入 Vue 的库文件  两种版本-->
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <!-- <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script> -->
    <!-- 生产环境版本，优化了尺寸和速度 -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    
    <!-- 2. 创建 Vue 的实例对象 -->
    <script>
    // 创建 vue 的实例对象
    const vm = new Vue({
    // el 属性是固定的写法，给发当前 vm 实例要控制页面上的哪个区域，
    // 接收的值是一个选择器
      el:"#app",
      // data 对象就是要渲染到页面上的数据
      data:{
        username:"张三",
        age:"98",
        likecolor:"cyen",
        info:'<h4 style="color:red; font-weight:bold;>欢迎来学习 vue<h4>"'
      }
    })

    </script>

  </body>
</html>
```

可以在插值表达中写表达式，也就是支持 javascript 表达式的运算。



### 属性绑定指令

> **注意： 插值表达式（{{ }}）只能用在元素的内容节点中，不能用在元素的属性节点中！**


```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">

  <head>
    <meta charset="utf-8" />
    <title>属性绑定</title>
      </head>
  <body>
    
<!-- 绑定事件 -->
    <div id="app">
      <!-- 输入框 -->
      <!-- v-bind 属性绑定值 -->
      <!-- 对元素属性进行动态绑定时,使用 v-bind -->
       <input type="text" v-bind:placeholder="tips">
     <hr>
     
     <!-- <<img v-bind:src="photo" alt="" style="width:50%;"> -->
     
     <!-- vue 规定 v-bind:  这个指令可以简写为 :   -->
     <img :src="photo" alt="" style="width:50%;">
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          tips:'请输入用户名',
          photo:'https://cn.vuejs.org/images/logo.svg',
          }
      });
    </script>
  </body>
</html>
```

在 vue中，可以使用 **v-bind :** 指令，为元素的属性进行动态绑定。

由于写的非常多可以简写为： **英文的一个冒号：**

```html
v-bind:   ======>  :
```

> 属性绑定的时候也支持表达式运算：

在使用 v-bind 属性绑定期间，如果绑定内容需要进行动态拼接，则字符串的外面应该包裹单引号，eg:


```html
<div :title="'box'+index">这是一个 div</div>

//像这样不加就是明确是一个变量，要去下面找
<div :title="box+index">这是一个 div</div>
```

### 事件绑定

可以使用 **v-on:** 来进行事件绑定。

```html
语法：有括号代码有参数，无括号代表无参数
v-on:click="函数名（）"
<button @click="add">
</button>

在 vm 对象中
methods : {
 add(){

}
}
```

> $event 的应用场景：

如果默认的事件对象 e 被覆盖了，则可以手动传递一个 $event


```html
<button @click="add(3,$event)">
</button>

在 vm 对象中
methods : {
 add(n,sy){
}
}
```

**事件修饰符：**

- .prevent

```html
<a @click.prevent="xx">链接</a>
```

- .stop

```html
<button @click.stop="xxx">
    按钮
</button>
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">

  <head>
    <meta charset="utf-8" />
    <title>事件绑定 v-on</title>
      </head>
  <body>
    
<!-- 绑定事件 -->
    <div id="app">
     <p>count 的值是: {{count}}</p>
     <!-- 点击按钮让 count 加1  可以使用小括号来传递参数-->
     <!-- 传递参数  不加括号表示没有参数,有参数必须要加括号-->
     <!-- <button v-on:click="add(2)">+1</button> -->
     <!-- 使用简写 -->
     <button @click="add(2)">+1</button>
     <button @click="sub">-1</button>
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
    // vm 里面所有属性和方法都可以通过 vm 调用
      var vm = new Vue({
        el:"#app",
        data:{
         count:0,
          },
          // methods 的作用:就是定义事件的处理函数
        methods:{
         // addCount:function(){
          //  console().log("+1");
          //   count++;
          // }
          // 简写写法:推荐使用方便
          add(n){
            // console.log(vm);
              console.log("+1");
            console.log(vm===this);//true  这里是三个等号表示全等
            // vm.count++;  推荐使用 this vm 完全等于 this
            this.count+=n;
          },
          sub(n){
            console.log("-1");
            this.count--;
          }
          
        }
      });
    </script>
  </body>
</html>
```

> **v-on:click=""  也可以简写为 :  [@click ](/click ) **




```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
  <head>
    <meta charset="utf-8" />
    <title>事件绑定</title>
      </head>
  <body>
<!-- 绑定事件 -->
    <div id="app">
    <p>count 的值是: {{count}}</p>
    <!-- 如果count 是偶数，则按钮背景变成红色， 否则取消背景色 -->
    <!-- <button @click="add">+N</button> -->
    <!-- 如果不带参数会默认会有个 e 对象 ，如果我们传递了参数，e 就会被覆盖-->
    <!-- vue 提供了内置变量 ，名字叫做 $event ，它就是原生 DOM 的事件对象 e （名字是 固定写法） -->
    <!-- 如果还想使用 e 对象 就用 $event -->
    <button @click="add(1,$event)">+N</button>
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
            count:0,
          },
        methods:{
          add(n,sy){
            this.count+=n;
            // 判断 count 的是否为偶数
            if(this.count %2 ==0){
              // 偶数
              sy.target.style.backgroundColor='red';
            }else {
              // 奇数
              sy.target.style.backgroundColor='yellow';
            }
          }
        }  
      });
    </script>
  </body>
</html>
```

事件绑定修饰符：

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">

  <head>
    <meta charset="utf-8" />
    <title>事件绑定修饰符</title>
      </head>
  <body>
    
<!-- 绑定事件 -->
    <div id="app">

    <!-- 绑定事件同时阻止默认行为 : 就可以实现点击不跳转，但会执行绑定函数中的内容-->
     <a href="http://www.baidu.com" @click.prevent="show">跳转到百度</a>
    <hr>
    
    <div style="height:150px; background-color:orange; padding-left:150px;
    line-height:150px;
    " @click="divHandler">
    <!-- 如果不使用 stop 修饰符的话，点击按钮也会触发按钮绑定的事件，
     如果使用 stop 就会阻止冒泡 ：也就是只会触发按钮的事件，不会同时触发了div 的事件
     -->
      <button @click.stop="btnHandler">按钮</button>
      
    </div>
    
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
            count:0,
          },
        methods:{
         show(){
            
           console.log("点击了");
         },
         btnHandler(){
           
           console.log("btnHandler");
         },
         divHandler(){
              console.log("divHandler");
         },
         
        }  
      });
    </script>
  </body>
</html>
```

### 事件修饰符– 按键绑定

> 在监听键盘事件时，需要判断详细的按键，可以为键盘相关的事件添加按键修饰符


```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">

  <head>
    <meta charset="utf-8" />
    <title>事件绑定--按键修饰符</title>
      </head>
  <body>
    

    <div id="app">
      <!-- 按键修饰符，按下 esc 就调用 clearinput 函数 -->
     <input  type="text" @keyup.esc="clearinput">
     <input type="text" @keyup.enter="commitAjax">
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
             
          },
        methods:{
          clearinput(e){
            // 功能按下 esc 键 不清除输入框里面的内容
            console.log("触发了 clear");
            e.target.value="";
          },
          commitAjax(e){
            console.log("触发了commit")
          }
        }
      });
    </script>
  </body>
</html>
```

主要用于，在操作时特定的按钮执行特定的操作。

### 双向绑定指令  v-model

> vue 提供了 v-model 双向数据绑定指令，用来辅助开发者在不操作 DOM 的前提下，快速获取表单的数据




v-model 有用的地方：

- 
input 输入框

- 
textarea

- 
select


```html
  <body>
    
<!-- 绑定事件 -->
    <div id="app">
      
    <p>用户的名字是：{{username}}</p>
    <input type="text" v-model="username" >
    <hr>
    <div v-model="username">
      {{username}}
    </div>
    <hr>
    <!-- 相当于下拉框 -->
    <select name="城市" v-model="city" >
      <option value="">请选择城市</option>
      <option value="1">北京</option>
      <option value="2">上海</option>
      <option value="3">广州</option>
    </select>
  
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
        username:"zhangsan",
        city:"",
          }
      });
    </script>
  </body>
```

v-model 指令的修饰符

| .number | 自动将用户的输入值转换为数值类型 | v-model.number |
| --- | --- | --- |
| .trim | 自动过滤用户输入的首尾空白字符 | 。。。 |
| .lazy | 在 change 时更新 而不是在 input 时更新 | 。。。 |


```html
<body>
    
<!-- 绑定事件 -->
    <div id="app">
      <!-- 默认是字符串，要转换成数字 v-model.number 就是会转换成数字  -->
    <input type="text" v-model.number="n1">+<input type="text"
     v-model.number="n2">=<span>{{n1+n2}}</span>
    <hr>
    <!-- .trim  自动过滤掉用户输入的首尾空白字符 -->
    <input type="text" v-model.trim="username">
    <button @click="getname">获取用户名</button>
    <hr>
    <input type="text" v-model.lazy="username">
    <button @click="getname">获取用户名</button>
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          username:"zs",
          n1:0,
          n2:0,
          },
          methods:{
            getname(){
              console.log('用户名是:'+this.username)
            }
          }
      });
    </script>
  </body>
```

### 条件渲染指令 v-if v-show

> 用来控制 DOM 的显示与隐藏： v-if v-show


```html
<body>
    
<!-- 绑定事件 -->
    <div id="app">
      <button>切换状态按钮</button>
      <!-- 隐藏方式不同 v-if 每次才会动态的移除元素/创建元素 v-show 会使用
      style="display:none" 来隐藏-->
      <p v-if="flag">这是被 v-if 控制的元素</p>
      <p v-show="flag">这是被 v-show 控制的元素</p>
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          // 如果 flag 为true 则显示被控制的元素 相反则隐藏
        flag:true,        
          }
      });
    </script>
  </body>
```

> v-show ：动态为元素添加或移除 display:none 样式，来实现显示或隐藏

v-if : 每次动态创建或移除元素元素 ，实现显示或隐藏元素


- 
如果频繁切换元素的显示状态用 v-show 性能会更好

- 
如果刚进入页面的时候，有些元素默认不需要被展示，而且后期这个元素很可能也不需要被展示出来，这种问情下 v-if 性能会更好


配套指令：v-if 可以和 v-else 配套使用

==但 v-else 不可以单独使用==

v-else-if，充当 v-if 的 “else-if" 块，可以连续使用：

v-else-if 指令**必须配合** v-if 指令一起使用，否则它不会被识别！

```html
      <div v-if="type=='A'">优秀</div>
      <div v-else-if="type =='B'">良好</div>
      <div v-else-if="type =='C'">中等</div>
      <!-- 其他情况都差 -->
      <div v-else>差</div>
```

### v-for（循环渲染指令）

```vue
  <body>
    
<!-- 绑定事件 -->
    <div id="app">
    <table >
      
      <thead>
          <th>索引</th>
          <th>ID</th>
          <th>姓名</th>
      </thead>

      <tbody>
        <!-- 可选的第二个参数 （item,index) in items    其实就是多了个下标-->
        <tr v-for="(item,index) in list" :title="item.title+index">
          <td>{{index}}</td>
          <td>{{item.id}}</td>
          <td>{{item.name}}</td>
        </tr>
        
      </tbody>
    </table>
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
        list:[
          {id:1,name:"张三"},
          {id:2,name:"李四"},
          {id:3,name:"王五"}
        ]
          }
      });
    </script>
  </body>
```

**官方建议，只要用到了 v-for 指令，那么一定要绑定一个 :key 属性

而且尽量把 id 作为属性的值**

```vue
<!-- 官方建议，只要用到了 v-for 指令，那么一定要绑定一个 :key 属性 
         而且尽量把 id 作为属性的值
         官方对 key 的值类型，是有要求的，只能是字符串或者数字类型
-->
```

> key 注意事项：


- 
key 的值只能是数字或字符串

- 
key 的值必须是**唯一性**（key 的值不能重复）

- 
那座把数据项 id 属性的值作为key 的值（因为 index 的值不具有唯一性）

- 
建议使用 v-for 指令时一定要指定 key 的值


> 总结：

vue ：


- 
导入 vue.js 文件

- 
new Vue() 构造函数，得到 vm 实例对象

- 
声明 el 和 data 数据节点

- 
MVVM 的对应关系


> 常见指令：


- 
插值表达式、v-bind  （：）、v-on（@）、v-if 和 v-else

- 
v-for 和 :key 、 v-model


#### 过滤器（本质是函数）的使用

> 是 vue 为开发者提供的功能，常用于文本的格式化。可以用在两个地方：插值表达式和 v-bind 属性绑定

它应该被添加在 Js 表达式的尾部，由“管道符” 进行调用




那个竖线就是管道符。

```vue
  <body>
    
<!-- 绑定事件 -->
    <div id="app">
      <!-- 这个真正的值是以 sycapi 的返回值为准 -->
    <p>message 的值是 ：{{message | sycapi }}</p>
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
        message:"hello vue.js",
        
          },
          // 声明过滤器  过滤器函数必须被定义到 filters 节点之下
          // 过滤器本质上是一个函数
          filters:{
            sycapi(val){
              // 注意过滤器函数形参中 val 永远都是管道符前面的那个值
              // 强调：过滤器中，一定要有一个返回值
              // 和 java 中的 charAt() 用法一样
              //字符串的 slice 方法，可以截取字符串，从指定索引往后截取
              // const up=val.slice(-1);
              // const up=val.slice();  截取整个字符串
              const up=val.slice(0);  //也是截取整个字符串
              console.log(up.toUpperCase());
            }
            
          }
      });
    </script>
  </body>
```

> 上面案例是私有过滤器

注意点：


- 
要定义到 filters 节点下，本质是一个函数

- 
在过滤器函数中，一定要有 return 值

- 
在过滤器的形参中，就可以获取到“管道符”前面那个待处理的值


#### 私有过滤器和全局过滤器

如果希望在多个 vue 实例之间共享过滤器就定义全局过滤器。



**如果全局过滤器和私有过滤器的名字一致时，按照就近原则，就是调用自己的不调用 全局的。有自己的就调用自己的，没有再调用 全局的。**

```html
  // 使用 Vue.filter() 定义全局过滤器
    Vue.filter("sycapi",function(str){

      const first=  str.charAt(0).toUpperCase();
      const other=str.slice(1).toUpperCase();
      return first+other+"====";
    
})
```

> 过滤器可以**串联**地进行调用，也就是**调用多个过滤器**；


```html
<p>message 的值是 ：{{message | sycapi | sss | yyy}}</p>
```



调用过程中传递参数。我们传递的参数必须要从第二个参数开始，因为第一个参数是给了 **“管道符”** 前面的那个值。

### 侦听器

**watch 侦听器**允许开发者监视数据的变化，从而**针对数据的变化做特定的操作**。

```html
var vm = new Vue({
        el:"#app",
        data:{
         username:''
          },
        watch:{
          
        }
      });
```

用法示例：

```html
  <body>
    
<!-- 绑定事件 -->
    <div id="app">
    <input type="text" v-model="username">
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

    <script>
      var vm = new Vue({
        el:"#app",
        data:{
         username:''
          },
          //所有的侦听器都应该被定义到 watch 节点下
        watch:{
          // 侦听器本质上是一个函数，要监听哪个数据的变化，就把数据名作为作为方法即可
          // 固定的新值在前,旧值在后
         username(newsy , oldsy){
           console.log("数据变化了")
           console.log("新值：",newsy)
           console.log("旧值为： ",oldsy)
         }
        }
      });
    </script>
  </body>
```

**侦听器的格式**：

- 
方法格式的侦听器

   - 
缺点：无法在刚进入页面的时候，自动触发！！！

   - 
缺点：如果侦听的是一个对象，如果对象中的属性发生了变化不会触发侦听器

- 
对象格式的侦听器

   - 
好处：可以通过 **immediate** 选项，让侦听器自动触发！！！

   - 
可以通过 **deep** 选项，让侦听器深度监听对象中每一个属性的变化！！！


```html
watch:{
          // 侦听器本质上是一个函数，要监听哪个数据的变化，就把数据名作为作为方法即可
        // 固定的新值在前,旧值在后
         // username(newsy , oldsy){
          //  console.log("数据变化了")
          //  console.log("新值：",newsy)
          //  console.log("旧值为： ",oldsy)
         // }
         
         // 对象格式的侦听器
         username:{
           // 侦听器的处理函数
           handler(newval , oldval){
             console.log(newval, oldval) 
           },
           //这个默认是 false
//控制侦听器是否会自动触发一次，默认不会，要改为 true
           immediate:true
         }
        }
```

```html
  <body>
    
<!-- 绑定事件 -->
    <div id="app">
    <input type="text" v-model="info.username">
    
    </div>
    
    <!-- 导入 Vue.js -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
    <script>
      var vm = new Vue({
        el:"#app",
        data:{
          // 用户的信息对象
          info:{
            username:"admin"
          }
          },
          watch:{
            info:{
              handler(newval){
              console.log(newval)
              },
              // 开启后,对象的属性都会被侦听到
              deep:true
            }
            }            
      });
    </script>
  </body>

===================================================
  // 如果想侦听子属性,则必须包裹一层单引号
          'info.username'(newval){
            
          }
```

## 计算属性

计算属性指的是通过一系列运算之后，最终得到一个属性值。

这个动态计算出来的发展值可以被模板结构或 methods 方法使用。

```html
  var vm = new Vue({
        el:"#app",
        data:{
          r:0,
          g:0,
          b:0
          },
          computed:{
            // 所有的计算属性都必须定义到 computed 节点之下
            // 计算属性在定义的时候，要定义成"方法格式"
//方法要返回一个计算好的对象
            rgb(){
              return `rgb($(this.r),$(this.g),$(this.b))`
            }
            
            
          }
      });
```

==也就是属性值可以动态的被计算==

特点：

1. 
定义的时候，要被定义为 "方法"

2. 
在使用计算属性的时候，当普通的属性使用即可


好处：

1. 
实现了代码复用

2. 
只要计算属性中依赖的数据变化了，则计算属性会自动重新求值。


### axios

> vue-devtools 浏览器调试 Vue 工具

axios 是一个专注于网络请求的库！

网址：[axios中文网|axios API 中文文档 | axios (axios-js.com)](http://www.axios-js.com/)

安装：npm install axios




```html
   <!-- 导入 axios.js -->
    <script   src="../Vuelesson01/node_modules/axios/dist/axios.js"></script>
  
  
   <script>
   //http://www.liulongbin.top:3006/api/getbooks
   
  //  调用  axios 方法得到的返回值是 Promise 对象 
   const result= axios(
   {
     method :'GET',
     url:'http://www.liulongbin.top:3006/api/getbooks'
   }
   )
  // 输出   Promise {<pending>} 
   console.log(result);
   
   result.then(function(books){
       console.log(books); 
   });
   </script>
```

axios 的基本使用

- 
发起 GET 请求
```html
 const result= axios(
   {
     method :'GET',
     url:'http://www.liulongbin.top:3006/api/getbooks',
// URL 中的查询参数
     params :{
       id:1
     },
    //  请求体参数
     data:{}

   }
   )
  // 输出   Promise {<pending>} 
   console.log(result);
   
   result.then(function(books){
       console.log(books); 
   });
```



## vue-cli

> 安装命令：npm install -g @vue/cli


> 创建工程

vue create 项目的名称




最后一个自己配置。



按空格键选中，Enter 键开启下一步



时间有点长等待创建成功：



npm run serve  跑项目

执行成功



==跑的那个终端不要关，关闭终端就是服务器被关闭了！！！==

vue 项目中src 目录的构成：

```html
assets 文件夹：存放项目中用到的静态资源文件，例如：css 样式，图片资源

components 文件夹： 程序员封装的、可复用的组件，都在放到这个目录下

main.js 是项目的入口文件，整个项目的运行要先执行  main.js 

App.vue 是项目的根组件


在工程化的项目中， vue 要做事情很单纯：通过 main.js 把 App.vue 渲染到 index.html 的指定区域中。

App.vue 用来编写待渲染的模板结构
```

### 组件的三个组成部分

> vue 是一个支持组件化开发的前端框架

组件的后缀后是 .vue  这些文件本质上都是一个组件


==每个 .vue 组件都由 3 部分构成，分别是：==

- 
**template**  组件的**模板结构**

- 
**script** 组件的 **javaScript 行为**

- 
**style**  组件的**样式**


### 使用组件的三个步骤

1. 
使用 import 语法来导入

2. 
使用 components 节点注册组件

3. 
以标签形式使用刚才注册的组件






####　数据代理

> 通过一个对象代理对另一个对象中属性的操作（读/写）




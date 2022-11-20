# Vue 3

在创建 vue 项目时不要勾选：就不会有 Eslint 校验

![image_KKmQasp6m0.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663900943229-78e8576c-539e-41c8-abd2-ed4836399bc9.png#averageHue=%23dad9d5&clientId=ua72070fa-087a-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=ufc364c45&name=image_KKmQasp6m0.png&originHeight=456&originWidth=1691&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72913&status=done&style=none&taskId=u70ea6ae2-2606-46ea-9da9-83298ba4403&title=)

### 组件基础：允许我们将 UI 划分为独立的，可重用的部分。

将导入的组件暴露给模板，我们需要有 components 选项上注册它，这个组件将会以其注册时的名字作为模板中的标签名。

每使用一个组件，就创建了一个新的实例


```vue
<script>
import ButtonCounter from './ButtonCounter.vue' //导入组件 

export default {
  components: {
    ButtonCounter //注册组件
  }
}
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>

//==================================另一个文件中
<h1>Here is a child component!</h1>
<ButtonCounter />
<ButtonCounter />
<ButtonCounter /> //注册的组件，使用方法
```

### 传递 props（传递数据）

一个组件可以有任意多个 props， 默认情况下，所有 prop 都接受任意类型的值

```vue
<script>
import BlogPost from './BlogPost.vue'
  
export default {
  components: {
    BlogPost
  },
  data() {
    return {
      posts: [
        { id: 1, title: 'My journey with Vue' },
        { id: 2, title: 'Blogging with Vue' },
        { id: 3, title: 'Why Vue is so fun' }
      ]
    }
  }
}
</script>

<template>
  <BlogPost
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
  ></BlogPost>
</template>
```

可以傅 v-bind 来传递动态 prop 值 

子组件可以通过调用内置的 $emit 方法，传入事件名称抛出一个事件同，

<slot> 是一个占位符，父组件传递进来的内容就会渲染在这里。
-------->    ------>      ------->   ------>       ------->      ------>         ------->        
### 动态组件 

```vue
<!-- currentTab 改变时组件也改变 -->
<component :is="currentTab"></component>
```

<component>  和 is  属性实现

:is 的值可以是：

-  被 注册的组件名 
-  导入的组件对象 

### 组件注册

-  全局注册 
```vue
import { createApp } from 'vue'
import MyComponent from './App.vue'
const app = createApp({})

app.component(
  // 注册的名字
  'MyComponent',
  MyComponent
)
```
 

-  局部注册 

```vue
<script>
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  }
}
</script>

<template>
  <ComponentA />
</template>
```

每个 components 对象里的属性，它们的 key 名就注册的组件名，而值就是相应组件的实现。

局部注册的组件在后代组件中并不可用

任何类型的值都可以作为 props 的值被传递。

所有的 props 都遵循着单向绑定原则 ， props 因父组件 的更新而变化，自然地将新的状态向下流往子组件，而不会逆向传递。

所有 prop 默认都是可选的，除非声明了 required: true

### Attributes 继承 

> 传递给一个组件，却没有被该组件声明为 props 或 emits 的 attribute(属性) 或者 v-on 事件监听器。 eg: class   style id


如果一个子组件的根元素已经有了 class 或 style 属性，它会会父组件上继承的值合并

```vue
<!-- <MyButton> 的模板 -->
<button class="btn">click me</button>

<button class="btn large">click me</button>
```

v-on 监听器也适用于这个规则。原生 <button>元素上有绑定事件，触发的时候也会触发父组件的事件。

在JS 中可以通过 $attrs 这个实例属性来访问组件的所有透传属性。

### 插槽

它标示了父元素提供的插槽内容将在哪里被渲染。

插槽内容可以是任意合法的模板内容，不局限于文本，可以传入多个元素，甚至是组件 

```vue
<script>
import FancyButton from './FancyButton.vue'
import AwesomeIcon from './AwesomeIcon.vue'
  
export default {
  components: { FancyButton, AwesomeIcon }
}
</script>

<template>
  <FancyButton>
    Click me
   </FancyButton>

  <FancyButton>
    <span style="color:cyan">Click me! </span> //元素
    <AwesomeIcon />//组件 
     <AwesomeIcon />
  </FancyButton>
</template>
```

插槽内容可以访问到父组件的数据作用域。但是插槽无法访问子组件的数据。

vue 模板只能访问其定义时所处的作用域

父组件模板中的表达式只能访问父组件的作用域；子组件模板中的表达式只能访问子组件的作用域。

### 具名插槽

当有多个插槽出口的时候，可以使用 <slot> 的  name 属性用来给各个插槽分配唯一的id，区分每一处要渲染的内容。

v-slot 对应的简写为  #  

```vue
<BaseLayout>
  <template v-slot:header>     
  //它的简写形式为
  <template #:header>
   //将目标插槽的名字传给该指令
    <!-- header 插槽的内容放这里 -->
  </template>
</BaseLayout>
```

```vue
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```

> 📌**当一个组件同时接收默认插槽和具名插槽时，所有位于顶级的非 <template> 节点都被隐式地视为默认插槽的内容。和上面等效的写法：**


```vue
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <!-- 隐式的默认插槽 -->
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```

动态插槽名： 动态指令参数在 v-slot 上也是有效的

```vue
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>

  <!-- 缩写为 -->
  <template #[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

动态参数：需要用 [] 包裹。

### Vue- Router

```vue
  <p>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
```

<router-view> 将显示与 url 对应的组件

每一个路由都映射到一个组件 

### 动态路由

动态字以冒号开始：

```vue
path:'users/:id'
```

如果 <router-view> 没有设置名字，那么默认 default

```vue

<script>
 import {version}  from 'vue'
export default {
  data() {
    return {
      count: 89,
      i:true,
      someObject:{}
    }
  },
  mounted(){
    const _th=someObject
    const newObject = {}
    this.someObject = newObject
    this.i= _th === this.someObject
    //这里输出 false ，
    this.increment()
  },
  methods :{
    increment() {
      this.count++
    } ,
    //在 methods 里面使用箭头函数，它没有自己的 this 上下文，访问不到
    increment1:()=>{
      this.count--
    }
  }
}
</script>
<template>
  {{this.i}}
  <br>
  {{count}}
  <br>
  <button @click="increment">
  </button>
</template>


<style>
  button {
    width:50px;
    height:20px;
  }
</style>
```

![image_Oi3yNBJe3x.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663900963263-e2dfac1a-add9-4403-800c-81a9c746a759.png#averageHue=%23d1c6ba&clientId=ua72070fa-087a-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u7d4cfb53&name=image_Oi3yNBJe3x.png&originHeight=911&originWidth=878&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79422&status=done&style=none&taskId=u98fb3a44-5e53-40f0-9600-c649bc89227&title=)

在 vue 中，状态都是默认深层响应式的。

```vue
<script>
export default {
  data() {
    return {
      obj:{
        nestd:{count:0},
        arr :['foo','bur']
      }
    }
  },
  methods: {
    mutateDeeply(){
      this.obj.nestd.count++
      this.obj.arr.push('syhk')
    }
  }
}
  //在 vue 中状态默认是深层响应式的，意味着更改深层次对象也能被检测到
</script>
<template>
{{this.obj}}
 <button @click="mutateDeeply">
  </button>
</template>
```

v-model

```vue
//v-model 使用
//<textarea> 中是不支持插值表达式的，使用 v-model 来替代
<script>
export default {
  data() {
    return {
      message: '',
      checked:false
    }
  }
}
</script>

<template>
  <p>Message is: {{ message }}</p>
  <input v-model="message" placeholder="edit me" />
  <br>
  <hr>
  <textarea v-model='message'           placeholder='syhk'>
  </textarea>
</template>
```

vue 生命周期

![image_JC-GPFqx0T.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663900971998-858b2d11-ca12-4a0e-b064-ff4e4d26cb87.png#averageHue=%23d6c8bc&clientId=ua72070fa-087a-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u57b67337&name=image_JC-GPFqx0T.png&originHeight=914&originWidth=739&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78624&status=done&style=none&taskId=uf4e567d6-34bf-4412-bf24-1138515e5bc&title=)

```vue
<script>
  export default{
    data() {
      return {
        msg:'',
        oldmsg:''
      }
    },
    watch: {
       msg (newValue,oldValue) {
        this.msg=newValue
        this.oldmsg=oldValue
      }
    }
  }
</script>
<template>
<p>
  {{this.msg}}
  <br>
  {{this.oldmsg}}
  </p>
  <form>
    <input type="text" v-model="msg" />
    <input type="submit"/>
  </form>
</template>
```

```vue

    <script type="text/javascript">
        let person = {
            name:'张三',
            age:18
        }
// Proxy 可以捕获到从对象身上的任何操作
        // const p = new Proxy(person,{
        //     //  读取 p 的某个属性时调用
        //     get(target,b){
        //     console.log("读取了某个属性",target,b);
        //     return target[b]
        //     },
        //     //   修改或追加 p 的某个属性时调用
        //     set(target,name,value) {
        //         console.log(`有人修改了 p 身上的 ${name},我要去更新了`)
        //     },
        //     //  删除 p 的某个属性时调用
        //     deleteProperty(target,name){
        //         console.log(`有人删除了 p 身上的 ${name} 属性，我要去更新了`)
        //         var flag= delete target[name]
        //         return flag 
        //     },
            
        // })

       let obj={a:1,b:2}



    //    通过 Proxy  代理： 拦截对象中任意属性的变化包括增删改查
    //   Reflect 反射： 对被代理对象的属性进行操作

// reactive 对比 ref
/*
数据角度对比：
ref 用来定义：基本类型数据 
reactive 对象或数组类型数据 
备注： ref 也可以用来定义对象 （或数组） 类型数据，它内部会自动通过  reactiv 转为代理对象
原理角度对比：
ref 通过 Object.defineProperty()与 set 来实现 响应式（数据劫持）
reactive 通过使用 Proxy 来实现响应式（数据劫持），并通过 Reflect 操作源对象内部的数据
从使用角度：
ref 定义的数据 ： 操作数据需要 .value ，读取数据时模板中真的读取不需要 .value 
reactive 定义的数据：操作数据与读取数据，均不需要 .value
*/
```

### script setup

> script setup 是vue3 的一个语法糖，在 setup 函数中。所有 ES 模块导出都被认为是暴露给上下文的值，并包含在 setup() 返回对象中。


```typescript
<script setup>    </script>
```

### 知识点

```vue
// 计算属性和 watch 的区别
// computed 有缓存
// computed 只有依赖数据发生改变，才会重新计算
// 不支持异步
// 如果 computed 属性属性值是函数,那么默认会走 get 方法;函数的返回值就是属性的属性值;
// 在 computed 中的,属性都有一个 get 和一个 set 方法,当数据变化时,调用 set 方法
// computed 中的成员可以只定义一个函数作为只读属性,也可以定义定义成 get/set 变成可读写属性,但是 methods 中的成员没有这样的
// watch 每次都需要执行函数,watch 更适用于数据变化时的异步操作
```

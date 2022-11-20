学习网站：

```vue
 {
    path: '/login',
    name: 'login',
    component: () => import('../views/login.vue'),
    meta: {
      // meta 是元信息
      redirectAlreadyLogin: true
    }
  },
  {
    // 动态字段以冒号开始 :  也就是路径参数
    // eg: /login/syhk  /login/syhk1 这样的 URL 都会映射到同一个路由中
    // 可以使用 ?  修饰符 (0个或 1 个) 将一个参数标记为可选
    path: '/login/:id?',
    component: () => import('../views/AboutView.vue'),
    meta: {
      // 用户登录后才能访问的路由
      requiredLogin: true
    },
    // 默认情况下，所有路由是不区分大小写的，并且能匹配带有或不带尾部斜线的路由
    // 可以通过 sensitive 来修改
    sensitive: true,

  }
]

const User = {
  // 取到路由参数的值
  templaet: '<div>User {{$route.params.id}}</div>',
  // return $route.params.id  返回给模板使用

}
```

目录结构：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663923832000-e9fd2474-ef00-44b3-af7c-e811d80bc287.png#clientId=u6ff1a04f-e2eb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=737&id=u69224569&margin=%5Bobject%20Object%5D&name=image.png&originHeight=921&originWidth=247&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68705&status=done&style=none&taskId=ua1bf11d5-7923-417f-9536-f82bc84dd22&title=&width=197.6)
router/test1.vue
```vue
<template>
  <h1>Test1</h1>
  <p>{{ router }}</p>
  <button @click="change1">点击更改1</button>
  <button @click="change2">点击更改2</button>
  <button @click="change3">点击更改3</button>
  <button @click="change4">移动4</button>
</template>

<script lang="ts">
  import { defineComponent, watch } from "vue";
  import router from "@/router";
  import home from "@/views/HomeView.vue";
  export default defineComponent({
    setup() {
      // 编程式导航
      // 字符串路径
      const change1 = function () {
        router.push("/");
      };
      //  带有路径的对象
      const change2 = () => {
        router.push({ path: "/about" });
      };
      const username: string = "syhk";
      // const change3 = () => {
      //   // 注意这里反单引号
      //   router.push(`user/${username}`); //导航到 /user/syhk
      // };
      // const change3 = () => {
      //   // 和上面效果一样的
      //   router.push({ name: "user", params: { username } }); //导航到 /user/syhk
      // };

      // const change3 = () => {
      //   // params 不能与 path 一起使用
      //   router.push({ path: "/user", params: { username } }); //导航到 /user
      // };
      // const change3 = () => {
      //   // 带参数  结果是 ： /user/syhk?name=syhk
      //   router.push({ name: "user", query: { name: "syhk" } });
      //   //    push 和其他所有导航方法都会返回一个 Promise ，让我们可以等到导航完成知道成功还是失败
      // };
      //  替换当前位置
      // 它和 push 唯一不同的是，它在导航时不会向 history 添加新记录

      const change3 = () => {
        // 带参数  结果是 ： /user/syhk?name=syhk
        //   router.push({ name: "user", replace: true });
        // 上面那个相当于
        router.replace({ path: "/user/syhk" });
        //    push 和其他所有导航方法都会返回一个 Promise ，让我们可以等到导航完成知道成功还是失败
      };

      /**
     * 声明式和编程式总结 ：
     * <router-link :to="..">  对应的编程式 router.push(...)
     * <router-link :to=".." replace>  router.replace(...)
     */

      //  横跨历史
      // 一个整数作为参数，表示历史堆栈中前进或打不通多少步
      const change4 = () => {
        //   router.go(1);
        // 向前移动一条记录，与 router.forward() 相同
        router.go(-1);
        //   返回一条记录，与 router.back() 相同
        router.go(3);
        //   前进 3 条记录
        //   如果没有那么多记录，默认失败
        router.go(100);
      };

      return {
        router,
        change1,
        change2,
        change3,
        change4,
      };
    },
    created() {},
  });
</script>
```
App.vue
```vue
<template>
  <!-- <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav> -->
  <!-- 路由出口，路由匹配到的组件将渲染在这里 -->
  <!-- <router-view /> <br/> -->

  <router-link to="/first">First page</router-link> |
  <router-link to="/second">Seconde page</router-link>
  <!-- 命名视图：就是把指定的组件，通过路由配置渲染到指定的 router-view 上 -->
  <router-view></router-view>
  <router-view name="a"></router-view>
  <router-view name="b"></router-view>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
```
导航守卫/test1.vue
```vue
<template>
  <h1>导航守卫测试</h1>
</template>

<script lang="ts">
import defineComponent from "vue";
import router from "vue-router";
import  home from '/src/views/HomeView'
export default defineComponent({
  setup() {
    // 导航守卫测试

    // 全局前置守卫
    // to 即将要进入的目标
    // from 当前导航正要离开的路由
    // 返回值
    // fals有 : 取消当前的导航，URL 地址会重置到 from 路由对应的地址

    // router.beforeEach((to, from) => {
    //   // ...
    //   return false;
    // });
//可选参数 next
      router.beforeEach(async (to,from,next)=>{
        if(检查是否有记录 && 登录 密码是否正确  &&
        // // 避免无限重定向
        //  to.name !== 'Login'){
        //   // 检测通过可以登录
        //   return {name:'Login'}
        // }
        // 使用第三个参数
        if(判断 ) next({name:'Login'})
        else next()
      });
      // 如果没有通过或者出现意外情况，就会抛出一个  Error，取消导航并且调用 router.onError() 注册的回调


      router.onError((error)=>{
        console.log(error);
      })


      // 全局解析守卫
      // 它在每次导航时都会被触发
      router.beforeResolve()

      // 全局后置钩子
      router.afterEach((to,from)=>{
        sendToAnalytics(to.fullPath)
      })


      // 组件内的守卫
      beforeRouteEnter(to,from){
        // 在渲染组件的对应路由被验证前调用
        // 这个守卫执行时，组件实例还没有被创建
      }

      beforeRouteUpdate(to,from){
        // 在当前路由改变，但是该组件被复用时调用
        // eg: 动态参数的路径： /user/:id  在 `/users/1` 和 `/users/2` 之间跳转的时候，
        // 组件实例会被复用，这个钩子就在这人时候调用
      }

      beforeRouteLeave(to,from){
        // 导航离开渲染该组件的对应路由时调用
      }


      // 检测导航故障
      const navigationResult = await router.push('/');
      if(navigationResult){
        // 导航被阻止
//...
      }else{
        // 导航成功 （包括重新导航的情况)
          //...
      }



    return {};
  },
});
</script>
```
index.ts
```vue
import { createRouter, createWebHashHistory, RouteRecordRaw } from 'vue-router'
  import HomeView from '../views/HomeView.vue'
  import First from '../views/命名视图/first.vue'

  import Second from '../views/命名视图/second.vue'
  import Third from '../views/命名视图/Third.vue'

  const routes: Array<RouteRecordRaw> = [
    // {
    //   path: '/',
    //   name: 'home',
    //   component: HomeView
    // },
    {
      path: '/about',
      name: 'about',
      // route level code-splitting
      // this generates a separate chunk (about.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
    },
    // {
    //   // 动态字段都以冒号开始 eg: test1/123  test1/syhk 都会匹配到这个路由
    //   // 当 test1/123 导航到 test1/syhk 时，相同的组件实例将被重复使用，两个路由都渲染相同的组件，复用将会更高效
    //   // 也就意味着组件的生命周期钩子不会被调用 
    //   path: '/test1/:id',
    //   name: 'test1',
    //   component: () => import('@/views/router/test1.vue')
    // },
    // 捕获所有路由或 404 路由
    {
      // 像这样所有访问到没有设置的路由都会匹配到这里
      // 也可以使用正则表达式匹配
      path: '/:pathMatch(.*)*',
      name: 'NotFound',
      component: () => import('@/views/404.vue')
    },
    // {
    //   // 这样指定了动态参数 userId 只匹配数字，其他不匹配，如果不加限制就是匹配任何内容
    //   path: '/:userId(\\d+)',
    //   name: 'test1',
    //   component: () => import('@/views/router/test1.vue')
    // },
    {
      // 使用 ?  修饰符表示一个参数标记为可选
      // eg: test1   test1/23 都会匹配到这个路由
      path: '/test1/:userId?',
      name: 'test1',
      component: () => import('@/views/router/test1.vue'),
      sensitive: true,
      strict: true
    },
    {
      path: '/user/syhk',
      name: 'user',
      component: () => import('@/views/router/user.vue')
    },
    {
      path: '/first',
      name: 'first',
      // 指定渲染在哪个视图上
      components: {
        default: First,
        a: Second,
        b: Third,
      }
    }, {
      path: '/second',
      name: 'second',
      components: {
        default: Third,
        a: Second,
        b: First,
      }
    },
    {
      // 重定向 可以省略 component 配置，它从来没有被直接访问过，没有组件需要渲染
      path: '/shouyue',
      redirect: '/'
    },
    // 相对重定向
    //就是 path 匹配不上就匹配  rdirect　返回回来的数据，访问两个都是可以的
    // 路径都可以使用动态参数
    {
      path: '/abouts',
      redirect: to => {
        // 相对位置不以 / 开头
        return 'about'
      }
    },
    // 别名
    // 通过别名可以自由地将 UI 结构 映射到一个任意的 URL,而不受配置的嵌套结构的限制 

    {
      path: '/about4',
      component: () => import('@/views/AboutView.vue'),
      // 这样配置后这个路由有三个别名： 可以访问下面三个中的任意一个都可以访问到  
      alias: ['/about1', '/about2', '/about3'],
    }
    // 将  props 传递给路由组件 ，这块有点看不明白，后面再看
    , {
      path: '/before',
      name: 'before',
      component: () => import('@/views/router/test1.vue'),
      // 路由独享的守卫
      // 这种独享的路由只会在进入路由的时候都会触发
      beforeEnter: (to, from) => {
        // 为 false 不跳转路由
        // 为 true 跳转路由
        return true;
      },
      // 路由元信息
      // 把任意信息附加到路由上
      meta: { requiresAuth: false }
    }

  ]

  // 默认情况下，所有路由都不区分大小写的，并且能匹配带有或不带有尾部斜线的路由
  //eg : test1  Test1  TEST1 ... 都会匹配到同一个路由
  const router = createRouter({
    /**
   * 不同的历史模式
   * Hash 模式： 是用 createWebHashHistory 创建的
   * HTML5 模式： createWebHistory() 创建这个模式，推荐使用这个模式
   */
    history: createWebHashHistory(),
    routes
  })
  export default 
```




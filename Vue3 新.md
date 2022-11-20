
上代码：

```vue
<template>
  <p class="msg">{{ msg }}</p>

  <div>
    <input type="text" id="input" />
    <button onclick="vm.text='Hello Syhk'">设置为 Syhk</button>
  </div>
  <div id="output"></div>

  

</template>

<script lang="ts">
import { defineComponent, onBeforeMount, onMounted, ref ,reactive} from "vue";
export default defineComponent({
  // 使用 setup 的情况下，不能再用 this 来获取 Vue 实例，也无法通过 this.xxx、this.fn() 这样来获取实例上的数据，或者执行实例上的方法
  setup(props,context) {
  // props 类型为： Object 由父组件传递下来的数据，可选
  // context 类型为：Object 组件的执行上下文 可选
    console.log(1)
    onBeforeMount(() => {
      console.log(2);
    });

    onMounted(() => {
      console.log(3);
    });

    console.log(4);
    // 输出 1  4  2 3

    const msg = "Hello World";

    // 要给 template 使用的数据必须要在这里面返回，如果不是可以不用写在这里
    return {
      msg,
      
    };
  },

  /*
    setup 组件创建前执行
    setup 组件创建后执行
    onBeforeMount 组件挂载到节点之前执行
    onMounted 组件挂载完成后执行
    onBeforeUpdate 组件更新之前执行
    onUpdated 组件更新之后执行
    onBeforeUnmount 组件卸载之前执行
    onUnmounted 组件卸载完成后执行
    onErrorCaptured 当捕获一个来自子孙组件的异常时激活钩子函数
  */
});
// // 定义一个响应式数据
// const vm=new Proxy({},{
//   set(obj,key,value){
//     document.querySelector("#input").value=value
//     document.querySelector("#output").innerText=value

//   }
// })
// // 处理输入行为
// document.querySelector("#input").oninput=function(e){
//   vm.text=e.target.value
// }

// 相对于 2.x 在 data 里定义后即可通过 this.xxx 来调用响应式数据，3.x 的生命周期里取消了 Vue 实例的 this，你要用到的比如 ref 、reactive 等响应式 API ，都必须通过导入才能使用，然后在 setup 里定义。
const msg = ref<string>("SYHK");
  // 切记！：对 ref 对象的值的读取，必须通过 value，否则读取不到
console.log(msg.value)
// 多类型
const phoneNumber = ref<number | string>(15356042885);
setTimeout(()=>{
  phoneNumber.value=123456;
},1000) //1 秒后修改它的值
  // 基本类型的定义
const msg1 = ref<boolean>(false);
// 引用类型
interface Member {
  id: number;
  name: string;
}
const userInfo = ref<Member>({
  id: 1,
  name: "Syhk",
});
// 定义一个普通数组
const uids = ref<number[]>([1, 2, 3]);
// 对象数组
const memberLsit = ref<Member[]>([
  {
    id: 1,
    name: "syhk1",
  },
  {
    id: 2,
    name: "syhk2",
  },
]);

// reactive 它是响应式 api 对于ref它只适合对象和数组
// 声明
interface Member1{
  id:number,
  name:string
}
// 使用 和ref 一样都要导入才能使用
const userInfo1:Member1 =reactive({
  id:1,
  name:'syhk1'
});
// 数组
const userList:Member1[]=reactive([
  {
   id:1,
   name:'s1' 
  },
  {
    id:2,
    name:'s2'
  }

]);
// reactive 对象在读取字段的值和修改的时候，与普通对象是一样的
console.log(userInfo1.id)
userInfo1.id=5
// 普通数组操作
let uids1:number[]=[1,2,3];
let numderUids:number[]=[1,2,3];
// 合并另外一个数组
uids1=[...uids1,...numderUids]
console.log(uids1) //输出 1,2,3,1,2,3

let uids2:number[]=reactive([1,2,3]);
uids2=[];
setTimeout(()=>{
  uids2.push(1);
  console.log(uids2+'#11111111111111')//有 1 
},1000)
uids2=[];// 丢失响应性
uids2.length=0;// 不会破坏响应性
</script>

<style scoped>
.msg {
  font-size: 14px;
}
</style>
```

```vue
<template>
  <h1>TEST2</h1>

  <ul class="user-info">
    <li class="item">
      <span class="key">ID:</span>
      <span class="value">{{ id }}</span>
    </li>

    <li class="item">
      <span class="key">gender:</span>
      <span class="value">{{ gender }}</span>
    </li>

    <li class="item">
      <span class="key">name:</span>
      <span class="value">{{ name }}</span>
    </li>

    <li class="item">
      <span class="key">age:</span>
      <span class="value">{{ age }}</span>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from "vue";

interface Member {
  id: number;
  name: string;
  age: number;
  gender: string;
}
let name: string = "syhk";
export default defineComponent({
  setup() {
    // 定义一个 reactive 对象
    const userInfo = reactive({
      id: 1,
      name: "syhk",
      age: 18,
      gender: "male",
    });
    // 定义一个新的对象，它本身不具备响应性，但是它的字段全部是 ref 变量
    const userInfoRefs = toRefs(userInfo);

    // 2s 后更新userInfo
    setTimeout(() => {
      userInfo.id = 2;
      userInfo.name = "syhkuserInfo";
      userInfo.age = 20;
      userInfo.gender = "mag";
    }, 2000);

    return {
      ...userInfoRefs,
      name,
      // userInfoRefs 的时候包含了一个字段，如果有一个单独的变量叫 name ，谁会生效取决于谁排在后面
    };
  },
  /*
    toRef  创建一个新的 ref 变量，转换 reactive 对象的某个字段为 ref 变量
    toRefs 创建一个新的对象，它的每个字段都是 reactive 对象各个字段的 ref 变量
    在 toRef 的过程中，如果使用了原对象上面不存在的 key ，那么定义出来的变量的 value 将会是 undefined 。
    如果你对这个不存在的 key 的 ref 变量，进行了 value 赋值，那么原来的对象也会同步增加这个 key，其值也会同步更新。
    */
});
</script>

<style scoped></style>
```

```vue
<template>
  <h1>函数的定义和使用</h1>
  <p>{{ msg }}</p>

  <button @click="changeMsg">修改 MSG</button>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
// 在 vue 3版本中函数和数据的定义一样，都是通过 setup 来完成
// 在 setup 里定义任意类型的函数（普通函数，class 类，箭头函数，匿名函数等等）
// 要在模板里面使用也需要 return 出去
// 需要自动执行的函数，执行时机需要遵循生命周期

export default defineComponent({
  setup() {
    const msg = ref<string>("syhk");
    // 这个函数需要暴露给模板使用，必须 return 才可以使用
    function changeMsg() {
      msg.value = "Hi Syhk";
    }

    // 这个要在页面载入时执行，无需 return 出去
    const init = () => {
      console.log("init");
    };

    // 在这里执行 init
    onMounted(() => {
      init();
    });

    return {
      msg, //数据
      changeMsg, //方法
    };
  },
});
</script>

<style></style>
```

```vue
<template>
  <h1>Watch 监听</h1>

  <p>{{ userInfo.name }}</p>
  <p>{{ userInfo.age }}</p>

  <hr />

  <p>{{ message }}</p>
  <p>{{ index }}</p>
</template>

<script lang="ts">
import { defineComponent, reactive, watch, ref, isReactive ,WatchStopHandle,watchEffect} from "vue";
//vue2 写法
// export default {
//   data() {
//     return {
//       a: 1,
//       b: 2,
//       c: {
//         d: 4
//       },
//       e: 5,
//       f: 6
//     }
//   },
//   watch: {
//     // 侦听顶级 property
//     a(val, oldVal) {
//       console.log(`new: ${val}, old: ${oldVal}`)
//     },
//     // 字符串方法名
//     b: 'someMethod',
//     // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
//     c: {
//       handler(val, oldVal) {
//         console.log('c changed')
//       },
//       deep: true
//     },
//     // 侦听单个嵌套 property
//     'c.d': function (val, oldVal) {
//       // do something
//     },
//     // 该回调将会在侦听开始之后被立即调用
//     e: {
//       handler(val, oldVal) {
//         console.log('e changed')
//       },
//       immediate: true
//     },
//     // 你可以传入回调数组，它们会被逐一调用
//     f: [
//       'handle1',
//       function handle2(val, oldVal) {
//         console.log('handle2 triggered')
//       },
//       {
//         handler: function handle3(val, oldVal) {
//           console.log('handle3 triggered')
//         }
//         /* ... */
//       }
//     ]
//   },
//   methods: {
//     someMethod() {
//       console.log('b changed')
//     },
//     handle1() {
//       console.log('handle 1 triggered')
//     }
//   }
// }
// vue3 ==================================================
// 有三个参数
// watch(
// source, 必传要监听的数据源
// callback,必传 监听到变化后要执行的回调函数
// options 可选，一些监听选项
// )

// watch 的数据源必须具备响应性或者是一个 getter eg:()=> attr ，如果用 let
// 定义一个普通变量，是无法监听的
export default defineComponent({
  setup() {
    // 定义一个响应式数据
    const userInfo = reactive({
      name: "Petter",
      age: 18,
    });

    // 2 s 后改变数据
    setTimeout(() => {
      userInfo.name = "syhk";
    }, 2000);
    // 监听这个响应式对象
    watch(userInfo, () => {
      console.log("监听整个 userInfo", userInfo.name);
    });
    // 也可以监听对象里面的某个值,数据源要写成 getter 函数
    watch(
      () => userInfo.name,
      // 回调函数
      (newValue: string, oldValue: string) => {
        console.log("只监听 name 的变化", userInfo.name);
        console.log("打印变化前后的值", { newValue, oldValue });
      }
    );

    // 批量监听 ========================================
    const message = ref<string>("");
    const index = ref<number>(0);

    // 2 s 后改变数据
    setTimeout(() => {
      (message.value = "syhkp"), index.value++;
    }, 2000);

    // 抽离相同的处理行为为公共函数
    const handleWatch = (
      newValue: string | number,
      oldValue: string | number
    ): void => {
      console.log({ newValue, oldValue });
    };

    // 定义多个监听操作,传入这个公共函数
    watch(message, handleWatch);
    watch(index, handleWatch);

    // watch API 还提供了一个批量监听的用法,数据源和回调参数变成了数组的形式
    watch(
      // 数据源改成了数组
      [message, index],
      // 回调的入参变成了数组,每个数组里面的顺序和数据源排序一致
      ([newMessage, newIndex], [oldMessage, oldIndex]) => {
        console.log("message的变化:", [newMessage, oldMessage]);
        console.log("index 的变化:", { newIndex, oldIndex });
      }
    );
    // 监听的选项
    /**
选项:
deep : boolean 默认值 false : 是否进行进行深度监听
immediate : boolean 默认值 false : 是否立即执行监听回调
flush : string  可选值: pre(默认),post,sync : 控制监听回调的调用时机
onTrack : (e)=>void :在数据源被追踪时调用
onTrigger :(e)=>void : 在监听回调被触发时调用
后面两个仅在开发模式下生效
deep 默认是 false,但是在监听 reactive 对象或数组时,会默认为 ture
在为 false 的情况下,如果直接监听一个响应式的引用类型数据,虽然它的属性的值有变化,但对其本身来说是不变的,所以不会触发 watch 的 callback
*/
    // 关闭尝试监听的例子:
    const nums = ref<number[]>([]); //这里使用 reactive 对象是默认开启的
    // 2s 后给这个数组添加项目
    setTimeout(() => {
      nums.value.push(1);
      console.log("修改后", nums.value);
    }, 2000);

    watch(
      nums,
      () => {
        //这里的 callback 不会被触发,因为关闭了 deep
        console.log("触发监听", nums.value);
      },
      {
        deep: false, //关闭 deep
      }
    );

    // 我们可以通过 isReactive 来判断是否需要手动开户深度监听
    // 监听这个数据时会默认开户深度监听
    const foo = reactive({
      name: "Petter",
      age: 18,
    });
    console.log(isReactive(foo)); //true 需要导入

    // 监听数据时,不会默认开启深度监听
    const bar = ref({
      name: "syhkbar",
      age: 20,
    });
    console.log(isReactive(bar)); //false

    // 监听选项之 immediate
    // watch 默认是惰性的,就是只有当被监听的数据源发生变化时才执行回调
    // 这里不会触发 watch 的回调
    const message1 = ref<string>("");

    // 2s 后改变数据
    setTimeout(() => {
      // 来到这里都会触发 watch 的回调
      message1.value = "SYHKmessage1";
    }, 2000);

    watch(message1, () => {
      console.log("触发监听", message1.value);
    });
    // 以上例子发现初始化的时候不会触发监听回调,可以通过 immediate 选项来让它直接触发
    // 它默认是 false ,可以设置为 true 让回调立即执行
    // 这里就会触发 watch 的回调了

    const message2 = ref<string>("");

    // 2s 后改变数据
    setTimeout(() => {
      message2.value = "settimeout里面的";
    }, 2000);

 const unwa=   watch(
      message2,
      () => {
        // 这里已经是第二次触发了
        console.log("immediate触发监听immediate", message2.value);
      },
      {
        //设置 immediate 选项
        immediate: true,
        // 在带有  immediat 选项时,不能在第一次回调时取消该数据源的监听
      }
    );

    // 监听选项:flush  用来控制监听回调的调用时机
    // pre 默认值:将在渲染前调用 场景:允许回调函数在模板运行前更新了其他值
    // sync 在渲染时被同步调用
    // post 被推迟到渲染之后调用 场景:如果通过 ref 操作 DOM 元素与子组件,需要使用这个值来启用该选项,以达到预期的执行效果

    // =======================停止监听=======================
    // 在 setup 里使用 watch 组件被卸载的时候也会一起被停止
    // 随着组件被卸载一起停止的前提是:watch 必须是同步语句创建的
    // 但是如果是 setTimeout 等异步函数里面创建,则不会绑定当前组件,就不会一起停止 watch 就需要手动停止
    // 在定义一个 watch 行为的时候,它会返回一个用来停止监听的函数
    // 在合适的地方调用它,可以取消这个监听

    // 异步语句中创建的
    const message3 = ref<string>("message3");
    setTimeout(()=>{
    const unwatch = watch(
      message3,
      () => {
        console.log("=========message3==============",message3.value);
      }
    );
        unwatch();//这里停止了监听就不会调用回调函数了
       message3.value="settimeout更改 message3";
    },2000);
// 注意:启用了 immediate 选项,不能在第一次触发监听回调时执行它
// 解决方案:
let unwatch1:WatchStopHandle
unwatch1=watch(
    message3,
    ()=>{
        //这里加一个判断,是函数才执行它
        if(typeof unwatch1 === 'function'){
            unwatch1()//这样这里使用就不会报错了
        } 
    },
    {
        immediate:true,
    }
)
 
// 监听效果清理
// 监听后的回调函数有一个参数 onCleanup ,它可注册一个清理函数
// 调用清理函数时机: watcher 即将重新运行的时候;watcher 被停止(组件被卸载或者手动停止监听)
// 注意:需要在停止监听之前注册好清理行为,否则不会生效
let unwatch2:WatchStopHandle
unwatch2=watch(
    message3,
    (newValue,oldValue,onCleanup)=>{
        // 在停止之前注册好清理行为
        onCleanup(()=>{
            console.log("监听清理ing....");
        });
        if(typeof unwatch2 === 'function'){
            unwatch2()//这样这里使用就不会报错了
        } 
    },
    {
        immediate:true,
    }
)
// =========watchEffect====================
// 它也会返回一个用于停止监听的函数
// eg:
// 单独定义两个数据
const name = ref<string>('syhkwatchEffect')
const age=ref<number>(1000)
    // 定义一个调用这两个数据的函数
    const getUserInfo=():void =>{
        console.log({
            name:name.value,
            age:age.value
        });
    }
// 2s 后改变第一个数据
setTimeout(()=>{
    name.value='TOM';
},2000);
// 4s 后改变第二个数据
setTimeout(()=>{
    age.value=1;
},4000)
const getsyhk=():void=>{
    console.log("watchEffect 是否会默认执行一次?");
    // 会默认执行一次
}
// 直接监听调用函数,在每个数据产生变化的时候,它会自动执行
watchEffect(getUserInfo);//两次改变,调用了两次函数,再加上默认执行了一次
/**
 * watch 和 watchEffect 的区别:
 * watch 可以访问侦听状态变化前后的值,而 watchEffect 没有
 * watch 是在属性改变时才执行(不会默认执行一次),而 watchEffect 则默认会执行一次,然后在属性改变的时候也会执行
 * 
 */
// watchEffect 可用的监听选项
// 它不支持 deep 和 immediate,其他的用法一样
// watchPostEffect 是 watchEffect 使用 flush:'post' 选项时的别名
// watchSyncEffect 是 watchEffect 使用 flush:'sync' 选项时的别名
    return {
      userInfo,
      message,
      index,
    };
  },
});
</script>

<style></style>
```

```vue
<template>
  <title>数据的计算</title>
  <h1>TEST5</h1>
  <p>{{ fullName }}</p>

  <input
    type="text"
    v-model="tagsStr"
    placeholder="请输入标签，多个标签用英文逗号分隔"
  />
  <p>{{ tagsStr }}</p>

  <hr />
  <!-- 渲染一段文本 -->
  <span v-text="msg"></span>

  <!-- 渲染一段HTML -->
  <div v-html="html"></div>

  <!-- 循环创建一个列表 -->
  <ul v-if="items.length">
    <li v-for="(item, index) in items" :key="index">
      <span>{{ item }}</span>
    </li>
  </ul>

  <!-- 事件  v-on -->
  <button @click="hello">Hello</button>

  <hr />
  <!-- 自定义指令使用 -->
  <!-- 这个使用默认值 unset -->
  <!-- <div v-syhk>{{msg}}</div> -->

  <!-- 这个使用传递进行的颜色 ,这里要反单引号  -->
  <!-- <div v-syhk="`yellow`">{{msg}}</div> -->

  <!-- 这个使用默认值 unset -->
  <div v-syhk1>{{ msg }}</div>

  <!-- 这个使用传递进行的颜色 ,这里要反单引号  -->
  <div v-syhk1="`yellow`">{{ msg }}</div>
  <slot />
  <!-- 就是占位作用，给父组件放东西 -->
</template>

<script lang="ts">
import { defineComponent, computed, ref } from "vue";
import type { ComputedRef } from "vue"; //类型最好分开导和上面区分开
export default defineComponent({
  // 数据的计算是使用 computed API ，它可以通过现有的响应式数据，去通过计算得到新的响应式变量
  // 这里的响应式数据，可以简单理解为通过 ref API 、 reactive API 定义出来的数据
  setup() {
    // 需要先导入 computed
    // 定义基本数据
    const firstName = ref<string>("Bill");
    const lastName = ref<string>("Gates");
    // 定义需要计算拼接结果的数据
    const fullName = computed(() => `${firstName.value} ${lastName.value}`);
    // 2s 后改变某个数据的值
    setTimeout(() => {
      // template 那边在 2s 后也会显示为 Petter Gates
      firstName.value = "Petter";
    }, 2000);
    // computed 变量和 ref 变量的用法一样，需要通过 value 才能拿到它的值
    // 区别在于：computed 的 value 是只读的
    // computed 需要传入一个回调函数，并 return 一个值，它需要有明确的返回值
    // 类型定义：在 defineCompontent 里会自动帮我们推导 vue api 的类型，一般情况下需要去定义 computed 出来的变量类型
    // 在需要手动指定的情况下，可以导入它的类型 ComputedRef
    const fullName1: ComputedRef<string> = computed(
      () => `${firstName.value}  ${lastName.value}`
    );
    // 对比普通函数的优势：性能优势：只要原始数据没有发生改变，多次访问 computed ，都会立即返回之前的计算结果，而不是多次执行函数，普通的函数调用多少次就执行多少次
    // 注意：它只会更新响应式数据的计算
    // eg:
    const nowTime = computed(() => {
      return new Date(); //它需要有明确的返回值，如果只有一条语句写直接写，不写大括号
    });
    console.log(nowTime.value);
    setTimeout(() => {
      console.log(nowTime.value); //2s 后这样的时间还是和上面的一样的
    }, 2000);
    // nowTime=new Date(); 错误，computed 定义的数据是只读的
    // 无法直接赋值，但是在必要的情况下，可以通过 computed 的 setter 来更新数据
    // eg: 必须使用　set 和 get 方法名，也只接受这个两个方法名
    const firstName1 = ref<string>("Bill");
    const lastName1 = ref<string>("Gates");
    const foo = computed({
      // 这里需要明确的返回一个值
      get() {
        //getter 用 value 读取值
        return `${firstName1.value}   ${lastName1.value}`;
      },
      // 这里接收一个参数，修改 foo时的新值
      set(newFirstName: string) {
        //setter
        firstName1.value = newFirstName;
      },
    });
    console.log(fullName.value);
    // 更新数据
    setTimeout(() => {
      // 这里更新的其实是 firstName1
      foo.value = "syhk";

      console.log(firstName1.value); //syhk

      console.log(foo.value); //syhk Gates
    }, 2000);
    // 应用场景：数据的拼接和计算，利用组件的动态数据
    const tags = ref<string[]>([]);

    const tagsStr = computed({
      get() {
        return tags.value.join(",");
      },
      set(newValue: string) {
        tags.value = newValue.split(",");
      },
    });
    // ===================指令=======================
    // v-show  v-bind :   v-slot #  v-pre   v-once
    const msg = ref<string>("Hello World!");
    const html = ref<string>('<p style="color:red;">Hello World!</p>');
    const items = ref<string[]>(["a", "b", "c", "d"]);

    function hello() {
      console.log(msg.value);
    }
    // ==========钩子函数========================
    /**
     * created 在绑定元素的 attribute 或事件监听器被应用之前调用
     * beforeMount 当指令第一次绑定到元素并且在挂载父组件之前调用
     * mounted 在绑定元素的父组件被挂载后调用
     * beforeUpdate 在更新包含组件的 VNode 之前调用
     * updated 在包含组件的 VNode及其子组件的 VNode 更新后调用
     * beforeUnmount 在卸载绑定元素的父组件之前调用
     * unmounted 当指令与元素解除绑定且父组件已卸载时，只调用一次
     */
    // 用法：
    const myDirective = {
      created(el: any, binding: any, vnode: any, prevVnode: any) {},
      mounted(el: any, binding: any, vnode: any, prevVnode: any) {},
      // 其他钩子
    };
    // 每个钩子函数都有 4 个入参
    // el 指令绑定的 DOM 元素,可以直接操作它
    // binding 一个对象数据
    // vnode el对应在 vue 里的虚拟节点信息
    // prevVNode Update 时的上一个虚拟节点信息,仅在 beforeUpdate 和 updated 可用
    // 其中使用最多的就是 el 和 binding
    // el 的值就是通过 document.querySelector 拿到的那个 DOM 元素
    /**
     * binding 包含的属性：
     * value 传递给指令的值，eg: v-foo="bar",支持任意有效的JS表达式
     * oldValue 指令的上一个值，仅对 beforeUpdate 和 updated 可用
     * arg 传给指令的参数eg:v-foo:bar
     * modifiers 传给指令的修饰符eg:v-foo.bar
     * instance 使用指令的组件实例
     * dir 指令定义的对象
     */
    // 返回给模板使用
    return {
      fullName,
      tagsStr,
      msg,
      html,
      items,
      hello,
    };
  },
  //  如何注册一个自定义指令
  // 局部注册
  // 编写自定义指令，和setup() 同级别
  // directives:{
  //   // directives 下的每个字段名就是指令名称
  //   syhk:{
  //     // 钩子函数
  //     mounted(el,binding){
  //       el.style.backgroundColor=
  //       typeof binding.value === 'string' ? binding.value : 'unset';
  //     },
  //   },

  // },
  // 在上面模板中使用，以上是对象式的写法
  // 也可以写成函数式
  directives: {
    //这个名字是固定的
    syhk1(el: any, binding: any) {
      el.style.backgroundColor =
        typeof binding.value === "string" ? binding.value : "unset";
    },
  },
  // 局部注册的自定义指令，默认在子组件内生效，子组件内不需要重复注册就可以使用
  // 自定义指令也可以注册成全局，在每个组件内就都可以使用，只要入口文件 main.ts 里启用它。

  // deep 选项
  // 作用：如果自定义指令用于一个有嵌套属性的对象，并且需要在嵌套属性更新的时候触发 beforeUpdate 和 updated 钩子，需要把它设为 true 才生效
  // eg:
  // directives:{
  //   foo:{
  //     beforeUpdate(el,binding){
  //       // ....
  //     },
  //     updated(el,binding){
  //       // ....
  //     },
  //     mounted(el,binding){
  //       // ...
  //     },
  //     deep:true,
  //   }
  // }
});
</script>

<!-- 
在 script-setup 模式下，定义了就可以直接在 template 里面使用

<script setup lang="ts">
  const msg:string ='syhk';
</script>
 -->
```

```vue
<template>
  <h1>TEST6</h1>
  <h1>插槽</h1>
  <Child>
    <!-- 组件是子组件要在父组件中使用 -->
    <!-- 父组件里面编写的内容被存放到子组件预先的 <slot/> 那个位置上 -->
    <p>这是插槽内容</p>
    <div>这是父组件的内容</div>

    <!-- 父组件使用具名插槽 -->
    <template v-slot:title>
      <h1>这是标题</h1>
    </template>

    <!-- 这里 # 是 v-slot 的缩写 -->
    <template #author>
      <h1>这是作者信息</h1>
    </template>

    <!-- 默认插槽内容 -->
    <p>这是默认插槽内容</p>
  </Child>
</template>

<script setup lang="ts">
//使用这个模式，子组件导入就可以使用而不用注册
// vue 在使用子组件的时候，子组件在 template 里像一个HTML 标签，可以在这个子组件标签里传入任意模板代码以及HTML 代码这就是插槽
// 默认插槽
// 默认情况下，子组件使用 <slot/> 标签即可渲染父组件传下来的插槽内容
import { defineComponent } from "vue";
import Child from "./test7.vue"; //导入子组件

// 具名插槽
// 子组件通过 name 属性来指定插槽名称

// export default defineComponent({
//     components:{
//         Child,//注册子组件
//     },
// });
</script>
```

```vue
<template>
  <p>这是子组件的内容</p>
  <!-- 具名插槽：子组件通过 name 属性来指定插槽名称 -->
  <div class="title">
    <slot name="title" />
  </div>
  <div class="author">
    <slot name="author" />
  </div>

  <div class="content">
    <!-- 其他插槽的内容放到这里 -->
    <slot />
  </div>
  <!-- 转到父组件应用： test6 里面 -->
  <hr />
  <!-- 默认内容：当父组件没有传入插槽内容时，会使用默认内容来显示 -->
  <div class="other">
    <slot>这是父组件没有传递东西过来时，显示的默认内容</slot>
  </div>
  <!-- 
注意：父组件里的所有内容都是在父级作用域中编译的
子组件里的所有内容都是在子作用域中编译的
 -->

  <!-- :class 动态绑定样式 -->
  <!-- 使用 :class 需要提前写好样式 -->
  <!-- 只想绑定一个单独的动态样式 -->
  <p :class="activeClass">SYHK</p>
  <!-- 绑定多个动态样式 -->
  <p :class="[activeClass1, activeClass2]">SYHK</p>
  <!-- 对动态样式做一些判断  -->
  <p :class="{ activeClass: isActive }">SYHK</p>
  <!-- 多个判断  -->
  <p :class="[{ activeClass1: isActive }, { activeClass2: !isActive }]">SYHK</p>

  <ul class="list">
    <li
      class="item"
      :class="{ cur: index === curIndex }"
      v-for="(item, index) in 5"
      :key="index"
      @click="curIndex = index"
    >
      {{ item }},{{ curIndex }},{{ index }}
    </li>
  </ul>

  <!-- 使用 :style 动态修改内联样式
默认情况下是转入一个对象去绑定
-->
  <p
    :style="{
      fontSize: '13px',
      'line-height': 2,
      color: '#ff2222',
      textAlign: 'center',
    }"
  >
    Hello World!
  </p>

  <!-- 绑定多套 style：需要在 script　先定义好各自的样式变量，通过数组来传入 -->
  <p :style="[style1, style2]">多套样式绑定</p>

  <hr />
  <p class="msg">v-bind动态修改样式</p>

  <hr />
  <!-- style module  -->

  <!-- <p :class="$style.msg1">Hello vue3 module</p> -->
  <!-- 使用自定义的名字 -->
  <!-- <p :class="syhk.msg1">Hello vue3 module</p>  -->

  <!-- useCssModule API -->
  <!-- v-html 来渲染 HTML 代码，在渲染的 HTML 里面添加样式 -->

  <div v-html="content"></div>
</template>

<script lang="ts">
import { defineComponent, ref, useCssModule } from "vue";

export default defineComponent({
  setup() {
    const activeClass: string = "activeClass";
    const activeClass1: string = "activeClass1";
    const activeClass2: string = "activeClass2";
    const isActive: boolean = true;
    const curIndex = ref<number>(0);

    // v-bind 动态修改 style ,在 3.2.0 之后才有
    // 在这里定义
    const fontColor = ref<string>("#ff0500");

    // 定义
    const style1 = {
      fontsize: "19px",
      "line-height": 3,
    };
    const style2 = {
      color: "#f12345",
      textAlign: "center",
    };

    // 获取样式
    const style = useCssModule("syhk");
    console.log(style);

    // 编写模板内容
    const content = `<p class="${style.msg}">
        <span class="${style.text}">Hello World! --- from v-html</span>
        </p>`;
    console.log(content);

    return {
      activeClass,
      activeClass1,
      activeClass2,
      isActive,
      curIndex,
      fontColor,
      style1,
      style2,
      content,
    };
  },
});
</script>

<!-- 也支持给 module 命名 -->
<style module="syhk">
/* <style module>  */
.title {
  width: 300px;
  height: 300px;
  background-color: tomato;
  margin: auto;
}
.author {
  width: 400px;
  height: 400px;
  background-color: aqua;
  margin: auto;
}
.content {
  width: 300px;
  height: 300px;
  background-color: cadetblue;
  margin: auto;
}
.other {
  width: 300px;
  height: 300px;
  background-color: cadetblue;
  margin: auto;
}

/* css 样式与预处理器 */

/* 动态绑定 css */

/* 
使用 :class 动态修改样式名


*/

.activeClass {
  color: red;
}
.activeClass1 {
  color: blue;
}
.activeClass2 {
  color: tomato;
}

/* 
v-bind 动态修改 style ,在 3.2.0 之后才有

*/
.msg {
  /* 官网建议用引号括号 */
  color: v-bind("fontColor");
}
.text {
  font-size: 19px;
}

.cur {
  color: red;
}
/* 
不管有没有开启 scoped 使用 v-bind 渲染出来的 CSS 变量，都会带上 scoped 的随机 hash 前缀，避免样式污染
CSS 没有作用域的概念，写某个样式就全局可用：
VUE 组件中有两种方案： VUE2 就有的 <style scoped>
vue3 中的 <style module>    
    
后面带上 scoped 这css 代码就只运行在这个组件渲染出来的虚拟 DOM 上 

style module  

注意： style module
必须显示的指定绑定某个样式，eg: $style.msg 才能生效
如果指定的类名是 - 命名 eg: .user-name ;就需要通过 $style['user-name'] 去绑定
如果单纯使用<style module> 在绑定样式的时候，默认使用 $style 对象来操作的

注意： 一旦开启  <style module> 里面所编写的样式，都必须手动绑定才能生效，没有绑定的样式会被编译，不会生动生效到 DOM 上

    */

.msg1 {
  color: #f57839;
}

/* 深度操作符: 
使用 scoped 后，父组件的样式将不会渗透到子组件中，但也不能直接修改子组件的样式。
如果确实要修改子组件的样式，必须通过 ::v-deep   :deep (快捷写法)
*/
</style>
```

### 路由

```vue
<template>
  <h1>路由使用</h1>

  <!-- 所有路由组件，要在访问后进行渲染，都必须在父级组件里带有 <router-view/>标签
router-view 在哪里，路由组件的代码就渲染在哪个节点上
在级路由和父级组件就是 src　下的 App.vue
记住 ： vue 3 里面是用什么就导入什么
-->
  <router-view />

  <!-- 使用 router-link 标签跳转 
可以把它当成 target="_self" 的a 标签，但无需要重新刷新页面
等效于：

 route.push({
     name:'home'
 });
-->
  <!-- 以两个效果等价的
to 属性中 test1 和 /test1 是一样的效果
-->
  <router-link to="/test1">首页</router-link>
  <hr />
  <span
    class="link"
    @click="
      route.push({
        name: 'home',
      })
    "
    >首页</span
  >

  <!-- 带参数的跳转 -->
  <span
    class="link"
    @click="
      route.push({
        name: 'home',
        params: {
          id: 123,
        },
      })
    "
    >带参数跳转</span
  >
  <hr />
  <!-- router-link 带参数写法 -->
  <router-link
    class="link"
    :to="{
      name: 'test3',
      params: {
        id: 321,
      },
    }"
  >
    事参数跳转
  </router-link>
  <!-- router-link 默认是被转换为一个 a 标签，但可以把它指定为生成其他标签eg:span div  li  
    这些标签不具备 href 属性，跳转时通过 click 事件去执行 -->
  <!-- eg:  vue2 通过 tag 属性来指定，vue3 使用 custom 和 v-slot 的配合来渲染为其他标签-->
  <!-- 在渲染一个带有路由导航功能的 div
custom 一个布尔值，用于控制是否需要渲染为 a 标签，当不包含它或者值为 false 时，就会依然使用 a 标签渲染
v-slot 是一个对象，用来决定标签的行为：它的值有：
href 解析后的URL ，将会作为一个 a 元素的 href 属性
route 解析后的规范化的地址
navigate触发导航的函数，会在必要时自动阻止事件
isActive 如果需要应用激活的 class 则为true 允许应用一个任意的 class
isExactActive 如果需要应用精确激活的 class 则为 true ，允许应用一个任意的class
一般来说 v-slot 必备的只有 navigate ，用来绑定元素的点击事件，否则元素点击不会有任何反应

要渲染为非 a 标签，切记两个点：
router-link 必须带上 custom 和 v-slot 属性
最终要渲染的标签，写在 router-link 里，包括对应的 className 和点击事件
-->
  <router-link to="/test4" custom v-slot="{ navigate }">
    <span class="link" @click="navigate">首页</span>
  </router-link>
</template>

<script setup lang="ts">
//引入路由
import { watch, watchEffect } from "vue";
import { RouteRecordRaw, useRouter } from "vue-router";
import test3Vue from "./test3.vue";
// vue3 里面是用到什么导入什么
// const routes: Array<RouteRecordRaw> =[
//     // ...
// ]

// 定义变量获取路由信息
const route = useRouter();
const routes: Array<RouteRecordRaw> = [
  {
    path: "/test3",
    redirect: {
      name: "test3",
      query: {
        from: "syhk",
      },
    },
  },
];
// 在 script 里面操作路由
// 跳转首页
// route.push({
//     name:'home'
// });
// 可以通过 span 然后绑定 click 事件来达到 router-link 的效果
// 返回上一页
// route.back();

// const router = createRouter({
//     history: createWebHistory(process.env.BASE_URL),
//     routes
// })

// 暴露定义好的路由数据
// export default routes
// 使用  setup 模式就可以不用写 export 默认的

// 在组件内使用全局钩子
route.beforeEach((to, from) => {
  // ....
});

// ============ 路由监听===============
// 监听整个路由
watch(route, (to, from) => {
  // ....
});
// 第一个参数是传入整个路由，第二个参数是 callback 可以获取 to 和 from 来判断路由变化情况

// 监听路由的某个数据
// watch(
//   () => routes.query.id,
//   () => {
//     console.log("监听到  query 变化 ");
//   }
// );

// watchEffect

// 获取文章详情
const getArticleDetail = (): void => {
  // 直接通过路由的参数来获取文章id
  const ARTICLE_ID: number = Number(route.params.id) || 0;
  console.log("文章id是：", ARTICLE_ID);

  // 请求文章内容
  // 此处略...
};

// 直接监听包含路由参数的那个函数
watchEffect(getArticleDetail);
</script>

<!-- 
公共路径： publicPath 它的默认值是 /
如果路由不止一级，就需要准备的指定 publicPath ，并且保证它是以 / 开关 / 结尾
eg : 如果项目部署在 https://chengpeiquan.com/vue3/
那么 publicPath 就可以设置为/vu3/
 -->

<style scoped></style>
```

目录结构：

![image_Q66Ptl1nCP.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1663901336134-5e847196-da36-4e47-8859-86963ff8594f.png#averageHue=%232a2f37&clientId=u0d7e975e-3a2e-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=ui&id=ue728e384&margin=%5Bobject%20Object%5D&name=image_Q66Ptl1nCP.png&originHeight=876&originWidth=311&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43500&status=error&style=none&taskId=u3c283f28-cac4-4031-aaf1-73499c01f37&title=)

Father内容：

```vue
<template>
  <h1>father 组件之间的通信</h1>

  <!-- 在这里拿到 -->
  <!-- <Child 
title="用户信息"
:index="1"
:uid="userInfo.id"
:user-name="userInfo.name"
/> -->
  <!-- 这样就完成 props  数据的下发
注意：在 template 绑定属性里面，如果是普通的字符串，比如上面的 title 可以直接给属性名赋值就可以
如果是变量或其他类型 Number , Object 等，需要通过属性动态绑定的方式来添加 v-bind 或 :
-->
  <!-- 子组件里面去接收 props -->

  <router-link to="Child">跳转子组件</router-link>

  <!-- emtis -->
  <Child @update-age="updateAge" />
  <!-- 动态绑定 props 是用 :  绑定 emit 是用 @,因为它是个事件 -->
  <hr />
  <!-- V-Model 使用 和emits 
可以同时绑定多个
-->
  <Child v-model:user-name="userInfo.name" v-model:user-uid="userInfo.id" />
  <!-- 接收数据和 props 一样 -->

  <!-- 父组件操作子组件 ref/emits -->
  <!-- 父组件可以通过对子组件 ref 属性,然后通过 ref 变量去操作子组件的数据或者调用里面的方法 -->
  <Child ref="child" />
</template>

<!-- 
通信的方式常用的方法有：
父组件向子组件  子组件向父组件
props        emits
v-model      emits
ref          emits
provide      inject
EventBus    emit/on    emit/on 
Vuex

 -->

<script lang="ts">
// props emits
import { react } from "@babel/types";
import { defineComponent, provide, reactive, ref } from "vue";
import Child from "../views/Child.vue"; //子组件
// 下发 props 是在父组件里面完成的，父组件向子组件下发之前，需要导入子组件并启用它作为自身的模板
interface Member {
  id: number;
  name: string;
  age: Number;
}

export default defineComponent({
  // 需要启用子组件作为模板
  components: {
    Child,
  },
  // 定义一些数据并 return 给 template用
  setup() {
    const userInfo: Member = {
      id: 1,
      name: "syhk",
      age: 18,
    };

    const userInfo1: Member = reactive({
      id: 1,
      name: "Syhk1",
      age: 0,
    });
    // 定义一个更新年龄的方法
    const updateAge = (age: Number): void => {
      userInfo1.age = age;
    };

    // ref 在 script 里面的操作
    // 给子组件定义一个 ref 变量
    //     const child=ref<HTMLElement>(null);
    // // 保证视图渲染完成后现执行操作
    // onMounted(()=>{
    //     // 执行子组件里面的 ajax 函数
    //     child.value.getList();

    //     // 打开子组件里面的弹窗
    //     child.value.isShowDialog=true;
    // })

    // ========= 爷孙组件通信 provide/inject======================
    // 它们之前隔着多层关系,深层引用
    // 要在 setup() 函数
    const msg: string = "Hello World";

    // provide 出去 需要导入
    provide("msg", msg);

    // provide 和 inject 本身不可响应,只需要传入的数据具备响应性,依然可以支持响应
    const msg1 = ref<string>("syhk");
    provide("msg1", msg1);

    // const userInfoo:Member=reactive({
    //     id:1,
    //     name:'syhk'
    // });
    // provide('userInfo2',userInfoo)

    // 绑定  emits
    // 子组件需要向父组件告知数据更新，或者执行某些函数是能完 emits 来进行的
    // 每个 emit 都是事件,要先由父组件给子组件绑定,子组件才知道怎么去调用
    // 要 return 出去，不然 template 拿不到数据
    return {
      userInfo,
      //   给 template 使用
      updateAge,
      //  child,
    };
  },
});
</script>
```

Child 内容：

```vue
<template>
  <h1>child 子组件</h1>

  <!-- 模板中使用 props -->
  <p>标题：{{ title }}</p>
  <p>索引：{{ index }}</p>
  <p>用户id：{{ uid }}</p>
  <p>用户名：{{ userName }}</p>
  <p>年龄为:{{ age }}</p>
</template>

<script lang="ts">
// 接收 Father 父组件下发下来的 props
import { defineComponent, inject } from "vue";
export default defineComponent({
  // 通过与 setup 同级的 props 来接收数据
  // props:[
  //     'title',
  //     'index',
  //     'userName',
  //     'uid'
  // ],
  // 记住写的是TS ，能带有类型的地方，就写上类型
  // 推荐使用对象形式来传递， 和 TS 不同，props 的类型首字母要大写 支持的类型和TS 的差不多
  // 改变后: 这样只有合法的类型才能传入
  // props:{
  //     // 单类型
  //     title:String,
  //     index:Number,
  //     userName:String,
  //     // 多类型 这里用的是 逗号分隔
  //     uid:[Number,String]

  // },
  // 可选及带有默认值的 props
  /**
   * type prop 的类型
   * required 是否必传， boolean
   * default 类型：any   与 type 字段的类型对应的默认值
   * validator 类型：function 自定义验证函数，需要一个 返回boolean值 ，当检验不通过时，会抛出警告信息
   */
  //   props: {
  //     // 可选，并提供默认值
  //     title: {
  //       type: String,
  //       required: false,
  //       default: "默认标题",
  //     },
  //     // 默认可选，单类型
  //     index: Number,
  //     // 添加一些自定义校验
  //     userName: {
  //       type: String,
  //       // 在这里校验
  //       validator: (v:String) => v.length >= 3,
  //     },
  //     // 默认可选，但多类型
  //     uid: [Number, String],
  //   },
  props: {
    title: String,
    index: Number,
    userName: String,
    uid: Number,
    age: Number,
  },
  //   在 script 部分使用 props,vue3 没有了 this ,需要给 setup 添加一个入参才可以操作
  //   和 props 一样,需要引入才可以操作,setup 的第二个入参是一个对象,可以完整导入 expose 然后通过 expose.emit 去操作,也要以按需要导入 {emit}
  setup(props, { emit }) {
    // 这个入参包含了我们定义的所有 props
    console.log(props);

    // prop 是只读，不允许修改
    // setup 的第一个入参，包含了我们定义的所有 props
    // 这个入参可以随意命名

    // 使用 emit
    setTimeout(() => {
      emit("update-age", 22);
    }, 2000);

    // inject 接收
    const msg: string = inject("msg") || "";

    // 拿到 inject ref reactive
    const msg1 = inject("msg1");
    const userInfoo = inject("userInfoo");
    // 这响应式数据 return 给模板使用,会实时更新数据
    // 引用类型的数据,拿到后可以直接用,属性的值更新后,子孙组件也会被更新

    // 基本类型被直接 provide 出去后,再怎么修改,都不会更新下去,子孙组件拿到的永远是第一次的那个值
  },
  //   子组件接收 emtis
  //   emits:[
  //     'update-age' //这个名称是指父组件在绑定时的名称
  //   ]
  // 接收时做一些校验
  // emits:{
  // // 需要校验
  // 'update-age':(age:number)=>{
  // // 写一条件拦截,要记得返回 false
  // if(age<18){
  //     console.log("年龄太小");
  //     return false;
  // }
  // // 通过返回 true
  // return true;
  // },
  // // 多个时,若不需要校验的
  // // 'update-age':null

  // },
  // v-model 的配置 emits
  // props:{
  //     userName:String,
  //     uid:Number
  // },
  // emits:[
  // 'update:userName',
  // 'update:uid'
  // ],
});
</script>
```

router 下的 login.ts 内容：

```typescript
// 路由可以在独立的 ts 文件里去操作

// 导入路由
import router from "@/router";
import { createRouter, createWebHashHistory, onBeforeRouteLeave, RouteRecordRaw } from 'vue-router'
// 执行路由跳转
router.push({
    name: 'test1'
})

// 路由元信息配置
const routes1: Array<RouteRecordRaw> = [
    {
        path: '/login',
        name: 'login',
        component: () => import(/* webpackChunkName: "login" */ '@views/login.vue'),
        meta: {
            title: '登录', // string 用于在渲染的进修配置浏览器标题
            isDisableBreadcrumbLink: true,  // 是否禁用面包屑链接（对一些没有内容的路由可以屏蔽访问）
            isShowBreadcrumb: false,  // 是否显示面包屑
            addToSidebar: false,     //是否加入侧边栏
            sidebarIcon: '',          // 配置侧边栏拳头 默认
            sidebarIconAlt: '',      //  配置侧边栏的图标展开状态
            isNoLogin: true  // 是否免密码登录 
        }
    }
];


// 路由重定向
/**
 * 路由重定向使用一个 redirect 字段，配置以对应的路由里面去实现跳转
 * 配置了 redirect 的路由只需要指定2个字段即可： Path 自己的路径  redirect 目标路由和路径，其他属性可以省略了 name component 可以不要
 * redirect 接收三种类型的值：  string 另外一个路由的 path
 * route 另外一个路由（类似 router.push)
 * function 可以判断不同情况的重定向目标，最终 return 一个 path 或 route
 */
// 配置 为path 只能针对哪些不带参数的路由
const routes: Array<RouteRecordRaw> = [
    // 重定向到 home
    {
        path: '/',
        redirect: '/home'
    },
    // 真正的首页
    {
        path: '/home',
        name: 'home',
        component: () => import('@/views/HomeView.vue')
    }
]

// 配置为 route
const routes2: Array<RouteRecordRaw> = [
    // 重定向到 home，并带上一个 query
    {
        path: '/',
        redirect: {
            name: 'home',
            query: {
                from: 'redirect'
            }
        }
    },
    // 真正的首页
    {
        path: '/home',
        name: 'home',
        component: () => import('@views/test5.vue')
    }

]

// 配置为 function
// const routes3: Array<RouteRecordRaw> = [
//     // 访问主域名时，根据用户的登录信息，重定向到不同的页面
//     {
//         path: '/',
//         redirect: () => {
//             // LOGIN_INFO 是当前用户的登录信息，可以从 localStorage 或者 Vuex 读取
//             const GROUP_ID: number = LOGIN_INFO.groupId;
//             // 根据组别id进行跳转
//             switch (GROUP_ID) {
//                 // 管理员，跳去仪表盘
//                 case 1:
//                     return '/dashboard';

//                 // 普通用户，跳去首页
//                 case 2:
//                     return '/home';

//                 // 其他都认为未登录，跳去登录页
//                 default:
//                     return '/login'
//             }
//         }
//     }
// ]


// 路由别名配置
// 它和路由重定向的异同：
// 配置了路由重定向，当用户访问 /a 时，URL 将会被替换成 /b，然后匹配的实际路由是 /b 。
// 配置了路由别名，/a 的别名是 /b，当用户访问 /b 时，URL 会保持为 /b，
// 但是路由匹配则为 /a，就像用户访问 /a 一样。

// 配置方法 添加 alias 字段实现
// 可实现可以通过 /home 访问首页，也可以通过 /index 访问首页
const routes4: Array<RouteRecordRaw> = [
    {
        path: '/home',
        alias: '/index',
        name: 'home',
        component: () => import('@views/test1.vue')
    }
]

// 404 路由配置
// 可以配置一个 404 路由来代替网站内的 404 页面
const routes5: Array<RouteRecordRaw> = [
    {
        path: '/:pathMatch(.*)*',
        name: '404',
        component: () => import('@views/404.vue')
    }
]
// 这样配置后只要访问到不存在的路由，就会显示这个 404 模板
// 新版的路由不支持直接配置通配符 *


// 导航守卫
// 它支持全局使用，也可以在 .vue 文件时单独使用
/**
 * 路由里的全局钩子函数：
 * 配置了钩子，所有的路由在创建 router 的时候进行全局的配置，
 * 也就是说，只要你配置了钩子，那么所有的路由在调用到的时候，
 * 都会触发这些钩子函数。
 * 
 * beforeEach 全局前置守卫 在路由跳转前触发
 * beforeResolve  全局解析守卫  在导航确认前，同时在组件内守卫和异步趾组件被解析后
 * afterEach 全局后置守卫  在路由跳转完成后触发 
 * 
 */
// import { createRouter } from "vue-router"; 也是需要导入才能使用
// 创建路由 
// beforeEach  路由拦截
const routersyhk = createRouter({ ...})
// 在这里调用导航守卫的钩子函数
routersyhk.beforeEach((to, from) => {
    //    return 用来操作路由接下来的跳转
    // if(登录失败){
    //     return '/home';//返回首页
    // }
})
//暴露出去
// export default routersyhk
// to 参数：即将要进入的路由对象   from 当前导航正要离开的路由


// beforeResolve
// router.beforeResolve(async to => {
// 如果路由配置了必须调用相机权限
//   if ( to.meta.requiresCamera ) {
// 正常流程，咨询是否允许使用照相机
//     try {
//       await askForCameraPermission()
//     }
// 容错
//     catch (error) {
//       if ( error instanceof NotAllowedError ) {
// ... 处理错误，然后取消导航
//         return false
//       } else {
// 如果出现意外，则取消导航并抛出错误
//         throw error
//       }
//     }
//   }
// })

// afterEach 
// to 即将要进入的路由对象 from 当前导航正要离开的路由
router.afterEach((to, from) => {
    // ....
})

// beforeEnter 路由独享前置守卫，在跳转跳转前触发它和 beforeEach 的作用相同
// 顺序 beforeEach（全局） > beforeEnter（独享） > beforeResolve（全局）
// 参数和 beforeEach 一样也是取消了 next, 通过 return 来代替
const routes6: Array<RouteRecordRaw> = [
    {
        path: '/home',
        name: 'home',
        component: () => import('@views/test4.vue'),
        // 在这里添加单独的路由守卫 
        beforeEnter: (to, from) => {
            document.title = 'syhk';
        }
    }
]

// onBeforeRouteUpdate 组件内的更新守卫：在当前路由改变，但是该组件被复用时调用
// 参数 to from




// onBeforeRouteLeave 组件内的离开守卫：导航离开该组件的对应路由时调用
// 参数 to from
// 当用户离开时，需要保存才能离开
// 调用离开守卫
onBeforeRouteLeave((to, from) => {
    // 弹出一个确认框
    const CONFIRM_TEXT: string = '确认要离开吗？您的更改尚未保存！';
    const IS_CONFIRM_LEAVE: boolean = window.confirm(CONFIRM_TEXT);

    // 当用户点取消时，不离开路由
    if (!IS_CONFIRM_LEAVE) {
        return false
    }
})
```

router 下的 index.ts 内容：

```typescript
import { createRouter, createWebHashHistory, RouteRecordRaw } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import test3Vue from '../views/test3.vue'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  },
  {
    // path 是路由访问的路径
    path: '/test1',
    // name 是路由的名称，非必填 
    name: 'test1',
    // component 是路由的模板文件，指向一个 vue 组件
    // 推荐使用这种方式：可以实现路由懒加载，也就是它接收一个函数，在 return 的时候返回模板组件 
    component: () => import('../views/test1.vue')
  },
  /**
   * vue 多级路由
   * eg:https://chengpeiquan.com/chinese-food/dumplings
   * chinese-food 是一级路由， dumplings 是二级路由，依次下去几层就是几级路由
   * 父子路由的关系：子路由信息配置到父级的 children 数组里面，孙路由也是一样的，配置到子路由的 children
   * 
   * 路由懒加载：就是当路由被访问的时候才加载对应的组件，按需载入
   * 
   */


  { //一级路由
    path: '/test2',
    name: 'test2',
    component: () => import('../views/test2.vue'),
    // 这里是二级路由
    children: [
      {
        path: '/test3',
        name: 'test3',
        component: () => import('../views/test3.vue'),
        // 这里是三级路由
        children: [
          {//访问它就是： .../test2/test3/test4
            path: '/test4',
            name: 'test4',
            component: () => import('../views/test4.vue')
          }
        ]
      }
    ]
  },
  {
    path: '/test3',
    name: 'test3',
    component: () => import('../views/test3.vue')
  },
  {
    path: '/test4',
    name: 'test4',
    component: () => import('../views/test4.vue')
  },
  {
    path: '/test5',
    name: 'test5',
    component: () => import('../views/test5.vue')
  },
  {
    path: '/test6',
    name: 'test6',
    component: () => import('../views/test6.vue')
  },
  {
    path: '/test7',
    name: 'test7',
    component: () => import('../views/test7.vue')
  },
  {
    path: '/test8',
    name: 'test8',
    component: () => import('../views/test8.vue')
  },
  {
    path: '/Child',
    name: 'Child',
    component: () => import('../views/Child.vue')
  },
  {
    path: '/Father',
    name: 'Father',
    component: () => import('../views/Father.vue')
  },
  {
    path: '/stroetest',
    name: 'stroetest',
    component: () => import('../stores/test.vue')
  }

]

const router = createRouter({
  history: createWebHashHistory(),
  routes
})

export default router
```

stores 下 index.ts 内容：

```typescript
// import { createStore } from 'vuex'
// // ============Vuex===============
// // 这个是 vuex 的入口文件

// // Pinia 被官方推荐在 vue3 项目里作为全局状态管理的新工具
// // 安装 npm install pinia

// export default createStore({
//   state: {
//   },
//   getters: {
//   },
//   mutations: {
//   },
//   actions: {
//   },
//   modules: {
//   }
// })

// Pinia 形式1
import {defineStore} from 'pinia'

export const userStore=defineStore('main',{
  // Store 选项
  // 定义一个基本的 message 数据
  // state:()=>({
  //   message:'Hello World',
  // }),
// 注意:不显式 return ,箭头函数的返回值需要用圆括号套起来
// 显式
state:()=>{
return{
  message:'Hello World',
  // 通过 as 关键字指定 TS 类型
  randomMessages:[] as string[],
// 或者用 <> 括号来指定
  randomMessages1:<string[]>[],

}
}

})

// 形式2
// export const userStore=defineStore({
//   id:'main',
//   // Store 选项
// })

// 不论哪种形式,都必须为 Store 指定一个唯一ID
```

stores 下的 test.vue 内容：

```vue
<template>
  <p>{{ store.message }}</p>
  <p>{{ store.randomMessages1 }}</p>
  <p>{{ messagess1 }}</p>
</template>

<script lang="ts">
import { storeToRefs } from "pinia";
import { computed, defineComponent, toRef, toRefs } from "vue";
import { userStore } from ".";

export default defineComponent({
  setup() {
    const store = userStore();

    // 可以通过实例直接来拿到数据
    console.log(store.message);

    // 更新数据直接写
    store.message = "syhk";

    // 使用  computed API
    // 如果要修改就要配置好 setter 的行为
    const messagess1 = computed({
      // getter
      get: () => store.message,
      // setter
      set(newValue) {
        store.message = newValue;
      },
    });
    // 这个时候就可以更改了
    messagess1.value = "ssssssssss";
    console.log("messagess1", messagess1.value);
    // messagess1='he'; 它是只读的 messagess1 变量是一个只有 getter ,没有 setter 的 ComputedRef 数据

    // storeToRefs API
    // Pinia 还提供了一个 storeToRefs API 用于把 state 的数据转换为 ref 变量。
    // const {msg2}=storeToRefs(store);
    // console.log("msg2",msg2.value)
    // 用 vue 的API 来处理
    const { message } = toRefs(store);
    console.log("message", message.value);
    // toRef 和 toRefs 一个转换一个字段,一个转换所有字段
    // const {message1} =toRef(store,'message')

    return {
      store, //返回给模板渲染数据
      messagess1,
    };
  },
});
</script>
```

lib 下三个文件内容：

bus.ts:

```typescript
// ============== 全局组件通信================
// EventBus
// 需要先安装 mitt  npm install --save mitt
// 在 lib 文件夹下创建一个  bus.ts 文件

import mitt from 'mitt';
export default mitt();
```

t1.vue

```vue
<template></template>

<script lang="ts">
import { defineComponent, onBeforeMount } from "vue";
import bus from "./bus";

// 常用api
// on 注册一个监听事件,用于接收数据  参数: type 方法名  handler 类型为函数  接收到数据之后要做什么处理的回调函数
// handler 建议使用具名函数,匿名函数无法销毁
// emit 调用方法发起数据传递  参数: type 与 on对应的方法名  data 与on对应的允许接收的数据
// off 用来移除监听事件  参数: type 与on对应的方法名  handler 函数类型 要删除的与on对应的 handler 函数名

// eg:
export default defineComponent({
  setup() {
    // 定义一个打招呼的方法
    const sayHi = (msg: string = "Hello World"): void => {
      console.log(msg);
    };

    // 启用监听
    bus.on("sayHi", sayHi);

    // 在组件卸载之前移除监听
    onBeforeMount(() => {
      bus.off("sayHi", sayHi);
    });
  },
});
</script>
```

t2.vue

```vue
<template></template>

<script lang="ts">
import { defineComponent } from "vue";
import bus from "./bus";
// 调用监听事件

export default defineComponent({
  setup() {
    // 调用
    bus.emit("sayHi", "my name is syhk");
  },
});
</script>
```

#### vue正确引入  element-plus
图标错误可以下载：
引入  element-plus 修改过的地方
参照这个博客，自己也修改了一些：
tsconfig.json:
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "strict": true,
    "jsx": "preserve",
    "sourceMap": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "lib": ["ESNext", "DOM"],
    "skipLibCheck": true,
    // 指定全局组件类型
    "types": ["element-plus/global"]
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.d.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "src/plugins/elementPlus.js"
  ],
  "references": [{ "path": "./tsconfig.node.json" }]
}

```
vite.config.ts:
```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
// ===============================
import AutoImport from 'unplugin-vue-components/vite'
import Components from 'unplugin-auto-import/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
// 下载了上面这两个插件 按需引入 
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()
    ,
  AutoImport({
    resolvers: [ElementPlusResolver()],
  }),
  Components({
    resolvers: [ElementPlusResolver()],
  }),
  ]
})

```
main.ts
```typescript
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
// 上面这两个也需要导入
// ==================
import zhCn from 'element-plus/es/locale/lang/zh-cn'
import * as ElementPlusIconsVue from '@element-plus/icons-vue'


const app = createApp(App)
// 全局注册 el-icon
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
    app.component(key, component)
}

app.use(ElementPlus, { locale: zhCn }).use(router).mount("#app")

// createApp(App).use(router)
//     .use(ElementPlus)
//     .mount('#app')


```
#### 前台解决跨域问题








### Pinia

```typescript
 import { createApp } from 'vue'
 import App from './App.vue'
 import router from './router'
// import store from './stores'
 import {createPinia} from 'pinia' //导入 Pinia
// Pinia 被官方推荐在 vue3 项目里作为全局状态管理的新工具
// 安装 npm install pinia

 createApp(App).
 use(createPinia()) //启用 Pinia
.use(router).mount('#app')
```

计算属性与 watch 的区别

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
#### JSON  和 querystring 的区别
```html
// config.data = querystring.stringify(config.data);
config.data = JSON.stringify(config.data);

// qs 和 Json 都是序列化  eg{name:'syhk', age:29}
// qs 结果： name=syhk&age=29
//  JSON 结果： {"name":"syhk","age":"20"}
```

#### vue 初始化样式
在 app.vue 样式中编写
```vue
#app {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```





















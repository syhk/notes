![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1665313649264-36a513fe-c97f-46b9-a236-bf178aecc4b2.png#clientId=u878e980b-415d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=59&id=uc1722d91&margin=%5Bobject%20Object%5D&name=image.png&originHeight=74&originWidth=372&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4256&status=done&style=none&taskId=uba34f5af-c8dd-474c-a3b7-cf979188668&title=&width=297.6)
user.ts
```typescript
import { defineStore } from 'pinia'

//  main 是 store 的唯一 id（第一个参数）
export const useUsersStore = defineStore('main', {
	// 这的其他配置项
	// 第二参数 options 是一个对象 k-v
	// 有一个 state 属性，它返回一个对象
	state: () => {
		return {
			name: String("syhk"),
			age: Number(28),
			sex: String("man"),

		}
	},
	// getters 属性 是一个对象；它里面是各种各样的方法（可以把它想象成 vue 中的计算属性） 
	getters: {
		getAddage: (state) => {
			// return state.age + 100;
			return (num: number) => state.age + num;
		},
		//  用箭头函数的话，需要知道 this 是指向哪里的，
		// 箭头函数里面不能使用 this
		getNameAndAge(): string {
			return this.name + this.getAddage; //调用其它的 getters
		}
	},
	// 相当于vue3 中 methods
	actions: {

		saveName(name: string) {
			this.name = name;
		}
	}
});
```
main.ts
```typescript
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
// ==================
import zhCn from 'element-plus/es/locale/lang/zh-cn'
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
// 引入 pinia
import { createPinia } from 'pinia'



const pinia = createPinia();

const app = createApp(App)
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
    app.component(key, component)
}

app.use(ElementPlus, { locale: zhCn }).use(router).use(pinia).mount("#app")


```
app.vue
```vue
<template>
  <router-view></router-view>
  <p>拿到的值为：{{ name + age + sex }}</p>
  <button @click="changeName">change</button>
  

  <button @click="reset">reset</button>
  

  <button @click="pathchStore">批量更改</button>

  <child></child>
</template>
<script setup lang="ts">
import { ref } from "vue";
import { useUsersStore } from "./store/user";
import Child from "./views/child.vue";
import { storeToRefs } from "pinia";

const store = useUsersStore();
console.log(store);
// 解构方式 非响应式
// const { name, age, sex } = store;
// 变为响应式
const { name, age, sex } = storeToRefs(store);
// 如果是在代码中使用的话，就可以不需要变成响应式的
//  这里更改后，子组件也不是响应式的
// const name = ref<string>(store.name);
// const age = ref<number>(store.age);
// const sex = ref<string>(store.sex);

// 修改 store 中的数据
const changeName = () => {
  // 这里是非响应式的
  store.name = "zhangsan";
  console.log(store.name);
};

// pinia 提供了一个方法，可以重置数据的内容 （重置按钮）
const reset = () => {
  store.$reset(); //重置 store
};

// 批量更改
const pathchStore = () => {
  // 只需要传递想更改的数据即可，也可以不需要传递所有数据
  store.$patch({
    name: "ssss",
    age: 1000,
    sex: "woman",
  });
};
```
Child.vue
```vue
<template>
  <p>我是 child</p>
  <p>拿到的值为：{{ name + age + sex }}</p>
  <!-- 使用 getters 属性 -->
  <p>新的年龄是： {{ store.getAddage(10000) }}</p>
  <!-- 直接在 html 中使用的话是响应式的 -->
  <p>名字是： {{ store.name }}</p>
  <p>调用其他的 getters {{ store.getNameAndAge }}</p>
  <button @click="savename">save</button>
</template>
<script setup lang="ts">
import { useUsersStore } from "../store/user";
import { storeToRefs } from "pinia";
const store = useUsersStore();
console.log(store);
// 解构方式
// const { name, age, sex } = store;
//  子组件也需要像这样变成响应式的
const { name, age, sex } = storeToRefs(store);

const savename = () => {
  store.saveName("what's why?");
};
</script>

```

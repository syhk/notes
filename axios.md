


github地址：


> Axios 是一个基于 [_promise_](https://javascript.info/promise-basics) 网络请求库，作用于[node.js](https://nodejs.org/) 和浏览器中。 它是 [_isomorphic_](https://www.lullabot.com/articles/what-is-an-isomorphic-application) 的(即同一套代码可以运行在浏览器和node.js中)。在服务端它使用原生 node.js `http` 模块, 而在客户端 (浏览端) 则使用 XMLHttpRequests。


安装  axios

`npm install axios`

`yarn add axios`

`vue add axios`

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

默认使用 get 方式请求

```javascript
<!-- 在 vue.js 之后引入 -->
<script src="https://unpkg.com/vue"></script>
<script src="https://unpkg.com/vue-axios-plugin"></script>
```

```vue
<script lang="ts" setup>
import { reactive, ref } from "vue";
import axios from "axios";
const user = reactive({
  username: String(""),
  password: String(""),
});


// 拿到前台数据
// 利用 axios 发起请求
const result = axios({
  method: "GET",
  url: "http://localhost:8888",
  params: {
    //URL 中的查询参数
    username: user.username,
    password: user.password,
  },
  data: {}, //请求体参数
});

// 发起 post 请求
axios({
  method: "POST",
  url: "http://localhost:8888",
  data: {
    user,
  },
});
// 也可以像这样
axios
  .post("url", {
    username: user.username,
    password: user.password,
  })
  .then(function (response) {
    // 成功做的事
    console.log(response);
  })
  .catch((error) => console.log(error));
// get 请求
axios
  .get("url", {
    params: {
      username: user.username,
      password: user.password,
    },
  })
  .then((response) => console.log(response))
  .catch((error) => console.log(error));

// 也可以执行多个并发请求
function getUserAccount() {
  return axios.get("/user/12345");
}
function getUserPermissions() {
  return axios.get("/user/12345/permissions");
}
axios
  .all([getUserAccount(), getUserPermissions()])
  .then(axios.spread((acct, perms) => console.log("两个请求现在都执行")));


</script>

<!-- vue-axios  对 axios 进行轻度封装-->
<!-- 安装  npm install --save axios vue-axios -->
<!-- 
import VueAxios from 'vue-axios'
import axios from 'axios'
createApp(App).use(router).use(VueAxios,axios).mount('#app')
 -->
```

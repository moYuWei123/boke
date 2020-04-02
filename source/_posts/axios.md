---
title: axios
date: 2019-3-23 19:02:09
tags: 基础
categories: Vue
---
# axios

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。 类似jq的$.ajax
axios是一个已经封装好的ajax 的一个库

## axios特性

+ 从浏览器中创建 `XMLHttpRequests`
+ 从 `node.js` 创建 `http` 请求
+ 支持 `Promise` API
+ 拦截请求和响应
+ 转换请求数据和响应数据
+ 取消请求
+ 自动转换 JSON 数据
+ 客户端支持防御 `XSRF`(跨站请求伪造)


### axios安装  vue-resource 尤雨溪 不再维护  推荐使用axios

使用 npm:

``` html
npm install axios --save

import axios from "axios"

```
使用 cdn:
``` html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```
### axios中文文档(非官方)
https://www.kancloud.cn/yunye/axios/234845

### axios 官方文档地址(英文)
https://github.com/axios/axios

### axios返回的数据
+ config 请求的时候附带的配置参数
+ data 后端返回的数据
+ headers  请求头 里面包含发送给后端的格式 `application/json;charset=UTF-8`
+ request  ajax请求
+ status  返回的状态码
+ statusText  返回的状态文字
### 电影接口  豆瓣电影图片做防盗链处理 需要加上这个<meta name="referrer" content="never">
+ 服务器地址 
http://59.110.138.169

http://114.55.243.6:8080

+ 豆瓣电影接口
https://apizza.net/pro/#/project/219bd7da9f36824337b05a33e9e08c51/browse?pass=a511db3cf920386c952dd6bdfbcf42fd

### 常用get/post请求

[ 电影接口]("http://www.bufanui.com/designInfo/detail/6fd1d4a4ccb44c878ae72273bd3dca08" "不凡电影接口")
+ get请求 豆瓣电影接口
  - get请求不带参数
``` html
<script>
import axios from "axios";
export default {
  methods: {
    init(){
      axios.get('http://www.bufantec.com/api/douban/movie/in_theaters')
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  },
  created () {
    this.init();
  }
}
</script>
```
![请求结果](./imgs/img1.jpg "请求结果")
   - get请求附带参数
``` html
<script>
import axios from "axios";
export default {
  methods: {
    init(){
      axios.get('http://www.bufantec.com/api/douban/movie/in_theaters?start=1&limit=10')
      // get请求附带参数的另一种写法
      /* axios.get('http://www.bufantec.com/api/douban/movie/in_theaters',{
        params:{
          start:2,
          limit:10
        }
      }) */
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  },
  created () {
    this.init();
  }
}
</script>
```
![请求结果](./imgs/img2.jpg "请求结果")
+ post请求带参数
```html
<script>
import axios from "axios";
let datas = {
        Mob:18311111111,
        validcode:"815961",
        use:"regiVali"
      };
export default {
  methods: {
    init(){
      axios.post('http://order.gjw.com/Order_Api/GetValiCode',datas )
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  },
  created () {
    this.init();
  }
}
</script>
```
![post请求](./imgs/img3.jpg "post请求")
### post请求注意事项 

axios 使用 post 发送数据时，默认是直接把传递的参数转换成 json 放到请求体中提交到后端的。也就是说，我们的 Content-Type 变成了 application/json;charset=utf-8 ,这是axios默认的请求头content-type类型。但是实际我们后端要求的 'Content-Type': 'application/x-www-form-urlencoded' 为多见，这就与我们不符合。
在HTTP协议消息头中，使用Content-Type来表示请求和响应中的媒体类型信息。它用来告诉服务端如何处理请求的数据，以及告诉客户端（一般是浏览器）如何解析响应的数据，比如显示图片，解析并展示html等等。
**post请求常见的数据格式（content-type）**

1. `Content-Type: application/json` ： 请求体中的数据会以json字符串的形式发送到后端(axios默认的) 会导致后端接收不到我们发送给后端的数据

2. `Content-Type: application/x-www-form-urlencoded`：请求体中的数据会以普通表单形式（键值对）发送到后端

解决方法
+ URLSearchParams   // 不支持ie
``` html
<script>
import axios from "axios";
let param = new URLSearchParams()
param.append('username', 'admin')
param.append('pwd', 'admin')
export default {
  methods: {
    init(){
      axios.post('http://localhost:3000/user',param )
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  },
  created () {
    this.init();
  }
}
</script>
```
+ qs模块 qs模块在node中默认自带
``` html
<script>
import axios from "axios";
import qs from "qs";
export default {
  name: '',
  data() { 
    return {

    }
  },
  methods: {
      add(){
          // post
          // post请求如果附带参数 那么需要附带的参数用qs.stringify转一下
        var obj = {
            name:"张飞1",
            age:10,
            sex:1,
            tel:123,
            address:"不凡1",
            school:"不凡1",
        }
        axios.post("http://59.110.138.169/api/ajax/user/save",qs.stringify(obj)
        )
      }
  }
 }
</script>
```
### axios执行多个并发请求

一次执行多次ajax请求
promise.all
执行多个并发请求是通过`axios.all`完成  
`axios.all([function , function , ...])`
请求完成之后 会以数组的形式返回全部的请求数据
``` html
<script>
import axios from "axios";
axios.defaults.baseURL = 'http://localhost:3000';
import qs from "qs";
let datas = {
        Mob:18311111111,
        validcode:"815961",
        use:"regiVali"
      };
let params = qs.stringify(datas)
export default {
  methods: {
    init(){
      return axios.get('http://localhost:3000/')
     
    },
    getUserInfo(){
       return axios.post('http://localhost:3000/user',params )
      
    }
  },
  created () {
    axios.all([this.init(), this.getUserInfo()])
    .then((val)=>{  // val返回的是全部的请求
        console.log(val)
    });
  }
}
</script>
```

### 全局的 axios 默认值

+ `axios.defaults.baseURL` 设置默认公共请求地址baseURL
当我们的项目里面有很多的接口时 设置默认baseURL对后期的维护很有必要 
例如 开发环境访问的是开发时的数据库 生产环境访问的是线上数据库 开发和生产访问的不是一个地址
在请求的时候自动的在地址前面加上设置的baseUrl
(开发环境是本地开发时的环境 生产环境是打包之后的环境)
``` html
<script>
if(process.env.NODE_ENV == "development"){
    axios.defaults.baseURL = 'http://localhost:3000';
}else{
    axios.defaults.baseURL = 'http://localhost:3001';
}
</script>
```




+ `axios.defaults.timeout = 2500`; 设置超时

  当后端返回数据过慢时设置了超时就会自动断开请求

``` html
<template>
  <div class="home">
    
  </div>
</template>

<script>


import axios from "axios";
axios.defaults.timeout = 3000;
export default {
  created () {
    axios.get("http://localhost:3000/")
    .then(res =>{
      console.log(res)
    })
  }
}
</script>

```
+ `axios.defaults.headers.common['token'] = token`;
token是登陆的时候返回的token 如果后端让在别的页面请求数据的是附带token 
一般用户判断用户是否登陆是根据token 进行判断的  一般把这个token存在session里面
只要把toke写在请求头里面 后端就知道我们登陆 
`axios.defaults.headers.common['token'] = localStorage["token"]  `
登陆的时候保存sessionkey  然后在请求其他接口的时候带上这个sessionkey  sessionkey  代表用户的身份 当我们请求数据的时候带上这个sessionkey  那么后端就知道现在是谁在请求数据 
还有些特殊的情况 登陆的时候后端会在本地设置cookie token 他会根据cookie里面的值token来判断用户是否登陆  

axios.defaults.headers.common['sessionKey'] = 登陆时后端返回的sessionKey
如果你每次请求接口需要验证，就加这个，不需要验证那就不用加
![用户身份鉴权](./imgs/session.jpg "用户身份鉴权")
``` html

<template>
  <div class="home">
    <button @click="getUserInfo">获取个人信息</button>
  </div>
</template>

<script>


import axios from "axios";
// axios.defaults.baseURL = "https:bufantest.com";
axios.defaults.timeout = 3000;


export default {
  data () {
    return {
      key:""
    }
  },
  methods: {
    getUserInfo(){
      if(localStorage.getItem("sessionKey")){
        var key = JSON.parse(localStorage.getItem("sessionKey"));
        axios.defaults.headers.common['user-session'] = key
      }
      axios.post("http://114.55.243.6:8080/user/getInfo")
      .then(res =>{
        console.log(res)
      })
    }
  },
  created () {
    // 登陆
    axios.post("http://114.55.243.6:8080/user/login",{
      loginType:0,
      mobile:18311111111,
      pwd:1,
      verifyCode:"",
    })
    .then(res =>{
      localStorage.setItem("sessionKey",JSON.stringify(res.data.data.sessionKey))
    })
  }
}
</script>
```
### axios拦截器 interceptors  

页面发送http请求，很多情况我们要对请求和其响应进行特定的处理；如果请求数非常多，单独对每一个请求进行处理会变得非常麻烦，程序的优雅性也会大打折扣。好在强大的axios为开发者提供了这样一个API：拦截器。拦截器分为 请求（request）拦截器和 响应（response）拦截器。
> 前端请求接口时首先向服务端发送请求的接口加参数 这个步骤称之为request
request 对象代表了一个HTTP请求，其具有一些属性来保存请求中的一些数据，比如params string，body，HTTP headers等等。
+ params get请求附带的参数
+ body post请求附带的参数
+ HTTP headers 提交数据类型

> 服务端接收到请求之后响应数据 这个步骤称之为response
response里面存放的就是服务端返回给我们的数据，包括状态码，返回的数据格式等等

axios拦截器就是对这请求前和返回数据后的这两个过程执行操作

### axios拦截器 开始 类似路由守卫
+ elementui安装
  - 安装elementui cnpm install element-ui --save
  - 注册element
```html
<script>
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI);
</script>

```
  
+ 请求拦截器
  - config里面包含请求的参数 如请求地址 请求类似 请求参数等
在请求的时候执行一些操作 比如开启loading弹窗 可以想象成router的导航守卫
```html
<script>
axios.interceptors.request.use(function (config) {
    // 在发起请求请做一些业务处理  
    // 如开启loading  对请求的参数做处理 添加token等
    // 例  在请求的时候开启elementui的loading
  loadings = Loading.service({ fullscreen: true });
  if(config.method == "post"){
      config.data = qs.stringify(config.data);
  }
  
  console.log(config)
    return config;
}, function (error) {
    // 对请求失败做处理
    return Promise.reject(error);
});
</script>

```
+ 响应拦截器
比如响应数据之后关闭loading弹窗 
```html
<script>
axios.interceptors.response.use(function (config) {
    // 在请求之后做处理 如关闭loading
  loadings.close();
  // 对返回过来的数据进行过滤操作
    return config.data;
}, function (error) {
    // 对请求失败做处理
    loadings.close();
    return Promise.reject(error);
});
</script>
```

### 封装常用的get/post请求
> 前言
get请求和post请求传参的写法
+ get请求传参
``` html
<script>
    axios.get('/user?ID=12345')
    axios.get('/user', {
        params: {
            ID: 12345
        }
    })
</script>

```
+ post请求传参的方式
```html
<script>
    axios.post('/user', {
        ID: 12345
    })
</script>

```
在src目录新建文件夹api
在api文件夹新建index.js
``` html
<script>
import axios from 'axios'
import { Loading } from 'element-ui'
import qs from "qs"

const service = axios.create({
  baseURL: "http://localhost:3000", // api的base_url
  timeout: 5000 // request timeout
});
let loadings;

// request interceptor
service.interceptors.request.use(config => {
//   var token = sessionStorage.getItem("token")
//   if (token) {
//     // 让每个请求携带token-- ['X-Token']为自定义key 请根据实际情况自行修改
//     config.headers['X-Token'] = token
//   }
  if(config.method == "post"){
    // config.headers['Content-Type'] = 'application/x-www-form-urlencoded';
    config.data = qs.stringify(config.data)
  }
  loadings = Loading.service({ fullscreen: true });
  return config
}, error => {
  
  console.log(error)
  Promise.reject(error)
})

// respone interceptor
service.interceptors.response.use(
  response => {
    loadings.close();
    return response
  },
  /**
   * 下面的注释为通过在response里，自定义code来标示请求状态
   * 当code返回如下情况则说明权限有问题，登出并返回到登录页
   * 如想通过xmlhttprequest来状态码标识 逻辑可写在下面error中
   * 以下代码均为样例，请结合自生需求加以修改，若不需要，则可删除
   */
  // response => {
  //   const res = response.data
  //   if (res.code !== 20000) {
  //     Message({
  //       message: res.message,
  //       type: 'error',
  //       duration: 5 * 1000
  //     })
  //     // 50008:非法的token; 50012:其他客户端登录了;  50014:Token 过期了;
  //     if (res.code === 50008 || res.code === 50012 || res.code === 50014) {
  //       // 请自行在引入 MessageBox
  //       // import { Message, MessageBox } from 'element-ui'
  //       MessageBox.confirm('你已被登出，可以取消继续留在该页面，或者重新登录', '确定登出', {
  //         confirmButtonText: '重新登录',
  //         cancelButtonText: '取消',
  //         type: 'warning'
  //       }).then(() => {
  //           location.reload() // 为了重新实例化vue-router对象 避免bug
  //       })
  //     }
  //     return Promise.reject('error')
  //   } else {
  //     return response.data
  //   }
  // },
  error => {
    console.log('err' + error) // for debug
    loadings.close();
    return Promise.reject(error)
  })

export default service
</script>
```
调用的时候先引入`api/index.js`
也可以把它挂载到vue原型上
``` html
<script>
    import api from "@/api"
    Vue.prototype.$ajax = api;
</script>

```
调用的时候执行
```html
<script>
this.$ajax.post('/user',datas  ).then(res=>console.log(res))
</script>
```










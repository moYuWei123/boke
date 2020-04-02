---
title: router
date: 2018-6-14 11:24:43
tags: 基础
categories: Vue
---
## vue路由
路由简单的理解 可以理解为浏览器地址栏的变化过程
### 路由简介

+ 传统的路由指的是：当用户访问一个url时，对应的服务器会接收这个请求，然后解析url中的路径，从而执行对应的处理逻辑。这样就完成了一次路由分发。
通俗理解 : 比如向浏览器地址栏输入地址 对应服务器根据这个地址返回返回数据(html css js等) 输入地址就是请求服务器的资源 这一输入地址返回对应数据叫做传统路由
传统路由向服务器请求数据 请求数据需要刷新页面
ajax局部刷新
+ 而前端路由是不涉及服务器的，是前端利用hash或者HTML5的history API来模拟实现的，一般用于不同内容的展示和切换(利用ajax来实现资源的更新)。
前端路由请求页面的时候不会向服务器请求数据 页面是由js创建完成的 router.js
+ 目前Vue 推荐单页面应用 SPA 开发模式，大型单页应用最显著特点之一就是采用前端路由系统，通过改变URL，在不重新请求页面的情况下，更新页面视图。Vue中的路由解决方案为vue-router。
+ 前端路由页面间的切换是组件之间的切换 不涉及服务器
> Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。我们用vue-cli开发的项目就是单页面应用。
### vue路由能实现哪些功能
+ 嵌套的路由/视图表
+ 模块化的、基于组件的路由配置
+ 路由参数、查询、通配符
+ 基于 Vue.js 过渡系统的视图过渡效果

+ 细粒度的导航控制
+ 带有自动激活的 CSS class 的链接
+ HTML5 历史模式或 hash 模式，在 IE9 中自动降级
+ 自定义的滚动条行为
### 路由安装 (我们在vue-cli创建项目的时候就默认安装路由)

### npm安装vue路由
> 这一步是安装路由 保存到我们package.json 里面的dependencies里面
```
cnpm install vue-router --save
```
### 路由起步

> 配置路由文件
1. 在main.js的同级目录新建一个router的文件夹(这个文件夹存放的是我们路由的配置文件)
2. 在这个文件夹下创建index.js 里面写上
```
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)
// 上面三个是引入vue-router(你安装路由之后不引入相当于做无用功)
export default new Router({  // 把我们的路由配置文件暴露出去
  mode: 'history', 
  // vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。如果不想/// 要很丑的 hash，我们可以用路由的 history 模式
  routes: [  // 这个存放的是我们路由的配置文件
    
  ]
})

```
3. 在view目录下新建两个文件夹 一个是index 一个是mine 这个文件夹代表两个页面(这个一个规范)  一个是index页面 一个是mine页面 分别在mine和index下面新建index.vue (创建我们的组件) 里面分别写上内容 '首页' 'mine页'
![项目结构]("https://raw.githubusercontent.com/208895638/teachVue/master/%E6%88%AA%E5%9B%BE/vue%E7%9B%AE%E5%BD%95%E6%88%AA%E5%9B%BE.jpg" "项目结构")
4. 在我们的router文件夹下的index.js里面引入我们的组件
```
import index from "@/views/index/index" // 引入index组件
import mine from "@/views/mine/index" // 引入mine组件
```
5. 配置路由

```
export default new Router({
  mode: 'history',  // 启用history路由
  routes: [ // 这里面是路由的配置项
    {
      path: '/',  // 这个是我们访问浏览器的地址
      name: 'index',  // 这个是我们给路由起的名称
      component: index  // 这个是 地址对应的组件
    },
    {
      path: '/mine', // 这个是我们访问浏览器的地址
      name: 'mine',  // 这个是我们给路由起的名称
      component: mine  // 这个是 地址对应的组件
    }
  ]
})
```
6. 上一步就代表路由配置完成 我们需要把我们的路由配置挂载到我们的vue实例上 在main.js里面操作
```
import Vue from 'vue'
import App from './App.vue'
import router from './router/index'  // 引入我们刚刚配置的路由文件
Vue.config.productionTip = false
new Vue({
  router, // 把路由挂载到vue实例上
  render: h => h(App)
}).$mount('#app')
```
7. 最后一步 在我们的app.vue里面加一个组件router-view
```
<router-link to="/">跳转到首页</router-link>
<router-link :to="{path:'/'}">跳转到首页</router-link>
    <router-link to="/mine">跳转到mine页</router-link>

    <router-view></router-view>
```
router-view这个组件是一个容器 我们页面的路由所对应的组件都渲染在这个容器里面
router-link是在vue里面做跳转链接用的 vue会把router-link渲染成a标签
8. 好了 一个最基本的路由完成 
### 自动激活的 CSS class 的链接 当router-link 里面的to地址与地址栏中的路由匹配一样时就自动激活router-link-active
如
当前路由为 /home 而<router-link to="/home" exact></router-link> router-link-active这个class名称就会自动被激活
> 自定义激活class名称  linkActiveClass
```
export default new Router({
  mode: 'history',  // 启用history路由
  linkActiveClass:"r-active",  // 这个就是设置激活是的class名称
  routes: [ // 这里面是路由的配置项
    {
      path: '/',  // 这个是我们访问浏览器的地址
      name: 'index',  // 这个是我们给路由起的名称
      component: index  // 这个是 地址对应的组件
    },
  ]
})

```

###  路由传递参数 
+ 动态路由
<!-- /a
/b
/c
home -->
 简介 我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在 vue-router 的路由路径中使用“动态路径参数”(dynamic segment) 来达到这个效果：
+ 配置动态路由
```
{
    path : "/mine/:id", // 动态路由
    name : "mine" ,
    component: mine
}


```
使用的时候 会自动匹配1，2，3，4 到mine组件
```
<router-link to="/mine/1">跳转到mine页</router-link>
<router-link to="/mine/2">跳转到mine页</router-link>
<router-link to="/mine/3">跳转到mine页</router-link>
<router-link to="/mine/4">跳转到mine页</router-link>
<router-link :to="{name:'mine',params:{id:1}}">跳转到mine页</router-link>
这里需要注意的一点就是 当路由是动态路由的时候我们得用命名路由的方式跳转
```

+ query传值

直接在路由后面传递需要传递的参数
获取这个参数 可以使用 this.$route.query $route 当前路由对象
```
<router-link to="/user?username='张三'&age='40'&sex='男'" tag="h2">跳转到user页</router-link>
```
### 嵌套路由 
> 简介 实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL 中各：
```段动态路径也按某种结构对应嵌套的各层组件，例如

/user/profile                     /user/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```
> 嵌套路由的写法
```
const router = new VueRouter({
  routes: [
    { path: '/user', component: User,redirect:"/user/profile"
      children: [
        {
          // 当 /user/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```
### 编程式的导航 除了使用 <router-link> 创建 a 标签来定义导航链接 我们还可以用js的方法跳转页面
```
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

```
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```
### 命名路由
> 简介
有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。可以在创建 Router 实例的时候，在 routes 配置中给某个路由设置名称。
```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
要链接到一个命名路由，可以给 router-link 的 to 属性传一个对象：
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
router.push({ name: 'user', params: { userId: 123 }})
```
### 重定向
> 重定向  把地址重定向到某个路由 重定向”的意思是，当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b
```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```
### html5 History模式

> 简介
vue-router 默认 hash 模式(特点 #/) —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用 history.pushState API 来完成 URL 跳转而无须重新加载页面。
当使用 history 模式时，URL 就像正常的 url，例如 http://yoursite.com/user/id，也好看！
不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 http://oursite.com/user/id 就会返回 404。

https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%BE%8B%E5%AD%90
> 解决方法
在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。


---
title: vue常用ui框架
date: 2019-5-27 15:22:59
tags: 基础
categories: Vue
---
# vue 

## vue常用ui框架
pc端`element`
移动端 `vant cube`
## 为什么使用ui框架
使用别人封装好的框架 可以大大的节省开发时间 提升开发效率 
缺点也很明显 不能定制需要 有时候与设计图差异大
当对样式没有很大需求的时候(后台管理系统)
### pc端ui框架 element
Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库。
它的开发团队是饿了么。
### element安装
`vue-cli3.x`安装参考下面地址
[vue3.x安装方法](https://github.com/ElementUI/vue-cli-plugin-elemen2t "vue3.x安装方法")


### npm 安装element ui框架  

+ `cnpm install  element-ui --save` 等价 `npm i element-ui -S`

+ 

### vue-cli的项目引入element的两种方式
`element-ui`安装后提供两种引入方式，全局引入以及按需引入，整个element-ui大概有1M左右，如果我们只需要几个组件(例如button)，这个时候全局引入就有些不合适了,按需引入可以很好的减少打包后的体积。

+ 全局引入 推荐
  把ui框架一次性的导入 
  - 好处 全部导入 节省了引用组件时的代码
  - 坏处 全部导入 有的组件用不到 就增加了项目的体积 

```html 
<!-- 全局引用的方式 -->
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```
+ 按需引入  

  把需要的组件引入 不需要的就不导入 打包项目的打包仅仅是引用的组件 大大减少了打包时的体积 
  坏处 每次引入的时候需要写一些额外的代码  

`npm install babel-plugin-component -D`

在babel.config.js里面加下面一段就可以
[按需引入参考](https://element.eleme.cn/2.10/#/zh-CN/component/quickstart "按需引入参考")
```html
"plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
```
![vue引入方式](./imgs/img1.jpg "vue引入方式")

## element开始
### Layout 布局
layout把页面分为24份，采用百分比布局
通过 row 和 col 组件，并通过 col 组件的 span 属性我们就可以自由地组合布局。
+ row组件
row组件为容器组件 在这个组件里面可以设置子元素之间的间隔
起作用的是给每个子组件设置内联样式`padding-left:10px;padding-right:10px;`
```html
<el-row :gutter="20"></el-row>
```
+ col组件 
col组件是布局组件
`<el-col :span="24"></el-col>` 这个el-col就占页面的`100%`,`<el-col :span="23"></el-col>`,这个el-col就占页面的`95.83333%`
+ 分栏偏移 
元素偏移多少单位 通过制定 col 组件的 offset 属性可以指定分栏偏移的栏数。也是百分比偏移 `margin-left:25%;`
```html
<el-row :gutter="20">
  <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="6" :offset="6"><div class="grid-content bg-purple"></div></el-col>
</el-row>
```
### 响应式布局
element 预设了五个响应尺寸：xs、sm、md、lg 和 xl。采当用媒体查询 显示区域在某个范围内时使用某一个尺寸
+ mobile – xs ( <768px ) 如iphonex  750px
+ tablet – sm ( 768~991px )  ipad
+ desktop – md ( 992~1199px ) 老式的电脑 比如大学学校电脑正方体的屏幕
+ large desktop – lg ( 1200px~1900px ) 笔记本到台式电脑
+ xl  (>1900px ) 超大电脑屏幕
如下面这段  在xs时占33.333%;在sm的时候占25%；在md时占16.666%；在lg时占12.5%；在xl时占4.16%；
```html
<el-row :gutter="10">
  <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple"></div></el-col>
</el-row>
```
### 基于断点的隐藏类  
使用方法 `class="hidden-xs-only"`
`only` 当在某个尺寸时隐藏
`and-down` 在某个尺寸下隐藏
`and-up` 在某个尺寸上隐藏
+  hidden-xs-only - 当视口在 xs 尺寸时隐藏
+  hidden-sm-only - 当视口在 sm 尺寸时隐藏
+  hidden-sm-and-down - 当视口在 sm 及以下尺寸时隐藏
+  hidden-sm-and-up - 当视口在 sm 及以上尺寸时隐藏
+  hidden-md-only - 当视口在 md 尺寸时隐藏
+  hidden-md-and-down - 当视口在 md 及以下尺寸时隐藏
+  hidden-md-and-up - 当视口在 md 及以上尺寸时隐藏
+  hidden-lg-only - 当视口在 lg 尺寸时隐藏
+  hidden-lg-and-down - 当视口在 lg 及以下尺寸时隐藏
+  hidden-lg-and-up - 当视口在 lg 及以上尺寸时隐藏
+  hidden-xl-only - 当视口在 xl 尺寸时隐藏
### Container 布局容器  
鸡肋  
`<el-container>`：外层容器。当子元素中包含 `<el-header>` 或 `<el-footer>` 时，全部子元素会垂直上下排列，否则会水平左右排列。

`<el-header>`：顶栏容器。

`<el-aside>`：侧边栏容器。

`<el-main>`：主要区域容器。

`<el-footer>`：底栏容器。
> 以上组件采用了 flex 布局，使用前请确定目标浏览器是否兼容。此外，`<el-container>` 的子元素只能是后四者，后四者的父元素也只能是 `<el-container>`。
### icon 图标 
element内置了一些icon图标 直接通过设置类名为 el-icon-iconName 来使用即可。例如：
```html
<i class="el-icon-edit"></i>
<i class="el-icon-share"></i>
<i class="el-icon-delete"></i>
<el-button type="primary" icon="el-icon-search">搜索</el-button>
```
![vue内置icon图标](./imgs/icon.jpg "vue内置icon图标")
### button 按钮
element 使用`type、plain、round和circle`属性来定义 Button 的样式。
  + `type` 定义按钮的样式
```html
<el-button type="primary">主要按钮</el-button>
```
  + `plain` 定义按钮是否为朴素按钮
```html
<el-button type="primary" plain>主要按钮</el-button>
```
  + `round` 定义按钮是否为圆角
```html
<el-button type="primary" round>主要按钮</el-button>
```
  + `circle` 定义按钮为圆形
```html
<el-button icon="el-icon-search" circle></el-button>
```
![vue button按钮](./imgs/button.jpg "vue button按钮")

### Link 文字链接
文字超链接 不可以用作跳转
```html
<el-link href="https://element.eleme.io" target="_blank">默认链接</el-link>
<el-link type="primary">主要链接</el-link>
<el-link type="success">成功链接</el-link>
<el-link type="warning">警告链接</el-link>
<el-link type="danger">危险链接</el-link>
<el-link type="info">信息链接</el-link>
```
![vue link文字链接](./imgs/link.jpg "vue link文字链接")

### Radio 单选框
在一组备选项中进行单选

### Checkbox 多选框
一组备选项中进行多选

### Input 输入框
通过鼠标或键盘输入字符

### ...

### element表单自定义验证
在防止用户犯错的前提下，尽可能让用户更早地发现并纠正错误。
``` html
<template>
  <div id="app">
    <!-- :model是绑定表单的属性  -->
    <!-- status-icon属性为输入侧框添加了表示校验查询查询结果的反馈图标。 -->
    <!-- rules是自定义的验证规则 -->
    <!-- 更多校验规则参考 async-validator  https://github.com/yiminghe/async-validator -->
    <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <!-- props是对应的那种校验规则 -->
      <el-form-item label="密码" prop="pass">
        <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="checkPass">
        <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="年龄" prop="age">
        <el-input v-model.number="ruleForm.age"></el-input>
      </el-form-item>
      <el-form-item>
        <!-- 既然用了element的表单验证规则 那么提交的时候也要用它定义的方法 -->
        <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
        <!-- 重置表单 -->
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>

  </div>
</template>
<script>
export default {
    data() {
      // 检测年龄 
      var checkAge = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('年龄不能为空'));
        }
        setTimeout(() => {
          if (!Number.isInteger(value)) {
            callback(new Error('请输入数字值'));
          } else {
            if (value < 18) {
              callback(new Error('必须年满18岁'));
            } else {
              callback();
            }
          }
        }, 1000);
      };
      // 检测输入的密码
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
         
        }
      };
      // 检测密码是否重复
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.ruleForm.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
        ruleForm: {
          pass: '',
          checkPass: '',
          age: ''
        },
        rules: {
          pass: [
            { validator: validatePass, trigger: 'blur' }
            // validator是验证规则  trigger是触发方式  如blue或change 
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          age: [
            { validator: checkAge, trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  }
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
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
<style lang="scss" scoped>
.el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
</style>

```

### 自定义表单验证步骤  

1.  在 el-form 表单里面添加需要校验的 规则(`rules`)  `<el-form ref="form" :model="form" label-width="80px" :rules="rulesname">`
2. 在表单的每一项 添加需要校验的属性 
`<el-form-item label="用户名" prop="username"><el-input v-model="form.username"></el-input></el-form-item>`
这一步需要检验的是 username  所以需要把这个username 添加到表单对应的这一项上面 同时 加个属性prop prop对应的值就是需要校验的属性


3. 书写自定义校验规则  `rulesname`  里面的username  pass 就是校验的每一项 对应prop
```html
<script>
rulesname:{ // 校验规则
    username: [
      // validator 校验规则 trigger 表单类型的触发方式 blur
      { validator: validateusername, trigger: 'blur' }
    ],
    pass: [
      { validator: validatepass, trigger: 'blur' }
    ]
  }
</script>
```
4. 实现校验规则 validator 的值就是校验规则 validateusername 这个函数需要自己手写 这个校验函数 写在data 里面的return 上面

5. 注意事项  validateusername 函数里面的callback  就是返回的校验信息  `return callback(new Error('密码长度不能小于6'));`  如果校验成功 什么都不返回 

### 分页 table

elementui实现表格增删改查


### 移动端框架 vant


*** 别用mint 几年没更新了 从网上找插件时千万别找那种几年没更新的项目 ***
轻量、可靠的移动端 Vue 组件库 
vant的开发团队是 `有赞`

### vant安装

[vant安装](https://youzan.github.io/vant/#/zh-CN/quickstart "vant安装")

### vant引入的两种方式 
+ 全局引入
```html

<script>
import Vue from 'vue';
import Vant from 'vant';
import 'vant/lib/index.css';

Vue.use(Vant);
</script>
```
+ 按需引入
首先安装babel插件
> 注意：配置 babel-plugin-import 插件后将不允许导入所有组件 
```html
<script>
npm i babel-plugin-import -D
</script>
```
配置babel
```html
<script>
module.exports = {
  plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
};
</script>
```
用的时候直接引入需要的组件即可
如
```html
<template>
  <div>
    <van-button type="default">默认按钮</van-button>
  </div>
</template>
<script>
import { Button } from 'vant';
import Vue from "vue";
Vue.use(Button)
</script>
```

安装参考
[vant引入方式](https://youzan.github.io/vant/#/zh-CN/quickstart "安装参考")


### vant rem适配 
vant的px尺寸转rem参考以下网址
[vant的px尺寸转rem](https://www.cnblogs.com/lml2017/p/9953429.html "vant的px尺寸转rem");

### pop弹出层结合日期 弹窗实现ios风格的日期选择
```html
<div id="app">
    <van-button type="primary" @click="showPopup">
      弹出时间
    </van-button>

    <van-popup v-model="show" position="bottom">
      <van-datetime-picker
        v-model="currentDate"
        type="date"
      ></van-popup>
  </div>
  <script>

export default {
  data () {
    return {
      currentDate:"",
      show:false,
      currentDate: new Date()
    }
  },
  methods: {
    showPopup(){
      this.show = !this.show;
    }
  }
}
</script>
```


### 上拉加载 

参考 这个网址  https://www.cnblogs.com/kerryw/p/9459621.html

### 下拉刷新 

vant的下拉刷新 使用前提首先样式得到位 

`.van-pull-refresh__track`是选择的时候可下拉的区域 所以得设置他的高度


### vant 作业 


vant 重构 前面做的购物车案例 




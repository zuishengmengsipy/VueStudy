# element

{% embed url="https://element.eleme.cn/\#/zh-CN/component/menu" %}

## 在线使用

导航栏使用方法：导航栏加路由方式

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!-- import CSS -->
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
    <!-- import JavaScript -->
    <script src="vue.js"></script>
    <!--    vue要先引入-->
    <script src="vue-router.js"></script>
    <script src="https://unpkg.com/element-ui/lib/index.js"></script>
    <style>
        .el-menu{
            /*弹性盒子*/
            display: flex;
            /*水平居中*/
            align-items: center;
            /*内容垂直居中*/
            justify-content: center;
        }
    </style>
</head>
<body>
<div id="app">
    <my_header></my_header>
    <router-view></router-view>
</div>

<template id="header">
    <div>
<!--  浏览器解析的时候大多会吧标签名el-menu这种转变成div等标签的类名-->
<!--  <el-menu router :default-active="activeIndex" class="el-menu-demo" mode="horizontal">-->
    <el-menu :router="true" :default-active="activeIndex" class="el-menu-demo" mode="horizontal">
<!--   :router="true",把该变量设为true，即可在激活导航时把 index 作为 path 进行路由跳转-->
        <el-menu-item index="/">处理中心</el-menu-item>
<!--            index相同，点击后动画相同-->
        <el-menu-item index="/java">java</el-menu-item>
        <el-menu-item index="/python">python</el-menu-item>
        <el-menu-item index="4">c</el-menu-item>
        <el-menu-item index="5">c++</el-menu-item>
    </el-menu>
    </div>
</template>
<script>
    let my_header = {
        template: "#header",
        data() {
            return {
                activeIndex: '/', //首页默认index
            };
        },
    };
    let rou = new VueRouter({
        routes:[
            {
                path:"/",
                component:{
                    template: `<div><h1>这是首页组件</h1></div>`
                }
            },
            {
                path:"/java",
                component:{
                    template: `<div><h1>这是java组件</h1></div>`
                }
            },
            {
                path:"/python",
                component:{
                    template: `<div><h1>这是python组件</h1></div>`
                }
            },
        ]
    });
    const app = new Vue ({
        el:"#app",
        components:{
            my_header: my_header
        },
        router: rou,
    })
</script>
</body>
</html>
```

## 配合vue-cli使用

### 安装element-ui

能更好的配合webpack打包工具的使用

```javascript
npm install element-ui -S
```

### 引用element-ui

在 main.js 中写入以下内容：

```javascript
import Vue from 'vue';
import ElementUI from 'element-ui';  // 引入
import 'element-ui/lib/theme-chalk/index.css'; // 引入样式文件
import App from './App.vue';

Vue.use(ElementUI); // vue使用

new Vue({
  el: '#app',
  render: h => h(App)
});
```

以上代码便完成了 Element 的引入。需要注意的是，样式文件需要单独引入。


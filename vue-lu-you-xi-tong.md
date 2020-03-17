# vue 路由系统

## Vue的路由系统

### 什么是路由

* 对于普通的网站，所有的超链接都是URL地址，所有的URL地址都对应服务器上对应的资源；
* 对于单页面应用程序来说，主要通过URL中的hash\(\#号\)来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，单页面程序中的页面跳转主要用hash实现；
* 在单页面应用程序中，这种通过hash改变来切换页面的方式，称作前端路由（区别于后端路由）；

### 在 vue 中使用 vue-router

* 导入 vue-router 组件类库：

```markup
<!-- 1. 导入 vue-router 组件类库 -->
<script src="./lib/vue-router-2.7.0.js"></script>
```

* 使用 router-link 组件来导航

```markup
<!-- 2. 使用 router-link 组件来导航 -->
<router-link to="/login">登录</router-link><!--默认相当于创建一个a标签，点击跳转到#/-->
<router-link to="/register" tag="span">注册</router-link>
<!-- tag属性相当于将默认创建的a标签切换成自定义标签 -->
```

* 使用 router-view 组件来显示匹配到的组件

```markup
<!-- 3. 使用 router-view 组件来显示匹配到的组件 -->
<router-view></router-view>
```

* 创建使用`Vue.extend`创建组件

```javascript
 // 4.1 使用 Vue.extend 来创建登录组件
 var login = Vue.extend({
   template: '<h1>登录组件</h1>'
 });

 // 4.2 使用 Vue.extend 来创建注册组件
 var register = Vue.extend({
   template: '<h1>注册组件</h1>'
 });
```

* 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则

```javascript
// 5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则
 var router = new VueRouter({
   routes: [
     { path: '/login', component: login },
     { path: '/register', component: register }
   ]
 });
```

* 使用 router 属性来使用路由规则

```javascript
// 6. 创建 Vue 实例，得到 ViewModel
 var vm = new Vue({
   el: '#app',
   router: router // 使用 router 属性来使用路由规则
 });
```

例子如下，包括（路由高亮，路由切换动效）：

```markup
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script>
  <!--1.安装vue-router路由模块-->
  <script src="./lib/vue-router-3.0.1.js"></script>
  <!--router-link-active是链接激活的时候自动携带，可在路由对象中使用linkActiveClass修改-->
  <style>
    .router-link-active,
    .myactive {
      color: red;
      font-weight: 800;
      font-style: italic;
      font-size: 80px;
      text-decoration: underline;
      background-color: green;
    }

    .v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateX(140px);
    }

    .v-enter-active,
    .v-leave-active {
      transition: all 0.5s ease;
    }
  </style>
</head>

<body>
  <div id="app">
    <!-- <a href="#/login">登录</a> -->
    <!-- <a href="#/register">注册</a> -->
    <!-- router-link默认渲染为一个a标签，并将链接转化为html锚点#/，如上 -->
    <router-link to="/login" tag="span">登录</router-link>
    <router-link to="/register">注册</router-link>

    <transition mode="out-in"><!-- 设置切换动画 -->
    <!-- 这是 vue-router 提供的元素，专门用来当作占位符的，
    按路由规则匹配到的组件，就会展示到这个router-view中去 -->
      <router-view></router-view><!-- 所以：我们可以把 router-view 认为是一个占位符 -->
    </transition>
  </div>

  <script>
    // 组件的模板对象
    let login = {
      template: '<h1>登录组件</h1>'
    }

    let register = {
      template: '<h1>注册组件</h1>'
    }

    /*  Vue.component('login', {
       template: '<h1>登录组件</h1>'
     }) */

    // 2. 创建一个路由对象，导入vue-router包之后，在window全局对象中，
    // 就有了一个路由的构造函数，叫做VueRouter
    // 在 new 路由对象的时候，可以为 构造函数，传递一个配置对象
    let routerObj = new VueRouter({
      // route // 这个配置对象中的 route 表示 【路由匹配规则】 的意思
      routes: [ // 路由匹配规则 
// 每个路由规则，都是一个对象，这个规则对象，身上，有两个必须的属性：
//  属性1 是 path， 表示监听 哪个路由链接地址；
//  属性2 是 component，表示如果路由是前面匹配到的path,则展示 component 属性对应的那个组件
        // 注意： component 的属性值，必须是一个 组件的模板对象， 不能是 组件的引用名称；
        // { path: '/', component: login },
        { path: '/', redirect: '/login' }, 
        // 这里的 redirect 和 Node 中的 redirect 完全是两码事
        { path: '/login', component: login },
        { path: '/register', component: register }
      ],
      linkActiveClass: 'myactive',  
      //这个属性是定义链接生效的默认类，默认是router-link-active
      //mode:'history'
      //这个属性是把路径上的锚点#去掉，更美观
    })

    let vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router: routerObj 
      // 将路由规则对象，注册到vm实例上，用来监听URL地址的变化，然后展示对应的组件
    });
  </script>
</body>
</html>
```

## 命名路由：路由name反向解析路径（:to）

路由匹配规则中使用name去定义，router-link使用:to去绑定，这样可以不用管路径直接进行路由导航

```markup
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="vue.js"></script>
  <script src="./node_modules/vue-router/dist/vue-router.js"></script>
  <style>
    .router-link-active,
    .myactive {
      color: red;
      font-weight: 800;
      font-style: italic;
      font-size: 80px;
      text-decoration: underline;
      background-color: green;
    }
    .v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateX(140px);
    }
    .v-enter-active,
    .v-leave-active {
      transition: all 0.5s ease;
    }
  </style>
</head>
<body>
  <div id="app">
<!--    <router-link to="/login" tag="span">登录</router-link>-->
<!--    <router-link to="/register">注册</router-link>-->
    <router-link :to="{name: 'log'}" tag="span">登录</router-link>
    <router-link :to="{name: 'reg'}">注册</router-link>
    <transition mode="out-in">
      <router-view></router-view>
    </transition>
  </div>
  <script>
    let login = {
      template: '<h1>登录组件</h1>'
    }
    let register = {
      template: '<h1>注册组件</h1>'
    }
    let routerObj = new VueRouter({
      routes: [ // 路由匹配规则
        { path: '/', redirect: '/login', name:'home'}, //使用name反向解析路由路径
        { path: '/login', name:'log',component: login },
        { path: '/register',name:"reg", component: register }
      ],
      linkActiveClass: 'myactive'
    })
    let vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router: routerObj
    });
  </script>
</body>
</html>
```

## 命名的路由视图

给路由定义不同名字，根据名字进行匹配，这叫命名路由

给不同的router-view 定义名字，通过名字进行对应组件的渲染，这叫命名的路由视图

在VueRouter的实例中，如果需要一次展示多个组件的话，可以使用:

```javascript
{ 
  path:'xx',
  components: {
       'default'(默认)：xxx(组件),
        'leftBox'(自定义命名): xx(组件名),
        xxx(自定义名)： xx(组件名),
  }
}
```

然后在中的name属性上，就可以使用自定义名来控制了

```javascript
<div id="app">
    <router-view></router-view>//注意要写一个默认的视图，不然别的单视图路由组件无法显示
    //默认显示的，有的路由只有一个组件，只显示这一个，多个组件由下方显示
    <router-view name="leftBox"></router-view>//只显示名字为leftBox上的组件
    <router-view name="rightBox" class="myright"></router-view> //只显示名字为leftBox上的组件
    //这样还有一个好处是可以加不同的class控制样式，位置等
</div>
<script>
    var header={
        template:'<h4>我是头部</h4>'
    }
    var left={
        template: '<p>我是左边</p>'
    }
    var right={
        template:'<p>我是右边</p>'
    }
    var rtobj=new VueRouter({
        routes:[
            {
                path:'/',
                components: {
                    'default':header,
                    'leftBox':left,
                    'rightBox':right
                }
            }
        ]
    })
    var vm=new Vue({
        el:"#app",
        router:rtobj
    })
</script>
```

原文链接：[https://blog.csdn.net/Zyl\_CN/article/details/83118489](https://blog.csdn.net/Zyl_CN/article/details/83118489)

## 在路由规则中定义参数

**第一种：在路径中直接使用`?id=11`进行查询，路由不用改变，例/login?id=8&name=wlx**

```text
{path: '/login', component: login}
```

通过 `this.$route.query`来获取路由中的参数：

```javascript
var login= Vue.extend({
   template: '<h1>登陆组件 --- {{this.$route.query.id}}</h1>'
});
```

**第二种：在规则中定义参数，路径直接使用/分割，例/register/8888/weilianxin：**

```javascript
{ path: '/register/:id', component: register }
```

通过 `this.$route.params`来获取路由中的参数：

```javascript
var register = Vue.extend({
   template: '<h1>注册组件 --- {{this.$route.params.id}}</h1>'
});
```

`this.$route`是一个对象，里面存放各种路由请求，匹配信息等

综合案例如下：

```markup
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
<div id="app">
    <router-link to="/login?id=8&name=wlx" tag="span">登录</router-link>
    <router-link to="/register/8888/weilianxin">注册</router-link>
    <router-view></router-view>
</div>
<script>
    let login = {
        template: '<h1>登录组件</h1>',
        created() { // 组件的生命周期钩子函数
            console.log(this.$route)
            console.log(this.$route.query.id+"========"+this.$route.query.name)
        }
    }
    let register = {
        template: '<h1>注册组件</h1>',
        created() { // 组件的生命周期钩子函数
            console.log(this.$route)
            console.log(this.$route.params.id+"========"+this.$route.params.name)
        }
    }
    let rou = new VueRouter({
        routes: [
            {path: '/login', component: login},
            {path: '/register/:id/:name', component: register},
        ]
    })
    let vm = new Vue({
        el: '#app',
        data: {},
        methods: {},
        components: {},
        router: rou
    })
</script>
</body>
</html>
```

## 手动路由，手动跳转路由地址

```markup
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
<div id="app">
    <router-link to="/login?id=8&name=wlx" tag="span">登录</router-link>
    <router-link to="/register/8888/weilianxin">注册</router-link>
    <router-view></router-view>
</div>
<script>
    let login = {
        template: '<h1>登录组件</h1>',
    }
    let register = {
        template: `<div>
                   <h1>注册组件</h1>
                   <button @click="my_click">点击跳转到登陆页面</button>
                   </div>`,
        methods:{
            my_click() {
                console.log(this.$route);
                console.log(this.$router);
                console.log(this.$el);
                console.log(this.$data);
                // $route 路由的所有信息
                // $router VueRouter实例化对象
                this.$router.push("/login") // 手动重定向路由路径
            }
        }
    }
    let rou = new VueRouter({
        routes: [
            {path: '/login', component: login},
            {path: '/register/:id/:name', component: register},
        ]
    })
    let vm = new Vue({
        el: '#app',
        data: {},
        methods: {},
        components: {},
        router: rou
    })
</script>
</body>
</html>
```

## 使用 `children`  属性实现路由嵌套

```markup
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
<div id="app">
    <router-link to="/account">Account</router-link>
    <router-view></router-view>
</div>

<script>
    // 父路由中的组件
    const account = Vue.extend({
        template: `<div>
        这是account组件
        <router-link to="/account/login">login</router-link> |
        <router-link to="/account/register">register</router-link>
        <router-view></router-view>
      </div>`
    });

    // 子路由中的 login 组件
    const login = Vue.extend({
        template: '<div>登录组件</div>'
    });

    // 子路由中的 register 组件
    const register = Vue.extend({
        template: '<div>注册组件</div>'
    });

    // 路由实例
    var router = new VueRouter({
        routes: [
            { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向，可以实现默认显示什么页面
            {
                path: '/account',
                component: account,
                children: [ // 通过 children 数组属性，来实现路由的嵌套
                    { path: 'login', component: login },
                    // 注意，子路由的开头位置，不要加 / 路径符
                    { path: 'register', component: register }
                ]
            }
        ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {},
        components: {
            account
        },
        router: router
    });
</script>
</body>
</html>
```

## 路由验证用户登录状态的方法

使用router.beforeEach\(\)和判断登录状态的meta属性auth，mata的元信息，键值都是自己设置，作用自己设置。

```javascript
const routes =[
    {
        path:'/',
        component: resolve => require(['../components/mainTem/Filtrate.vue'],resolve),
        meta:{auth:true,keepAlive: false},//元信息的键值自己设置
        name:'Filtrate'
    },
    {//首页
        path:'/Home',
        component: resolve => require(['../components/mainTem/Home.vue'],resolve),
        meta:{auth:true,keepAlive: true },
        name:'Home'
    },
    {//登录
        path:'/Login',
        component: resolve => require(['../components/mainTem/Login.vue'],resolve),
        meta:{auth:true,keepAlive: false},
        name:'Login'
    },//最新案例
    {
        path:'/NewCase',
        component: resolve => require(['../components/mainTem/NewCase.vue'],resolve),
        meta:{auth:true,keepAlive: true},
        name:'NewCase'
    },//推荐
    {
        path:'/Recommend',
        component: resolve => require(['../components/mainTem/Recommend.vue'],resolve),
        meta:{auth:true,keepAlive: true},
        name:'Recommend'
    },//个人中心
    {
        path:'/User',
        component: resolve => require(['../components/mainTem/User.vue'],resolve),
        meta:{auth:true,keepAlive: true},
        name:'User'
    },//筛选
    {
        path:'/Filtrate',
        component: resolve => require(['../components/mainTem/Filtrate.vue'],resolve),
        meta:{auth:true,keepAlive: false},
        name:'Filtrate'
    },
    {//详情
        path:'/Detail',
        component: resolve => require(['../components/mainTem/Detail.vue'],resolve),
        meta:{auth:true,keepAlive: false},
        name:'Detail'
    }
]
```

使用localStorage储存的用户token值。 

业务逻辑：用户没有localStorage说明是第一次登录，则直接跳到登录页面，在登录页面储存token值，存在localStorage，用户关闭页面后，在24小时内，再次打开页面直接进入主页面，通过localStorage去判断当前token是否有效，如果已失效，则提示登录已超时，重新跳到登录页面。

**meta 只有在路由中设置了meta auto属性为true的才判断，以上路由全部由设置**

**router.beforeEach\(function\(to,from,next\){}\)**

        **to to为向后走的路由对象，包括路由的完整信息**

        **from from为从哪跳来的路由对象**

        **next\(\) next\(\)控制路由向下走，重新定义路由跳转的路由next\(‘路由路径\)，没有参数则按原目的地走，不写next\(\)则会不跳转**

```javascript
/** 验证用户是否登录 **/
router.beforeEach((to,from,next) => {
    //下两句都是如果在mate中有Auth 那么就返回true
    //if(to.some( m => m.meta.auth)) {
    if(to.matched.some( m => m.meta.auth)) {
        // console.log("先判断访问的是否是登录页面");，不判定可能会next死循环
        if(to.name=='Login'){//如果是
            next();
        }else{
          if(localStorage.getItem('token')){

          //访问服务器缓存数据，判断当前token是否失效
            Vue.http.get("http:xxxx/Login/UserIsLogin?token="+
            localStorage.getItem('token')+"&url="+
            to.name,{withCredentials: true}).
            then(response => response.json()).then(num => {
                    // console.log('是否登录',num);
                    if(num.code==1001){
                        next(); // 如果已经登陆
                    }else{
                        alert('您的token已超时，请重新登录');
                        next('/Login');
                    }
            })
          }else{
            next('/Login');
          }

        }
  　} else {
        console.log("请先登录");
        next()
    }
})
```

beforeEach的next死循环：

```javascript
router.beforeEach((to, from, next) => {
    // 判定登陆状态
    if (to.mate.requireAuth) { //如果已经登陆，则继续   
        next();
    }
    next({path: '/login'}) // 未登录则login，会一直循环跳转到login，所以要判定是否是login
});
```

## 命名视图实现经典布局（一个页面多个同级组件）

```markup
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>    <title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .header {
            background: red;
            height: 100px;
        }

        .container {
            display: flex;
            height: 500px;
        }

        .left {
            background: lightblue;
            flex: 2;
        }

        .main {
            background: lightsalmon;
            flex: 8;
        }
    </style>
</head>
<body>
<div id='app'>
    <router-view></router-view>
    <div class="container">
        <router-view name="left"></router-view>
        <router-view name="main"></router-view>
    </div>
</div>
</body>
<script>
    let header = {
        template: "<h1 class='header'>Header头部区域</h1>"
    }
    let leftBox = {
        template: "<h1 class='left'>leftBox侧边栏区域</h1>"
    }
    let mainBox = {
        template: "<h1 class='main'>MainBox主体区域</h1>"
    }
    let router = new VueRouter({
        routes: [
            {
                path: '/', components: {
                    'default': header,
                    'left': leftBox,
                    'main': mainBox
                }
            },
            // {path:'/',component:header},
            // {path:'/left',component:leftBox},
            // {path:'/main',component:mainBox}
        ]
    })
    // 实例化vue对象
    let vm = new Vue({
        // 绑定对象
        el: '#app',
        data: {},
        methods: {},
        router
    })
</script>
</html>
```

## `nrm`的安装使用

作用：提供了一些最常用的NPM包镜像地址，能够让我们快速的切换安装包时候的服务器地址； 什么是镜像：原来包刚一开始是只存在于国外的NPM服务器，但是由于网络原因，经常访问不到，这时候，我们可以在国内，创建一个和官网完全一样的NPM服务器，只不过，数据都是从人家那里拿过来的，除此之外，使用方式完全一样；

1. 运行`npm i nrm -g`全局安装`nrm`包； 

2. 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址； 

3. 使用`nrm use npm`或`nrm use taobao`切换不同的镜像源地址；

## 相关文件

1. [URL中的hash（井号）](http://www.cnblogs.com/joyho/articles/4430148.html)


# day3

## 定义Vue组件

什么是组件： 组件的出现，就是为了拆分Vue实例的代码量的，能够让我们以不同的组件，来划分不同的功能模块，将来我们需要什么样的功能，就可以去调用对应的组件即可； 组件化和模块化的不同：

* 模块化： 是从代码逻辑的角度进行划分的；方便代码分层开发，保证每个功能模块的职能单一；
* 组件化： 是从UI界面的角度进行划分的；前端的组件化，方便UI组件的重用；

### **全局组件定义的三种方式**

**全局组件定义的三种方式**

```markup
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <!--自定义组件使用时一定要被vue绑定的标签包裹，否则无效，例如下面一行的注释-->
        <!--<my-com1></my-com1>  无效-->
        <div id='app'>
            <!-- 如果使用组件，直接，把组件的名称，以HTML标签的形式，引入到页面中，即可 -->
            <!-- 定义的时候可以用驼峰法，调用时大写变小写，中间加“-” -->
            <my-com1></my-com1>
            <mycom2></mycom2>
            <mycom3></mycom3>
            <mycom4></mycom4>
            <mycom5></mycom5>
        </div>
        <!-- 在vue控制的#app外面，使用template元素，定义组件的HTML结构 -->
        <template id="tmp1">
            <div>
                <h3>这是通过template元素，在外部创建的组件</h3>
                <p>好用，不错</p>
            </div>
        </template>
        <!-- 在vue控制的#app外面，使用script元素和type属性，定义组件的HTML结构 -->
        <script id="tmp2" type="x-template">
            <div><a href="#">登录</a> | <a href="#">注册</a></div>
        </script>
    </body>
    <script>
        // 1.1、使用Vue.extend 来创建全局的Vue组件
        let com1 = Vue.extend({
            // 通过template属性，指定了组件要展示的HTML结构
            template:'<h3>这是用Vue.extend创建的组件</h3>'//内部必须只有一个根节点
        })
        // 1.2、使用Vue.component('组件的名称',创建出来的的组件模板对象)
        // 定义的时候可以用驼峰法，但是调用时必须使用“-”，否则无效
        Vue.component('myCom1',com1)
        
        // 2.1、合并写法
        Vue.component('mycom2',Vue.extend({
            // 通过temloate属性，指定了组件要展示的HTML结构
            template:'<h3>这是用Vue.component两步合并创建的组件</h3>'//内部必须只有一个根节点
        }))
        // 2.2、简化写法
        Vue.component('mycom3',{
            template:'<h3>这是用Vue.component一步创建的组件</h3>'//内部必须只有一个根节点
        })
        
        // 3.1、指定写法，指定组件的部分
        Vue.component('mycom4',{
            template:'#tmp1'
        })
        //3.2
        Vue.component('mycom5', {
            template: '#tmp2'
        })
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
            },
            methods:{
            }
        })
    </script>
</html>
```

> 注意： 组件中的DOM结构，有且只能有唯一的根元素（Root Element）来进行包裹！

### 私有组件的定义方式

> 定义私有组件通过Vue构造函数的components

```markup
<html>
    <head>
        <meta charset='utf-8'><title></title>
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
            <login></login>
            <login2></login2>
        </div>
        <div id="app2">
            <!-- 报错，因为只在app1中有效 -->
            <login></login>
        </div>
        <template id="tmp1">
            <h1>私有的login2组件</h1>
        </template>
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
            },
            methods:{
            },
            components:{//定义实例内部私有组件
                login:{
                    template:"<h1>私有的login组件</h1>"
                },
                login2:{
                    template:"#tmp1"
                }
            }
        })
        let vm2 = new Vue({
            // 绑定对象
            el:'#app2',
            data:{
            },
            methods:{
            }
        })
    </script>
</html>
```

### 组件中展示数据和响应事件

在组件中，`data`需要被定义为一个方法，例如：

```markup
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
            <mycom1></mycom1>

            <counter></counter>
            <hr>
            <counter></counter>
            <hr>
            <counter></counter>
        </div>
        <template id="counter">
            <div>
                <input type="button" value="+1" @click="increment">
                <h3>{{count}}</h3>
            </div>
        </template>
    </body>
    <script>
        // 1、组件可以有自己的 data 数据
        // 2、组件的 data 和 实例的data 有一点不一样，实例中的data可以为一个对象，但是组件中的data必须是一个方法
        // 3、组件中的data 除了必须为一个方法外，这个方法内部，还必须返回一个对象才行
        Vue.component('mycom1',{
            template:'<h1>这是全局组件.data:{{msg}}</h1>',
            data:function(){
                return {
                    msg:'这是data里的信息'
                }
            }
        })
        let dataObj = { count:0 }
        Vue.component('counter',{
            template:'#counter',
            data:function(){
                // return dataObj //如果使用这句话，的一下按钮，三个计数器同时计数因为返回的是同一个计数对象
                return { count:0 } //使用下面这种是因为要每次组件生成标签，data是不同的对象，互不影响
            },
            methods:{
                increment(){
                    this.count++
                }
            }
        })
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
            },
            methods:{
            }
        })
    </script>
</html>
```

在子组件中，如果将模板字符串，定义到了script标签中，那么，要访问子组件身上的`data`属性中的值，需要使用`this`来访问；

**【重点】为什么组件中的data属性必须定义为一个方法并返回一个对象？**

计数器案例，方法每次返回的都是不同的数据对象，计数器之间互不影响，返回对象的话，所有的计数器调用的都是同一个计数对象

## 组件的切换

```markup
<html>
<head>
    <meta charset='utf-8'>
    <title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    <style>
        .v-enter,
        .v-leave-to {
            opacity: 0;
            transform: translateX(30px);
        }

        .v-enter-active,
        .v-leave-active {
            position: absolute;
            transition: all 0.3s ease;
        }

        h3 {
            margin: 0;
        }
    </style>
</head>
<body>
<div id='app'>
    <h2>使用v-if、v-else切换组件（只能两个之间切换）</h2>
    <a href="" @click.prevent="flag=true">登录</a>
    <a href="" @click.prevent="flag=false">注册</a>
    <login v-if="flag"></login>
    <register v-else="flag"></register>
    <hr>
    <h2>使用component元素切换组件(不限定数量切换),并添加切换动画</h2>
    <a href="" @click.prevent="componentId='login'">登录</a>
    <a href="" @click.prevent="componentId='register'">注册</a>
    <!-- component是一个占位符，:is属性，可以用来指定要展示的组件名称 -->
    <transition mode="out-in">
        <component :is="componentId"></component>
    </transition>
</div>

</body>
<script>
    Vue.component('login', {
        template: '<h3>登录组件</h3>'
    })
    Vue.component('register', {
        template: '<h3>注册组件</h3>'
    })
    // 实例化vue对象
    let vm = new Vue({
        // 绑定对象
        el: '#app',
        data: {
            flag: true,
            componentId: 'login'
        },
        methods: {}
    })
</script>
</html>

<!-- 当前学过的Vue标签 -->
<!--
    <component :is="componentId"></component>
    <template></template>
    <transition></transition>
    <transition-group></transition-group>
-->
```

**使用`:is`属性来切换不同的字组件，并添加切换动画步骤详解**

组件实例定义方式：

```javascript
// 登录组件
const login = Vue.extend({
  template: `<div>
    <h3>登录组件</h3>
  </div>`
});
Vue.component('login', login);
// 注册组件
const register = Vue.extend({
  template: `<div>
    <h3>注册组件</h3>
  </div>`
});
Vue.component('register', register);
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
  el: '#app',
  data: { comName: 'login' },
  methods: {}
});
```

使用`component`标签，来引用组件，并通过`:is`属性来指定要加载的组件：

```markup
<div id="app">
  <a href="#" @click.prevent="comName='login'">登录</a>
  <a href="#" @click.prevent="comName='register'">注册</a>
  <hr>
  <transition mode="out-in">
    <component :is="comName"></component>
  </transition>
</div>
```

添加切换样式：

```css
<style>
  .v-enter,
  .v-leave-to {
    opacity: 0;
    transform: translateX(30px);
  }
  .v-enter-active,
  .v-leave-active {
    position: absolute;
    transition: all 0.3s ease;
  }
  h3{
    margin: 0;
  }
</style>
```

## 父子组件之间的传值

### 父向子传值

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
<div id="app">
    <!-- 父组件，可以在引用子组件的时候，通过属性绑定（v-bind:）的形式, 把需要传递给子组件的数据，以属性绑定的形式，传递到子组件内部，供子组件使用 -->
    <com1 v-bind:parentmsg="msg"></com1><!-- v-bind绑定后也不能立即使用，需要在子组件的props数组里定义一下 -->
</div>
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
        el: '#app',
        data: {
            msg: '123 啊-父组件中的数据'
        },
        methods: {},
        components: {
            // 子组件中默认无法访问到父组件中的data上的数据和methods 中的方法
            com1: {
                // data: function () {
                //     // 因为data一定得是方法，所以两种书写方式都行，这种和下面那种
                //     return {
                //         title: '123',
                //         content: 'qqq'
                //     }
                // },
                data() { // 注意：子组件中的 data 数据，并不是通过父组件传递过来的，而是子组件自身私有的，比如：子组件通过 Ajax，请求回来的数据，都可以放到 data 身上；
                    // data 上的数据，都是可读可写的；props中的数据，都是只读的，无法重新赋值
                    return {
                        title: '123',
                        content: 'qqq'
                    }
                },
                template: '<h1 @click="change">这是子组件 --- {{ parentmsg }}</h1>',
                // 注意：组件中的所有props中的数据，都是通过父组件传递给子组件的
                // props中的数据，都是只读的，无法重新赋值
                props: ['parentmsg'], // 把父组件传递过来的parentmsg属性，先在props数组定义一下，这样才能使用这个数据
                directives: {},
                filters: {},
                components: {},
                methods: {change() {this.parentmsg = '被修改了'}}
            }
        }
    });
</script>
</body>
</html>
```

### 父向子传递函数和子组件向父组件传值

**父组件向子组件传递方法：**使用的是事件绑定机制；v-on,调用是在子组件里使用$emit进行调用

**子组件向父组件传值用以下方法实现：**

* 原理：父组件将方法的引用，传递到子组件内部，子组件在内部调用父组件传递过来的方法，同时把要发送给父组件的数据，在调用方法的时候当作参数传递进去。
* 父组件将方法的引用传递给子组件，其中，`show`是父组件中`methods`中定义的方法名称，`func`是子组件调用传递过来方法时候的方法名称`<com2 @func="show"></com2>`
* 子组件内部通过`this.$emit('方法名', 要传递的数据)`方式，来调用父组件中的方法，同时把数据传递给父组件使用

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="./lib/vue.js"></script>
</head>
<body>
  <div id="app">
    <!-- 父组件向子组件传递方法，使用的是事件绑定机制；v-on -->
    <com2 @func="show"></com2><!-- 调用是在子组件里使用$emit进行调用 -->
  </div>
  <template id="tmpl">
    <div>
      <h1>这是子组件的模板</h1>
      <input type="button" value="这是子组件中的按钮，点击它，触发父组件传递过来的func方法" @click="myclick">
    </div>
  </template>

  <script>
    // 定义了一个字面量类型的组件模板对象
    let com2 = {
      template: '#tmpl', // 通过指定了一个Id,表示去加载这个指定Id的template元素中的内容，当作组件的HTML结构
      data() {
        return {
          sonmsg: { name: '小头儿子', age: 6 }
        }
      },
      methods: {
        myclick() {
          // 使用emit在子组件调用父组件传来的方法，emit 英文原意是触发，调用、发射的意思
          // this.$emit('func123', 123, 456)，第一个参数是方法名，后面的参数都是方法的形参
          this.$emit('func', this.sonmsg)
        }
      }
    }

    let vm = new Vue({
      el: '#app',
      data: {
        datamsgFormSon: null  //用于接收子组件传来的数据
      },
      methods: {
        show(data) {
          console.log('调用了父组件身上的 show 方法: --- ' + data)
          this.datamsgFormSon = data  //获取子组件传来的数据
          console.log(this.datamsgFormSon.name) //使用子组件传来的数据
        }
      },
      components: {
        com2
        // com2: com2
      }
    });
  </script>
</body>
</html>
```

## 评论列表案例

目标：主要练习父子组件之间传值

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Doc</title>
    <script src="./lib/vue.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css"
          integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
</head>
<body>
<div id="app">
    <cmt-box @func="loadComments"></cmt-box>
    <ul class="list-group">
        <li class="list-group-item" v-for="item in list" :key="item.id">
            <span class="badge">评论人：{{ item.user }}</span>
            {{ item.content }}
        </li>
    </ul>
</div>
<template id="tmpl">
    <div>
        <div class="form-group">
            <label>评论人：</label>
            <input type="text" class="form-control" v-model="user">
        </div>
        <div class="form-group">
            <label>评论内容：</label>
            <textarea class="form-control" v-model="content"></textarea>
        </div>
        <div class="form-group">
            <input type="button" value="发表评论" class="btn btn-primary" @click="postComment">
        </div>
    </div>
</template>
<script>
    let commentBox = {
        data() {  //评论输入框内容置空
            return {
                user: '',
                content: ''
            }
        },
        template: '#tmpl',
        methods: {
            postComment() { // 发表评论的方法
                // 分析：发表评论的业务逻辑
                // 1. 评论数据存到哪里去？？？存放到了localStorage（开发者工具Application的local Storage）中，localStorage.setItem('cmts', '')
                // 2. 先组织出一个最新的评论数据对象
                // 3. 想办法，把第二步中，得到的评论对象，保存到 localStorage 中：
                //  3.1 localStorage只支持存放字符串数据，要先调用JSON.stringify把数据变成字符串
                //  3.2 在保存最新的评论数据之前，要先从localStorage获取到之前的评论数据（string），转换为 一个数组对象，然后把最新的评论，push到这个数组
                //  3.3 如果获取到的 localStorage 中的评论字符串，为空不存在，则可以返回一个'[]'，让 JSON.parse 去转换，否则为空报错
                //  3.4 把最新的评论列表数组，再次调用JSON.stringify转为数组字符串，然后调用localStorage.setItem()
                let comment = {id: Date.now(), user: this.user, content: this.content} //第一步生成评论对象
                // 从 localStorage 中获取所有的评论，如果没有则返回'[]'
                let list = JSON.parse(localStorage.getItem('cmts') || '[]')
                list.unshift(comment) //将评论push到第一条
                // 在localstorage中重新保存最新的评论数据
                localStorage.setItem('cmts', JSON.stringify(list))

                this.user = this.content = ''  //把评论框的内容置空
                // this.loadComments() // ?????
                this.$emit('func')
            }
        }
    }
    let vm = new Vue({
        el: '#app',
        data: {
            list: [
                {id: Date.now(), user: '李白', content: '天生我材必有用'},
                {id: Date.now(), user: '江小白', content: '劝君更尽一杯酒'},
                {id: Date.now(), user: '小马', content: '我姓马，风吹草低见牛羊的马'}
            ]
        },
        created() {
            this.loadComments()
        },
        methods: {
            loadComments() { // 从本地的 localStorage 中，加载评论列表
                let list = JSON.parse(localStorage.getItem('cmts') || '[]')
                this.list = list
                console.log(list)
            }
        },
        components: {
            'cmt-box': commentBox
        }
    })
</script>
</body>
</html>
```

## 使用 `this.$refs` 来获取元素和组件

```markup
  <div id="app">
    <div>
      <input type="button" value="获取元素内容" @click="getElement" />
      <!-- 使用 ref 获取元素 -->
      <h1 ref="myh1">这是一个大大的H1</h1>

      <hr>
      <!-- 使用 ref 获取子组件 -->
      <my-com ref="mycom"></my-com>
    </div>
  </div>

  <script>
    Vue.component('my-com', {
      template: '<h5>这是一个子组件</h5>',
      data() {
        return {
          name: '子组件'
        }
      }
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        getElement() {
          // 通过 this.$refs 来获取元素
          console.log(this.$refs.myh1.innerText);
          // 通过 this.$refs 来获取组件
          console.log(this.$refs.mycom.name);
        }
      }
    });
  </script>
```

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
<router-link to="/login">登录</router-link><!--相当于创建一个a标签，点击跳转到#/-->
<router-link to="/register">注册</router-link>
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
    <!-- router-link默认渲染为一个a标签，如上 -->
    <router-link to="/login" tag="span">登录</router-link>
    <router-link to="/register">注册</router-link>


    <transition mode="out-in"><!-- 设置切换动画 -->
    <!-- 这是 vue-router 提供的元素，专门用来当作占位符的，按路由规则匹配到的组件，就会展示到这个router-view中去 -->
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

    // 2. 创建一个路由对象，导入vue-router包之后，在window全局对象中，就有了一个路由的构造函数，叫做VueRouter
    // 在 new 路由对象的时候，可以为 构造函数，传递一个配置对象
    let routerObj = new VueRouter({
      // route // 这个配置对象中的 route 表示 【路由匹配规则】 的意思
      routes: [ // 路由匹配规则 
        // 每个路由规则，都是一个对象，这个规则对象，身上，有两个必须的属性：
        //  属性1 是 path， 表示监听 哪个路由链接地址；
        //  属性2 是 component， 表示，如果 路由是前面匹配到的 path ，则展示 component 属性对应的那个组件
        // 注意： component 的属性值，必须是一个 组件的模板对象， 不能是 组件的引用名称；
        // { path: '/', component: login },
        { path: '/', redirect: '/login' }, // 这里的 redirect 和 Node 中的 redirect 完全是两码事
        { path: '/login', component: login },
        { path: '/register', component: register }
      ],
      linkActiveClass: 'myactive'  //这个属性是定义链接生效的默认类，默认是router-link-active
    })

    let vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router: routerObj // 将路由规则对象，注册到vm实例上，用来监听URL地址的变化，然后展示对应的组件
    });
  </script>
</body>
</html>
```

## 在路由规则中定义参数

第一种：在路径中直接使用`?id=11`进行查询，路由不用改变

```text
{path: '/login', component: login}
```

通过 `this.$route.query`来获取路由中的参数：

```javascript
var login= Vue.extend({
   template: '<h1>登陆组件 --- {{this.$route.query.id}}</h1>'
});
```

第二种：在规则中定义参数：

```javascript
{ path: '/register/:id', component: register }
```

通过 `this.$route.params`来获取路由中的参数：

```javascript
var register = Vue.extend({
   template: '<h1>注册组件 --- {{this.$route.params.id}}</h1>'
});
```

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

## 使用 `children` 属性实现路由嵌套

```markup
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
        { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向
        {
          path: '/account',
          component: account,
          children: [ // 通过 children 数组属性，来实现路由的嵌套
            { path: 'login', component: login }, // 注意，子路由的开头位置，不要加 / 路径符
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

## `watch`和`computed`监听事件

考虑一个问题：想要实现 `名` 和 `姓` 两个文本框的内容改变，则全名的文本框中的值也跟着改变；（用以前的知识如何实现？？？）

```markup
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
</head>
<body>
<div id='app'>
    <!-- 分析 1、监听到文本框的改变-->
    <h2>第一种，键盘事件监听，配合getFullname改变fullname</h2>
    <input type="text" v-model="firstname" @keyup="getFullname"> +
    <input type="text" v-model="lastname" @keyup="getFullname"> =
    <input type="text" v-model="fullname">
    <h2>第二种，使用watch监听</h2>
    <input type="text" v-model="firstname2"> +
    <input type="text" v-model="lastname2"> =
    <input type="text" v-model="fullname2">
    <h2>第三个，使用computed计算属性</h2>
    <input type="text" v-model="firstname3"> +
    <input type="text" v-model="lastname3"> =
    <input type="text" v-model="fullname3">
</div>
</body>
<script>
    // 实例化vue对象
    let vm = new Vue({
        // 绑定对象
        el: '#app',
        data: {
            firstname: '',
            lastname: '',
            fullname: '',
            firstname2: '',
            lastname2: '',
            fullname2: '',
            firstname3: '',
            lastname3: '',

        },
        methods: {
            getFullname() {
                this.fullname = this.firstname + '-' + this.lastname
            }
        },
        // 使用watch可以监视 data 中指定数据的变化，然后触发这个 watch 中对应的function处理函数
        // 只要指定的自变量改变了，那监听的因变量也会改变
        watch: {
            'firstname2': function (newVal, oldVal) {
                console.log("new:" + newVal + "--old:" + oldVal)
                this.fullname2 = this.firstname2 + '-' + this.lastname2
            },
            'lastname2': function (newVal) {
                this.fullname2 = this.firstname2 + '-' + newVal
            }
        },
        // 在 computed 中可以定义一些 属性，这些属性，叫做【计算属性】，
        // 计算属性的本质是一个方法，只不过，我们在使用这些计算属性的时候，把它们的名称，直接当做属性来使用的；并不会把计算属性当做方法来调用
        // 注意：
        // 计算属性，在引用的时候，一定不要加（）去调用，直接把他当做普通属性去使用就好
        // 只要计算属性内部所用到的 data 发生变化，就会立即从新计算这个属性的值
        // 计算属性的求值结果，会被缓存，方便下次直接使用，如果计算属性中方法内部的data数据没有发生变化则不重新计算
        computed: {
            'fullname3': function () { //computed计算属性，一定要有return返回值
                return this.firstname3 + "-" + this.lastname3
            },
        }
    })
</script>
</html>
```

###  **watch-监听路由地址的改变**

```markup
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'><title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>
<body>
<div id='app'>
    <router-link to="/login">登录</router-link>
    <router-link to="/register">注册</router-link>
    <router-view></router-view>
</div>
</body>
<script>
    // 创建组件模板对象
    let login = {
        template: "<h1>登录组件</h1>",

    }
    let register = {
        template: "<h1>注册组件</h1>",
    }
    let router = new VueRouter({
        routes: [
            {path: '/', redirect: 'login'},
            {path: '/login', component: login},
            {path: '/register', component: register}
        ]
    })
    // 实例化vue对象
    let vm = new Vue({
        // 绑定对象
        el: '#app',
        data: {},
        methods: {},
        router,
        watch: {
            "$route.path": function (newVal, oldVal) {
                console.log("new:" + newVal + "--old:" + oldVal)
                if (newVal == "/login") {
                    console.log("欢迎进入登录页面")
                } else if (newVal == "/register") {
                    console.log("欢迎进入注册页面")
                }
            }
        }
    })
</script>
</html>
```

## `watch`、`computed`和`methods`之间的对比

1. `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
2. `methods`方法表示一个具体的操作，主要书写业务逻辑；
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；

## `nrm`的安装使用

作用：提供了一些最常用的NPM包镜像地址，能够让我们快速的切换安装包时候的服务器地址； 什么是镜像：原来包刚一开始是只存在于国外的NPM服务器，但是由于网络原因，经常访问不到，这时候，我们可以在国内，创建一个和官网完全一样的NPM服务器，只不过，数据都是从人家那里拿过来的，除此之外，使用方式完全一样；

1. 运行`npm i nrm -g`全局安装`nrm`包； 

2. 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址； 

3. 使用`nrm use npm`或`nrm use taobao`切换不同的镜像源地址；

## 相关文件

1. [URL中的hash（井号）](http://www.cnblogs.com/joyho/articles/4430148.html)


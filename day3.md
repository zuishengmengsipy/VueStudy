# day3

## 定义Vue组件

> 组件可以理解成可以复用的vue对象

什么是组件： 组件的出现，就是为了拆分Vue实例的代码量的，能够让我们以不同的组件，来划分不同的功能模块，将来我们需要什么样的功能，就可以去调用对应的组件即可； 组件化和模块化的不同：

* 模块化： 是从代码逻辑的角度进行划分的；方便代码分层开发，保证每个功能模块的职能单一；
* 组件化： 是从UI界面的角度进行划分的；前端的组件化，方便UI组件的重用；

> Vue组件需要注册后才可以使用。注册有全局注册和局部注册两种方式。

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
        <!--<my-com1></my-com1>无效，要在外面套有被vue绑定的div等-->
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
            template:'<h3>这是用Vue.extend创建的组件</h3>'
            //template的DOM结构必须被一个元素包含，缺少<div></div>会无法渲染并报错。
            //内部必须只有一个根节点
        })
        // 1.2、使用Vue.component('组件的名称',创建出来的的组件模板对象)
        // 定义的时候可以用驼峰法，但是调用时必须使用“-”，否则无效
        Vue.component('myCom1',com1)
        
        // 2.1、合并写法
        Vue.component('mycom2',Vue.extend({
            // 通过temloate属性，指定了组件要展示的HTML结构
            template:'<h3>这是用Vue.component两步合并创建的组件</h3>'
        }))
        // 2.2、简化写法
        Vue.component('mycom3',{
            template:'<h3>这是用Vue.component一步创建的组件</h3>'
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

在组件中，**`data`必须被定义为一个方法**，例如：

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
    // 2、组件的data和实例的data不一样，实例data可以为一个对象，组件的data必须是一个方法
    // 3、组件中的data 除了必须为一个方法外，这个方法内部，还必须返回一个对象才行
    Vue.component('mycom1',{
        template:'<h1>这是全局组件.data:{{msg}}</h1>',
        data:function(){
            return {
                msg:'这是data里的信息'
            }
        }
    })
    let dataObj = { count:8 }
    Vue.component('counter',{
        template:'#counter',
        data:function(){
            // 因为组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，
            // 例如 data、computed、watch、methods 以及生命周期钩子等。
            // 仅有的例外是像 el 这样根实例特有的选项。
            
            // return dataObj
            //如果使用这句的话，按一下按钮，三个计数器同时计数因为data一样，
            //返回的也是同一个计数对象
            return { count:8 }
            //使用下面这种是因为要每次组件生成标签，data是不同的对象，互不影响
            //因为虽然data一样，但是data每次返回的对象不一样
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

## 组件的切换（:is的使用）

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

**使用`:is`属性来切换不同的子组件，并添加切换动画步骤详解**

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

**`:is`的其他用法**

Vue组件的模板在某些情况下会受到HTML的限制，比如`<table>`内规定只允许是`<tr>`、`<td>`、`<th>`等这些表格元素，所以在`<table>`内直接使用组件时无效的。**这种情况下，可以使用特殊的`is`属性来挂载组件**。

```markup
<div id="app">
    <table>
        <tbody :is="my-component"></tbody>
    </table>
</div>

Vue.component('my-component', {
    template: `<div>这里是组件内容</div>`
});
```

常见的限制元素还有`<ul>`、`<ol>`、`<select>`。

## 父子组件之间的传值

> 组件就是可复用的vue实例，子组件是套在new vue里面的组件，子组件中默认无法访问到父组件中的data上的数据（使用props即可）和methods 中的方法（使用$emit）

1、父组件可以使用 props 把数据传给子组件。  
2、子组件可以使用 $emit 触发父组件的自定义事件

vm.$emit\( event, arg \) //触发当前实例上的事件

vm.$on\( event, fn \);//监听event事件后运行 fn；

**vm.$emit\(event,\[args\]\)**

参数： {string} event \[…args\] 触发**当前实例（自己组件的vue实例找不到就从父组件上找）**上的事件。附加参数都会传给监听器（**vm.$on**）回调。

**vm.$on\(event,callback\)**

参数： {string \| Array} event \(数组只在 2.2.0+ 中支持\) {Function} callback

用法： 监听当前实例上的自定义事件。事件可以由vm.$emit触发。回调函数会接收所有传入事件触发函数的额外参数。

上面是官方的api，很简洁，粗略一看很容易误解，这里主要是$on的用法，这里回过头去看一下$on的用法，$on是监听当前实例上的自定义事件，这个自定义事件可以由$emit来触发，$on回调函数接收的msg便是$emit方法第二个参数传过来的值。当然你也可以在回调函数里不使用msg参数而执行其他操作。

```javascript
vm.$on('test', function (msg) {
 console.log(msg)
}) 
vm.$emit('test', 'hi')
// => "hi" 1 2 3 4 5
```

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
    <!-- 传入步骤：父组件，引用子组件时，通过属性绑定（v-bind:）的形式, 
    即把数据以属性绑定的形式，传递到子组件内部，供子组件使用 -->
    <com1 v-bind:parentmsg="msg"></com1><!--子组件-->
    <!-- v-bind绑定后也不能立即使用，需要在子组件的props数组里定义一下 -->
</div>
<script>
    // 创建 Vue 实例（父组件），得到 ViewModel
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
                data() { 
// 注意：子组件中的 data 数据，并不是通过父组件传递过来的，而是子组件自身私有的，
// 比如：子组件通过 Ajax，请求回来的数据，都可以放到 data 身上；
// data 上的数据，都是可读可写的；props中的数据，都是只读的，无法重新赋值
                    return {
                        title: '123',
                        content: 'qqq'
                    }
                },
                template: '<h1 @click="change">这是子组件 --- {{ parentmsg }}</h1>',
                // 注意：组件中的所有props中的数据，都是通过父组件传递给子组件的
                // props中的数据，都是只读的，无法重新赋值
                props: ['parentmsg'], 
                // 把父组件传递过来的parentmsg属性，先在props数组定义一下，
                // 这样才能使用这个数据
                directives: {},
                filters: {},
                components: {},
                methods: {change() {this.parentmsg = '被修改了'}}//传值
            }
        }
    });
</script>
</body>
</html>
```

### 子向父传值：父向子传递函数然后借此函数子组件向父组件传值

**父组件如何向子组件传递函数？：**使用的是事件绑定机制：v-on,调用是在子组件里使用$emit进行调用

**子组件向父组件传值用以下方法实现：**

* 原理：父组件将方法的引用传递到子组件内部，子组件在内部调用父组件传递过来的方法，同时把要发送给父组件的数据，在调用方法的时候当作参数传递进去。
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
      <input type="button"
       value="这是子组件中的按钮，点击触发父组件传递过来的func方法" @click="myclick">
    </div>
  </template>

  <script>
    // 定义了一个字面量类型的组件模板对象
    let com2 = { //子组件
      template: '#tmpl', 
      // 通过指定了一个Id,表示去加载这个指定Id的template元素中的内容，当作组件的HTML结构
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

    let vm = new Vue({ //父组件
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

### 评论列表案例

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

### Prop 类型 <a id="Prop-&#x7C7B;&#x578B;"></a>

到这里，我们只看到了以字符串数组形式列出的 prop：

```text
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

但是，通常你希望每个 prop 都有指定的值类型。可以以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型

```text
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

这不仅为你的组件提供了文档，还会在它们遇到错误的类型时从浏览器的 JavaScript 控制台提示用户。你会在这个页面接下来的部分看到[类型检查和其它 prop 验证](https://cn.vuejs.org/v2/guide/components-props.html#Prop-%E9%AA%8C%E8%AF%81)。

进阶

{% embed url="https://blog.csdn.net/howgod/article/details/90695332" %}

{% embed url="https://segmentfault.com/a/1190000015199363" %}



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


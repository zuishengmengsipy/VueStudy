# vue组件

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

参数： {string} event \[…args\] 触发**当前实例（自己组件的vue实例找不到就从父组件上找，都找不到则创建该事件）**上的事件。附加参数都会传给监听器（**vm.$on**）回调。

**vm.$on\(event,callback\)**

参数： {string \| Array} event \(数组只在 2.2.0+ 中支持\) {Function} callback

用法： 监听当前实例上的自定义事件。事件可以由vm.$emit触发。回调函数会接收所有传入事件触发函数的额外参数。

上面是官方的api，很简洁，粗略一看很容易误解，这里主要是$on的用法，这里回过头去看一下$on的用法，$on是监听当前实例上的自定义事件，这个自定义事件可以由$emit来触发，$on回调函数接收的msg便是$emit方法第二个参数传过来的值。当然你也可以在回调函数里不使用msg参数而执行其他操作。

```javascript
vm.$on('test', function (msg) {
 console.log(msg)
 this.talk = msg 
//注意这里的this，因为this在function里，所以谁触发了function，this就是谁，这里是vm触发了function
}) 
vm.$emit('test', 'hi')
// => "hi"
```

### 父向子传值

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
</head>
<body>
<div id="app">
    <!-- 传入步骤：父组件，引用子组件时，通过属性绑定（v-bind:）的形式,
    即把数据以属性绑定的形式，传递到子组件内部，供子组件使用 -->
    <input type="text" v-model="msg">
    <com1 v-bind:parentmsg="msg"></com1><!--子组件-->
    <!-- v-bind绑定后也不能立即使用，需要在子组件的props数组里定义一下 -->
    <!--这里用v-model绑定了父级的数据msg，当通过输入框任意输入时-->
    <!--子组件接收到的props["parentmsg"]也会实时响应，并更新组件模板。-->
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
                        content: 'qqq',
                        m:this.parentmsg
                    }
                },
                template: '<h1 @click="change">这是子组件 --- {{ parentmsg }}--，这是子组件的m--{{m}}--</h1>',
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
    <!-- 父组件向子组件传递函数，使用的是事件绑定机制；v-on -->
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

### 非父子组件通信

**单页面**

非父子组件的传值，vue官网指出，可以使用一个空vue实例作为事件中央线！

```markup
<html>
<head>
  <meta charset='utf-8'>
  <title></title>
  <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
</head>
<body>
<div id='app'>
<sister></sister>
<brother></brother>
</div>
</body>
<script>
  let me = new Vue(); //中间联系人,空的就行
  let sister = {
    template:`<div>
              <h1>这是姐姐组件</h1>
              <button @click="my_click">点击后通过我给哥哥留言</button>
              </div>`,
    methods: {
      my_click(){
        //点击通过我给哥哥留言
        me.$emit("call", "今晚等我") // 通过中间联系人进行联系
      }
    }
  }

  let brother = {
    template:`
<div>
<h1>这是哥哥组件</h1>
<p>姐姐告诉你：{{say}}</p>
</div>`,
    data(){
      return{
        say:""
      }
    },
    mounted(){//组件加载完成后执行的方法
      let that = this;//添加that定义使之成为brother，原因见下
      me.$on("call", function (data) {
        // console.log(data)
        // this.say = data//不能使用this这里的this是me，这里需要是brother
        that.say = data//添加that定义使之成为brother
      })
    }
  }
  // 实例化vue对象
  let vm = new Vue({
    // 绑定对象
    el: '#app',
    components:{
      sister:sister,
      brother:brother
    },
    methods: {
      show(data){
        console.log("dsfds")
      },
    }
  })
</script>
</html>
```

**小項目中**

公共实例文件bus.js，作为公共数控中央总线，**用vue-cli搭建的项目中，公共实例只能是js文件，不能是vue文件**

```text
import Vue from "vue";
export default new Vue();
```

第一个组件 first.vue

```text
import Bus from '../bus.js';
export default {
  name: 'first',
  data () {
    return {
      value: '我来自first.vue组件！'
    }
  },
  methods:{
    add(){// 定义add方法，并将msg通过txt传给second组件
      Bus.$emit('txt',this.value);
    }
  }
}
```

**第二个组件second.vue**

```text
import Bus from '../bus.js';
export default {
  name: 'second',
  data () {
    return {
    }
  },
  mounted:function(){
    Bus.$on('txt',function(val){//监听first组件的txt事件
      console.log(val);
    });
  }
}
```

这样，就可以在第二个非父子关系的组件中，通过第三者bus.js来**获取**到第一个组件的value，为什么说获取，因为将mounted进行修改后，可以更改第二个组件的属性内容，但是无法将内容渲染到网页上，不知道为啥

```text
mounted: function () {
  let that = this;//添加that定义使之不是bus，因为下面的this是bus，不是第二个组件
  bus.$on('txt', function (val) {//监听first组件的txt事件
    // console.log(val);
    that.msg = val;
    console.log(that.msg);//这里已经变化，但是页面没有刷新插值，不知道为什么，强制刷新也不行
  });
}
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

父组件可以使用 props 把数据传给子组件，到这里我们只看到了以字符串数组形式列出的 prop：

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

`props`中声明的数据与组件`data`函数中`return`的数据主要区别就是`props`的数据来自父级，而`data`中的是组件自己的数据，作用域是组件本身，这两种数据都可以在模板`template`及计算属性`computed`和方法`methods`中使用。



进阶

{% embed url="https://blog.csdn.net/howgod/article/details/90695332" %}

{% embed url="https://segmentfault.com/a/1190000015199363" %}

## 混合

对于组件的重复功能和数据的储存器，可以使用Mixins的来引用重复代码，对于项目来说，一个组件一个文件，混合的作用较小

```javascript
<html>
<head>
    <meta charset='utf-8'>
    <title></title>
    <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
</head>
<body>
<div id="app">
    <PopUp></PopUp>
    <ToolTip></ToolTip>
</div>
</body>
<!--点击显示和隐藏  提示框的显示和隐藏-->
<!--html 代码-->
<script>
    let base = {
        data: function () {
            return {
                visible: true,
            }
        },
        methods: {
            show: function () {
                this.visible = true
            },
            hide: function () {
                this.visible = false
            }
        }
    }

    Vue.component('popup', {
        template:`
            <div>
            <button @click="show">PopUp show</button>
            <button @click="hide">PopUp hide</button>
            <div v-if="visible"><p>hello everybody</p></div>
            </div>
        `,
        // data: function () {
        //     return {
        //         visible: true, //默认是显示的
        //     }
        // },
        mixins: [base]
    });
    Vue.component('tooltip', {
        template: `
            <div>
            <div @mouseenter="show" @mouseleave="hide">显示隐藏</div>
            <div v-if="visible"><p>鼠标移入显示，移出隐藏</p></div>
            </div>
        `,
        mixins: [base]
    });

    new Vue({
        el: "#app",
    })
</script>
</html>
```

## 插槽

为组件做内容分发的，使用相同的组件，替换其中的一部分内容，而且没有slot标签，组件里面写任何东邪都无法显示

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="ret">
    <coma>
        <h2 slot="title">大头儿子</h2>
        <h4 slot="article">头很大</h4>
    </coma>
    <coma>
        <h2 slot="title">小头爸爸</h2>
        <div slot="article">头很小</div>
        <!--dsafdsafdsafs-->
        <!--<p>A paragraph for the main content.</p>-->
        <!--<p>And another one.</p>-->
    </coma>
    <coma><!--vue2.6以上支持v-slot具名插槽，且标签只能用template，效果一样-->
        <template v-slot:title><h2>大头儿子</h2></template>
        <template v-slot:article>头很大</template>
        <!--dsafdsafdsafs-->
        <!--<p>A paragraph for the main content.</p>-->
        <!--<p>And another one.</p>-->
        <!--<div v-slot:article>头很大</div>  标签只能是template，别的不行-->
    </coma>
</div>
<script>
    let coma = {
        template:`<div>
                    <h1>这是一个组件</h1>
                    <slot name="title"></slot>
                    <slot name="article"></slot>
                    <!--<slot></slot>-->
                    没有名字的叫默认插槽，coma的注释两行解注释也无法显示 
                    </div> `,
    };
    const ret = new Vue({
        el:"#ret",
        components: {
            coma:coma
        }
    })
</script>
</body>
</html>
```

### 具名插槽（接上例代码）

```markup
<coma><!--vue2.6以上支持v-slot具名插槽，且标签只能用template，效果一样-->
    <template v-slot:title><h2>大头儿子</h2></template>
    <template v-slot:article>头很大</template>
    <!--dsafdsafdsafs-->
    <!--<p>A paragraph for the main content.</p>-->
    <!--<p>And another one.</p>-->
    <!--<div v-slot:article>头很大</div>  标签只能是template，别的不行-->
</coma>
```

 就是这么简单，插槽的名字现在通过 `v-slot:slotName` 这种形式来使用。 `v-slot` 只能添加到 `<template>` 或自定义组件上，这点与弃用的 slot 属性不同

 **Tips: 没有名字的 `<slot>` 隐含有一个 `"default"` 名称**

例如，上面的**被注释的默认插槽**，如果你想显示调用的话，可以这样：

```markup
  <template v-slot:default>
    dsafdsafdsafs
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>
```

### 作用域插槽

有时候，我们想在父组件中访问子组件内部的一些可用数据。例如，假设有一个下面**模板（template）的 `<current-user>` 组件**：

```text
template:`<span>
        <slot>{{ user.lastName }}</slot>
        </span>`
```

我们可能想用用户的名字来替换掉插槽里面的姓，于是我们这样写：

```text
<current-user>
  {{ user.firstName }}
</current-user>
```

很不幸，上面这段代码不能如你预期那样工作，因为当前代码的作用域环境是在父组件中，所以它访问不了 `<current-user>` 内部的数据。

为了解决这个， 我们可以在 `<current-user>` 内部的 `<slot>` 元素上动态绑定一个 `user` 对象属性：

```text
<span>
  <!-- 完整 v-bind:user 下面是简写形式 -->
  <slot :user="user">
    {{ user.lastName }}
  </slot>
</span>
```

绑定到 `<slot>` 元素上的属性我们称之为 **slot props**。现在，在父作用域中，我们可以通过 `slot-scope` 来访问 `user` 数据了：

```text
<current-user>
  <template slot-scope="slotProp">
    {{ slotProp.user.firstName }}
  </template>
</current-user>
```

同样的，我们使用 `v-slot` 重构上面的代码：

```text
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

或者直接作用在 `<current-user>` 上的写法：

```text
<!-- 省略默认插槽名字 -->
<current-user v-slot="slotProp">
  {{ slotProp.user.firstName }}
</current-user>

<!-- 显示调用默认插槽名字 -->
<current-user v-slot:default="slotProp">
  {{ slotProp.user.firstName }}
</current-user>
```

> 在这个栗子中，我们选择 `slotProp` 作为我们的 **slot props** 名字，但你可以使用你喜欢的任何名字。

### 单个默认插槽的缩写形式

在上述情况下，当且仅当提供了默认插槽内容时，我们可以使用 `v-slot` 直接作用在组件上：

```text
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

我们可以简化上面的的默认插槽写法：

```text
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

请注意了，默认插槽的缩写语法不能与具名插槽混用：

```text
<!-- 控制台将报警告：-->
<!-- To avoid scope ambiguity, the default slot should also use <template> syntax when there are other named slots. -->
<!-- 意思就是说，为了避免作用域模糊 -->
<!-- 当有其他具名插槽时，默认插槽也应当使用 '<template>' 模板语法 -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```

于是，上面的代码，我们改写成：

```text
<current-user>
 <!-- 两种写法均可 -->
  <!--<template v-slot="slotProps">
    {{ slotProps.user.firstName }}
  </template>-->
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```

### 插槽内容的解构赋值

在 Vue 代码内部，我们传递的 slotProps 其实就是函数的一个单一参数：

```text
function (slotProps) {
  // ... slot content ...
}
```

这也就意味着 `v-slot` 的值只要满足函数参数定义的 JavaScript 表达式的都可以接受。因此，在支持的环境（单文件或现代浏览器）中，你还可以使用 **ES2015** 解构语法来提取特定的插值内容，例如：

```text
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```

代码看起来更简洁对吧。我们还可以重命名解构变量：

```text
<current-user v-slot="{ user: person }">>
  {{ person.firstName }}
</current-user>
```

这给了我们很多自由操作的空间，你甚至可以自定义回退内容，以便在未定义插值情况下使用：

```text
<current-user v-slot="{ user = { firstName: 'Guest' } }">>
  {{ user.firstName }}
</current-user>
```

### 动态插槽名称

> 2.6.0+ 新增

[动态指令参数](https://link.juejin.im/?target=https%3A%2F%2Fvuejs.org%2Fv2%2Fguide%2Fsyntax.html%23Dynamic-Arguments) 也适用于 `v-slot` ，允许我们定义动态插槽名称：

```text
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
复制代码
```

### 命名插槽简写

> 2.6.0+ 新增

与 `v-on` 和 `v-bind` 类似，`v-slot` 也有一个简写，即使用 `#` 代替 `v-slot`。例如， `v-slot:header` 简写成 `#header`:

```text
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
复制代码
```

和其他指令一样，只有在提供参数时才能使用简写形式，下面的写法是无效的：

```text
<!-- 将会触发一个控制台警告 -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>
复制代码
```

也就是说，如果你想使用简写语法，则务必指定插值的名字：

```text
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```

链接：[https://juejin.im/post/5c64e11151882562e4726d98](https://juejin.im/post/5c64e11151882562e4726d98)

[https://blog.csdn.net/weixin\_34357436/article/details/91435871](https://blog.csdn.net/weixin_34357436/article/details/91435871)

## 使用 `this.$refs` 来获取元素和组件（获取DOM）

第一步：给标签元素加ref属性

第二步：用this.$refs.属性值来获取

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

## Vue.$set解决视图信息不能及时变化的情况

这里定义了一个列表数据，通过三个不同的按钮来控制列表数据。  
调用方法：Vue.set\( target, key, value \)  
     target：要更改的数据源\(可以是对象或者数组\)  
     key：要更改的具体数据（数组下标）  
     value ：重新赋的值

```markup
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<div id="app2">
    <p v-for="item in items" :key="item.id">
        {{item.message}}
    </p>
    <button class="btn" @click="btn2Click()">动态赋值</button><br/>    
    <button class="btn" @click="btn3Click()">为data新增属性</button>
</div>
<script src="../../dist/vue.min.js"></script>
<script>
var vm2=new Vue({
    el:"#app2",
    data:{
        items:[
            {message:"Test one",id:"1"},
            {message:"Test two",id:"2"},
            {message:"Test three",id:"3"}
        ]
    },
    methods:{
        btn2Click:function(){//解决修改问题
            Vue.set(this.items,0,{message:"Change Test",id:'10'})
            //需要更改项，下标，修改值
        },
        btn3Click:function(){//添加
            var itemLen=this.items.length;
            Vue.set(this.items,itemLen,{message:"Test add attr",id:itemLen});
        }
    }
});
</script>
</body>
</html>
```

此时页面是这样![](//upload-images.jianshu.io/upload_images/16300678-6cce499618f4ab59.png?imageMogr2/auto-orient/strip|imageView2/2/w/171/format/webp)

点击第一个按钮后运行methods中的btn2Clcick方法，将Test one更改为Change Test  
![](//upload-images.jianshu.io/upload_images/16300678-31418e431ee67019.png?imageMogr2/auto-orient/strip|imageView2/2/w/674/format/webp)  
 运行后的结果：此时列表中第一列的Test one已经变成了Change Test  
![](//upload-images.jianshu.io/upload_images/16300678-31d54a9ba042f6ab.png?imageMogr2/auto-orient/strip|imageView2/2/w/170/format/webp)

这里警惕一种情况： 当写惯了JS之后，有可能我会想改数组中某个下标的中的数据我直接this.items\[XX\]就改了，如：

```javascript
btn2Click:function(){
  this.items[0]={message:"Change Test",id:'10'}
}
```

我们来看看结果：![](//upload-images.jianshu.io/upload_images/16300678-5669dc2f032a1101.png?imageMogr2/auto-orient/strip|imageView2/2/w/649/format/webp)

这种情况，是Vue文档中明确指出的注意事项，由于 JavaScript 的限制，Vue 不能检测出数据的改变，所以当我们需要动态改变数据的时候，Vue.set\(\)完全可以满足我们的需求。

仔细看的同学会问了，这不是还有一个按钮吗，有什么用？

我们还是直接看：![](//upload-images.jianshu.io/upload_images/16300678-ca543b889ff4d1a3.png?imageMogr2/auto-orient/strip|imageView2/2/w/556/format/webp)  


这是初始的列表数据，数据里面有三个对象  
 点击之后：![](//upload-images.jianshu.io/upload_images/16300678-54498bed8c453e91.png?imageMogr2/auto-orient/strip|imageView2/2/w/601/format/webp)  
 这里可以看出，Vue.set\(\)不光能修改数据，还能添加数据，弥补了Vue数组变异方法的不足  
链接：https://www.jianshu.com/p/e6e8c45e7fd6


# day1

## 什么是Vue.js

* Vue.js 是目前最火的一个前端框架，React是最流行的一个前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于Weex）
* Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！
* Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）
* 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；

## 为什么要学习流行框架

* 企业为了提高开发效率：在企业中，时间就是效率，效率就是金钱；
  * 企业中，使用框架，能够提高开发的效率；
* 提高开发效率的发展历程：原生JS -&gt; Jquery之类的类库 -&gt; 前端模板引擎 -&gt; Angular.js / Vue.js（能够帮助我们减少不必要的DOM操作；提高渲染效率；双向数据绑定的概念【通过框架提供的指令，我们前端程序员只需要关心数据的业务逻辑，不再关心DOM是如何渲染的了】）
* 在Vue中，一个核心的概念，就是让用户不再操作DOM元素，解放了用户的双手，让程序员可以更多的时间去关注业务逻辑；
* 增强自己就业时候的竞争力
  * 人无我有，人有我优
  * 你平时不忙的时候，都在干嘛？

## 框架和库的区别

* 框架：是一套完整的解决方案；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。
  * node 中的 express；
* 库（插件）：提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。
  * 1. 从Jquery 切换到 Zepto
  * 1. 从 EJS 切换到 art-template

### 使webstorm不对含vue的html报错的方式：

在改文件当前目录下安装有vue库即可，可以使用npm命令，先在当前目录下使用npm init初始化一个node项目然后npm install vue即可支持vue语法不报错

## Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

* MVC 是后端的分层开发概念；
* MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel
* 为什么有了MVC还要有MVVM

![MVVM](.gitbook/assets/01.mvc-he-mvvm-de-guan-xi-tu-jie.png)

### Vue.js 基本代码 和 MVVM 之间的对应关系

{% code title="第一个Vue" %}
```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <!-- 引入vue.js -->
    <!--<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>-->
    <script src="./lib/vue.js"></script>
    <title>Title</title>
</head>
<body>
<!--Vue实例控制的这个区域就是MVVM中的V-->
<div id="app">
    <p>{{ msg }}</p>
</div>

<script>
    // 当我们导入包后，在浏览器的内存中就多了一个vue的构造函数
    // 我们new的这个vm对象，就是我们MVVM中的VM调度者
    var vm = new Vue({
        // *****el的  #对应id，.对应类  ****
        el: '#app',  //表示当前我们new的这个Vue实例，要控制页面上的哪个区域
        // 这里的data就是MVVM中的M，专门用来保存每个页面的数据的
        data:{  //data属性中，存放的是el中要用的数据
            msg:'hello world!'  //通过Vue提供的指令，很方便的就把数据渲染到了页面上，程序员不在动手操作DOM元素了
            //前端的Vue等框架，不提倡我们去手动操作DOM元素了
        }
    })
</script>
</body>
</html>
```
{% endcode %}

### Vue`基本的代码结构、插值表达式、v-cloak、v-text、v-html`

{% code title="v-cloak,v-text,v-html的学习.html" %}
```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        [v-cloak] {
            display: none;
        }
    </style>
</head>
<body>
<div id="app">
    <!-- 使用v-cloak可以解决插值表达式闪烁问题（网络差时替换慢） -->
    <h1 v-cloak>===={{msg}}====</h1>
    
    <!-- 默认 v-text 没有闪烁问题的 -->
    <!-- v-text会覆盖元素中原本全部的内容 -->
    <h2 v-text="msg"></h2>

    <div>{{msg2}}</div>
    <!-- v-text不会解析html -->
    <div v-text="msg2"></div>
    <!-- v-html会解析变量中的html -->
    <!-- v-html也会全部覆盖原本的内容 -->
    <div v-html="msg2"></div>
</div>
</body>
<!-- 引入vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    // 实例化vue对象
    let vm = new Vue({
        // 绑定对象
        el: '#app',
        data: {
            msg: 'hello vue.js',
            msg2: '<h3>这里是h3的内容<h3>'
        }
    })
</script>
</html>
```
{% endcode %}

### Vue指令之`v-bind`的和`v-on`

**指令名`v-bind`简化指令`:`**在绑定的时候，拼接绑定内容：`:title="btnTitle + ', 这是追加的内容'"`

`v-bind`进行绑定的是属性，后面的字符串已经变成了**表达式**不再是字符串了

**指令名`v-on`简化指令`@`**

{% code title="v-bind,v-on的学习.html" %}
```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="app">
        <!-- v-bind:是vue中提供标签里绑定属性的指令,把后面属性值变成表达式,并且支持拼接 -->
        <!-- 属性里不能使用插值表达式，因为会全当成字符串 -->
        <input type="button" value="按钮1" v-bind:title="mytitle+'123'" v-on:click="alert">
        <!-- 按钮鼠标悬停显示这是自定义title123 -->
        <input type="button" value="按钮2" v-bind:title="mytitle+'123'+mytitle" v-on:click="alert">
        <!-- v-bind可以简写为一个冒号（:） -->
        <!-- v-on可以简写为一个@ -->
        <input type="button" value="按钮2" :title="mytitle" @click="alert">
        <!-- @可以后面直接接语句类似@click="comName='login'" ，这样可以直接修改vue对象里data的属性值-->
        <!-- 上面一行的用法具体表现在动画，获取焦点的动画切换等地方 -->
    </div>

</body>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<script>
    // 实例化vue对象
    let vm = new Vue({
        // 绑定对象
        el:'#app',
        data:{
            mytitle:'这是自定义title'
        },
        // 方法
        methods:{
            // 自定义事件
            alert:function(){
                alert('hello world!');
                console.log(this)  //Vue
            },
            // ES6的写法
            // alert(){
            //     alert('hello world!');
            //     console.log(this)  //Vue
            // }
            // 箭头函数的this是Window
            // 而在上面那些函数的this是当前Vue对象
            // alert:()=>{
            //     console.log(this)  //Window
            // },
        }
    })
</script>

</html>
```
{% endcode %}

### 跑马灯效果

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>跑马灯效果</title>
</head>
<body>
<div id="app">
    <input type="button" value="跑起来" v-on:click="star">
    <input type="button" value="停下来" @click="end">
    <!--事件绑定操作还能加个括号，这样就能传参了-->
    <!--<input type="button" value="停下来" @click="end()">-->
    <h3>{{msg}}</h3>
</div>
</body>
<script src="./vue.js"></script>

<script>
    let vm = new Vue({
        el:'#app',
        data:{
            msg:"这是跑马灯的效果，跑起来",
            myInterval:null  //不定义下面会有未定义错误
        },
        // methods:{
        //     star:function(){
        //         // console.log(this.msg)
        //         console.log(this)  //this是vue
        //         // 如果已经定义，则不再向下执行
        //         if(this.myInterval){
        //             return  //如果不加这个判断，那么跑马速度越来越快
        //         }
        //         let _this = this  //匿名函数this是windows，所以要替换this
        //         this.myInterval = setInterval(function(){ //这个是匿名函数
        //             // console.log(this)  //this是window
        //             // console.log(_this)  //_this是vue
        //             let first = _this.msg.substring(0,1)
        //             let rest = _this.msg.substring(1)
        //             _this.msg = rest+first
        //         },200)
        //     },
        //     end(){
        //         console.log(this.myInterval)
        //         clearInterval(this.myInterval)
        //         this.myInterval = null
        //     }
        // }
        methods:{
            star:function(){
                // console.log(this.msg)
                console.log(this);  //this是vue
                // 如果已经定义，则不再向下执行
                if(this.myInterval){
                    return  //如果不加这个判断，那么跑马速度越来越快
                }
                // setInterval设置一个周期的计时器事件
                this.myInterval = setInterval(()=>{
                    console.log(this); //匿名函数this是vue
                    let first = this.msg.substring(0,1);
                    let rest = this.msg.substring(1);
                    this.msg = rest+first
                },200) // 每点击一次把成功的结果给myInterval
                // 下次点击时判断myInterval，如果已有一个计时器时间则不执行
            },
            end(){
                clearInterval(this.myInterval);
                this.myInterval = null
            }
        }
    })
</script>

</html>
```

### 事件修饰符：

* .stop 阻止冒泡
* .prevent 阻止默认事件
* .capture 添加事件侦听器时使用事件捕获模式
* .self 只当事件在该元素本身（比如不是子元素）触发时触发回调
* .once 事件只触发一次

```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <style>
            .inner{
                width: 100%;
                height: 150px;
                background: lawngreen;
            }
        </style>
    </head>
    <body>
        <div id='app'>
            <h2>原来的（有冒泡）</h2>
            <div class="inner" @click="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>

            <h2>.stop阻止冒泡</h2>
            <div class="inner" @click="div1Handler">
                <!-- 使用.stop阻止冒泡 -->
                <input type="button" value="点我" @click.stop="btnHandler">
            </div>

            <h2>.prevent阻止默认行为，阻止a标签的默认跳转</h2>
            <a href="http://baidu.com" @click.prevent="linkClick">百度一下</a>

            <h2>.capture:添加事件侦听器时使用事件捕获模式</h2>
            <!-- 先外层后内层 -->
            <div class="inner" @click.capture="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>

            <h2>.self:只当事情在该元素本身（比如不是子元素）触发时触发回调</h2>
            <div class="inner" @click.self="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>

            <h2>.once:时间只能触发一次</h2>
            <div class="inner" @click.once="div1Handler">
                <input type="button" value="点我" @click="btnHandler">
            </div>
            <a href="http://baidu.com" @click.prevent.once="linkClick">百度一下</a>
        </div>
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
            },
            methods:{
                div1Handler(){
                    console.log("这是触发了div1的click事件")
                },
                btnHandler(){
                    console.log("这是触发了btn的click事件")
                },
                linkClick(){
                    console.log("点击了百度一下")
                }
            }
        })
    </script>
</html>
```

### Vue指令之`v-model`和`双向数据绑定`

* v-model实现数据的双向绑定 
* v-model只能用在表单元素中

```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
            <h4>{{msg}}</h4>
            <!-- v-bind只能实现数据的单向绑定 -->
            <input type="text" :value="msg" style="width:100%">
            <!-- v-model实现数据的双向绑定,效果类似于v-bind，但只能用于表单 -->
            <input type="text" v-model="msg" style="width:100%">
        </div>
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                msg:'闫少航http://shaohang.xin'
            },
            methods:{

            }
        })
    </script>
</html>
```

### 简易计算器案例

```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
            <input type="text" v-model="n1">
            <select v-model="opt">
                <option value="+">+</option>
                <option value="-">-</option>
                <option value="*">*</option>
                <option value="/">/</option>
            </select>
            <input type="text" v-model="n2">
            <input type="button" value="=" @click="calc">
            <input type="text" v-model="res">
        </div>

    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                n1:0,
                n2:0,
                res:0,
                opt:'+'
            },
            methods:{
                // 计算
                calc(){
                    // switch(this.opt){
                    //     case '+':
                    //         this.res = Number(this.n1) + Number(this.n2)
                    //         break
                    //     case '-':
                    //         this.res = Number(this.n1) - Number(this.n2)
                    //         break
                    //     case '*':
                    //         this.res = Number(this.n1) * Number(this.n2)
                    //         break
                    //     case '/':
                    //         this.res = Number(this.n1) / Number(this.n2)
                    //         break
                    // }
                    // 投机取巧一下（正是开发尽量少用）
                    this.res = eval('Number(this.n1)' + this.opt + 'Number(this.n2)' )
                }
            }
        })
    </script>
</html>
```

### 在Vue中使用样式

#### 使用class样式

1. 数组

   ```markup
   <h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>
   ```

2. 数组中使用三元表达式

   ```markup
   <h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
   ```

3. 数组中嵌套对象

   ```markup
   <h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
   ```

4. 直接使用对象

   ```markup
   <h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
   ```

#### 使用内联样式

1. 直接在元素上通过 `:style` 的形式，书写样式对象

   ```markup
   <h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
   ```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
   * 在data上定义样式：

     ```markup
     data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
     }
     ```

   * 在元素中，通过属性绑定的形式，将样式对象应用到元素中：

     ```markup
     <h1 :style="h1StyleObj">这是一个善良的H1</h1>
     ```
3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象
   * 在data上定义样式：

     ```markup
     data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
     }
     ```

   * 在元素中，通过属性绑定的形式，将样式对象应用到元素中：

     ```markup
     <h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
     ```

**Vue使用样式总汇：**

```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <style>
            .red{
                color:red;
            }
            .thin{
                font-weight: 200;
            }
            .italic{
                font-style: italic;
            }
            .active{
                letter-spacing: 0.5em;
            }
        </style>
    </head>
    <body>
        <!--class样式-->
        <div id='app'>
            <!-- 传统写法 -->
            <!-- vue为了动态样式，修改了传统写法 -->
            <h1 class="red thin">这是第一个h1标签</h1>
            <!-- 第一种使用方式，直接传递一个数组 -->
            <h1 :class="['red','italic']">这是第二个h1标签</h1>
            <!-- 在这里面可以使用三元表达式，不推荐 -->
            <h1 :class="['red','italic',flag?'active':'']">这是第三个h1标签</h1>
            <!-- 简化写法 在数组中使用对象(键值对)来代替三元表达式-->
            <h1 :class="['red','italic',{'active':flag}]">这是第四个h1标签</h1>
            <!-- 直接使用对象 -->
            <h1 :class="{red:true,italic:false,thin:true,active:flag}">这是第四个h1标签</h1>
            <h1 :class="class_obj">这是第四个h1标签</h1>
        </div>
        <!--内联样式style-->
        <div id="app1">
            <!--传统写法-->
            <h1 style="color:blue">这是内联标签</h1>
            <!--vue写法,key若带-，如font-weight，则key必须带引号-->
            <h1 :style="{color:'blue','font-weight':'200'}">这是内联标签2</h1>
            <h1 :style="styleObj1">这是内联标签2</h1>
            <h1 :style="[styleObj1,styleObj2]">这是内联标签2</h1>
        </div>
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                flag:true,
                class_obj:{red:true,italic:false,thin:true,active:this.flag},
            },
            methods:{

            }
        })
        let vm1 = new Vue({
            el:"#app1",
            data:{
                styleObj1:{color:'blue','font-weight':200},
                styleObj2:{'font-style':'italic'}
            }
        })
    </script>
</html>
```

### Vue指令之`v-for`和`key`属性

{% code title="迭代数组" %}
```markup
<ul>
  <li v-for="(item, i) in list"><!--i是索引-->
  索引:{{i}}---姓名:{{item.name}}---年龄:{{item.age}}</li>
</ul>

<!--list: [{name:"wlx",age:45},{name:"ete",age:78}]-->
<!--
索引:0---姓名:wlx---年龄:45
索引:1---姓名:ete---年龄:78
-->
```
{% endcode %}

{% code title="迭代对象中的属性" %}
```markup
<!-- 循环遍历对象身上的属性.i -->
<div v-for="(val, key, i) in userInfo">{{key}} --- {{val}} --- {{i}}</div>
<!--obj: {name:"wlx",age:45, weight:98}-->
<!--
name --- wlx --- 0
age --- 45 --- 1
weight --- 98 --- 2
-->
```
{% endcode %}

* 迭代数字

```text
<p v-for="i in 10">这是第 {{i}} 个P标签</p>
```

> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，key 现在是必须的。

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

{% code title="v-for和key使用汇总" %}
```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
            <!-- 循环简单数组 -->
            <p v-for="(item,index) in list">索引：{{index}} -- 值：{{item}}</p><hr>
            <p v-for="(item) in list">值：{{item}}可以只拿值</p><hr>
            <!-- 循环对象数组 -->
            <p v-for="user in list2">id:{{user.id}}--name:{{user.name}}</p><hr>
            <!-- 遍历对象 -->
            <p v-for="(val,key) in user">{{key}}:{{val}}</p><hr>
            <p v-for="(value) in user">{{value}}只拿值</p><hr>
            <!-- 迭代数字 -->
            <p v-for="count in 10">这是第{{count}}次循环</p><hr>

<!-- 在2.2.0+的版本里面，当在组件找那个使用v-for必须加key属性(用v-bind绑定) -->
<!--当Vue.js用V-for更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，-->
<!--Vue将不是移动DOM元素来匹配数据项的顺序，而是简单复用此处每个元素，并且确保它在特定素引下显示已被渲染过的每个元素。-->
<!--这种显示不会保证顺序，为了给Vue一个提示，使其跟踪每个节点的身份，重用和重新排序现有元素，需要为每项提供一个唯一key属性。-->
            <div>
                <lable>Id:
                    <input type="text" v-model="id">
                </lable>
                <lable>Name:
                    <input type="text" v-model="name">
                </lable>
                <input type="button" value="sum" @click="add">
            </div>
            <!--注意：在v-for循环的时候，key属性只能使用number或string-->
            <!--注意：key在使用的时候只能用v-bind属性形式进行绑定-->
            <p v-for="item in list2" :key="item.id" >
                <input type="checkbox">{{item.id}}----{{item.name}}
            </p>
        </div>
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',

            data:{
                id:"",
                name:"",
                list:[1,2,3,4,5,6],
                list2:[
                    {id:1,name:'zhangsan'},
                    {id:2,name:'lisi'},
                    {id:3,name:'wangwu'}
                ],
                user:{
                    id:1,
                    user:'shaohang',
                    age:20
                }
            },
            methods:{
                add(){
                    // this.list2.push({id:this.id,name:this.name})
                    this.list2.unshift({id:this.id,name:this.name})
                    //在列表最前面添加，不加key，选中在添加会产生错位
                }
            }
        })
    </script>
</html>
```
{% endcode %}

### Vue指令之`v-if`和`v-show`

通过`v-if`和`v-show`的属性值true和false来控制该元素是否显示

> 一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好。

```markup
<!DOCTYPE html>
<!--
    v-if的特点：每次都会重新删除或创建元素
    v-show的特点：每次不会进行DOM的删除和创建，只是切换元素display:none样式
    v-if有较高的切换性能消耗
    v-show有较高的初始渲染消耗
    如果元素涉及到频繁切换，最好不用v-if
    如果元素可能永远不会被显示出来则推荐使用v-if
 -->
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
            <input type="button" @click="flag=!flag" value="toggle">
            <h3 v-if="flag">这是用v-if控制的</h3>
            <h3 v-show="flag">这是用v-show控制的</h3>
        </div>
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                flag:true
            },
            methods:{

            }
        })
    </script>
</html>
```



## 方法，`computed`计算属性，watch侦听属性

### 引子

模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```markup
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

在这个地方，模板不再是简单的声明式逻辑。你必须看一段时间才能意识到，这里是想要显示变量 `message` 的翻转字符串。当你想要在模板中多次引用此处的翻转字符串时，就会更加难以处理。

### **使用函数（方法）：**

经过上面的学习，简化处理该问题能想到把翻转字符串封装到方法里，然后直接调用方法：

```javascript
<p>Reversed message: "{{ reversedMessage() }}"</p>
// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

还有一种新的方式能够更简易的实现：叫做计算属性

### **计算属性：**

```javascript
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})

结果：
Original message: "Hello"
Computed reversed message: "olleH"
```

这里我们声明了一个计算属性 `reversedMessage`。我们提供的函数将用作属性 `vm.reversedMessage` 的 getter 函数：

```javascript
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```

你可以打开浏览器的控制台，自行修改例子中的 vm。`vm.reversedMessage` 的值始终取决于 `vm.message` 的值。

**计算属性的setter**

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：

```javascript
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

现在再运行 `vm.fullName = 'John Doe'` 时，setter 会被调用，`vm.firstName` 和 `vm.lastName` 也会相应地被更新。

### 计算属性缓存 vs 方法：

我们可以通过在表达式中调用方法来达到同样的效果，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` **计算属性会立即返回之前的计算结果，而不必每次都执行函数。**

这也同样意味着下面的计算属性将不再更新，因为 `Date.now()` 不是响应式依赖：

```javascript
computed: {
  now: function () {
    return Date.now()
  }
}
```

相比之下，每当触发重新渲染时，调用方法将**总会**再次执行函数。

我们为什么需要缓存？假设我们有一个性能开销比较大的计算属性 **A**，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 **A** 。如果没有缓存，我们将不可避免的多次执行 **A** 的 getter！如果你不希望有缓存，请用方法来替代。

### watch侦听属性（能用计算属性尽量用计算属性）

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
        // 使用watch可以监视 data 中指定数据的变化，
        // 然后触发这个 watch 中对应的function处理函数
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
2. `methods`方法表示一个具体的操作，主要书写业务逻辑（多用）；
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作； 虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。


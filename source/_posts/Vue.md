---
title: Vue
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-19 18:55:40
tags: ['js', 'vue']
password:
summary:
categories:
---

# Vue

## 概述

### 什么是Vue.js

+ Vue.js 是目前最火的一个前端框架，React是最流行的一个前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于Weex）

+ Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！

+ Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）

+ 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；

### 为什么要学习流行框架 - 提高开发的效率
 + 提高开发效率的发展历程：原生JS -> Jquery之类的类库 -> 前端模板引擎 -> Angular.js / Vue.js（能够帮助我们减少不必要的DOM操作；提高渲染效率；双向数据绑定的概念【通过框架提供的指令，我们前端程序员只需要关心数据的业务逻辑，不再关心DOM是如何渲染的了】）
 + 在Vue中，一个核心的概念，就是让用户不再操作DOM元素，解放了用户的双手，让程序员可以更多的时间去关注业务逻辑；

### 框架和库的区别

 + 框架：
    + 是一套完整的解决方案；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。在框架这个环境里运行代码
    + node 中的 express；
 + 库（插件）：提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。引用库作为工具。
    + DOM操作：从Jquery 切换到 Zepto
    + 模块化：从 EJS 切换到 art-template

### Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

 + MVC 是后端的分层开发概念；
 + MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：[MVVM](https://www.cnblogs.com/onepixel/p/6034307.html)把前端的视图层，分为了 三部分 Model, View , VM(ViewModel)
 + 为什么有了MVC还要有MVVM
 + Vue.js 基本代码 和 MVVM 之间的对应关系

### 基本的代码结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/vue-2.4.0.js"></script><!-- 1. 导入Vue的包 -->
</head>
<body>
  <div id="app"><!-- 1.容器 将来 new 的Vue实例，会控制这个 元素中的所有内容 Vue 实例所控制的这个元素区域，就是我们的 V -->
    <p>{{ msg }}</p>
  </div>
  <script>
    // 2. 创建一个Vue的实例 new Vue()；并配置options选项对象
    // 当我们导入包之后，在浏览器的内存中，就多了一个 Vue 构造函数
    //  注意：我们 new 出来的这个 vm 对象，就是我们 MVVM中的 VM调度者
    var vm = new Vue({
      el: '#app',  // 表示，当前我们 new 的这个 Vue 实例，要控制页面上的哪个区域;两种写法：'jq选择器' 或 element元素对象
      template:``,
      data: { // 这里的 data 就是 MVVM中的 Model，专门用来保存 每个页面的数据的data。属性中，存放的是 el 中要用到的数据
        msg: '欢迎学习Vue',
      },
      methods:{
          func1:function(e){
              console.log(this);//this指当前new Vue实例
              console.log(e)
          }
      }
    })
  </script>
</body>
</html>
```



## 指令Directives

值都是必须""，

> ### 属性值选项：
>
> "number + 1"
>
> "ok ? 'YES' : 'NO'"
>
> " message.split('').reverse().join('') "
>
> ### 在Vue中使用样式
>
> - 使用class样式
>   1. 数组 `<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>`
>   2. 数组中使用三元表达式`<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>`
>   3. 数组中嵌套对象`<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>`
>   4. 直接使用对象`<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>`
> - 使用内联样式
>   1. 直接在元素上通过 `:style` 的形式，书写样式对象`<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>`
>   2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
>      - 在data上定义样式：`data: { h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }}`
>      - 在元素中，通过属性绑定的形式，将样式对象应用到元素中：`<h1 :style="h1StyleObj">这是一个善良的H1</h1>`
>   3. 在`:style`中通过数组，引用多个 `data` 上的样式对象
>      - 在data上定义样式：`data: {h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },h1StyleObj2: { fontStyle: 'italic' }}`
>      - 在元素中，通过属性绑定的形式，将样式对象应用到元素中：`<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>`

### `v-text`/`v-html` 

> **插值** 和{{xx}}的区别是：
>
> ```html
> <p>{{msg}}哈哈哈是的</p> 	//可以更新元素的部分 textContent
> <p v-text="msg">无效文字</p> //更新元素的全部 textContent
> //text和html的区别 同 inner-text和inner-html的区别
> ```

### `v-bind:attr=""` `:attr`

>**单向数据传递属性值。缩写`:` 绑定atrr style class等 属性，三种用法：**
>
>  - 直接使用指令`v-bind`
>  - 使用简化指令`:`
>  - 在绑定的时候，拼接绑定内容：`:title="btnTitle + ', 这是追加的内容'"`
>
>  ```html
>  <div v-bind:argument="expression"></div> //绑定属性格式
>  <!-- 绑定一个属性 -->
>  <img v-bind:src="imageSrc">
>  <!-- 缩写 -->
>  <img :src="imageSrc">
>  <!-- 内联字符串拼接 -->
>  <img :src="'/path/to/images/' + fileName">
>  <!-- ID 绑定 -->
>  <div v-bind:id="'list-' + id"></div>
>  <!-- class 绑定 -->
>  <div :class="{ red: isRed }"></div> <!--绑定的对象 自动toString() -->
>  <div :class="[classA, classB]"></div>
>  <div :class="[classA, { classB: isB, classC: isC }]">
>  <div :class="xx === 2?'cla1':'"></div>
>  <!-- style 绑定 -->
>  <div :style="{ fontSize: size + 'px' }"></div>
>  <div :style="[styleObjectA, styleObjectB]"></div>
>  <!-- 绑定一个有属性的对象 -->
>  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>
>  <!-- 通过 prop 修饰符绑定 DOM 属性 -->
>  <div v-bind:text-content.prop="text"></div>
>  <!-- prop 绑定。“prop”必须在 my-component 中声明。-->
>  <my-component :prop="someThing"></my-component>
>  <!-- 通过 $props 将父组件的 props 一起传给子组件 -->
>  <child-component v-bind="$props"></child-component>
>  <!-- XLink -->
>  <svg><a :xlink:special="foo"></a></svg>
>  ```

### `v-model=""` 

> 双向数据绑定 value属性
>
> ```html
> <p>{{msg0}}</p>
> <input type="text" v-model = 'msg0'>
> ```

### `v-show`

>  条件渲染 
>
>  和 `v-if`区别 是： `v-if`的实现原理是html的`<!---->`；而`if-show`的实现原理是css的`opacity:0` 。 前者的频繁性切换性能消耗较多 后者的初始性能消耗较多

###  `v-if` `v-else-if` `v-else`

> 条件渲染
>
> ```html
> <p v-if="height > 180">{{height}}</p>
> <p v-else-if=''>{{height}}</p>
> <p v-else>{{height}}</p>
> <!--三个必须相邻兄弟元素-->
> ```
>
> - `v-if`
>
>   ```html
>   <p v-if="show">显示</p>
>   <p v-if="hide">隐藏</p>
>   <p v-if="height > 180">{{height}}</p>
>   
>   let vm = new Vue({
>         el : '.container',
>         data : {
>             show:true,
>             hide:false,
>             height:150,
>             number:0,
>         }
>   )
>   ```
>
> - `v-else-if` : *前一兄弟元素必须有 `v-if` 或 `v-else-if`*
>
> - `v-else` : *前一兄弟元素必须有 `v-if` 或 `v-else-if`*
>


### `v-for`

> 列表渲染：遍历 数据 可接受数据类型 Array | Object | number | string 
>
> ```html
> <div v-for="(value[, index]) in items"></div>
> <div v-for="(value[, key]) in object"></div>
> <div v-for="str in stringX"></div>
> <div v-for="(value[, key, index]) in object"></div>
> <script>
>    let vm = new Vue({
>        el : '.container',
>        data : {
>            scores:[1,3,50,62],
>            dog:{name:'ss',age:12},
>            str1:'sddd大师傅但是',
>            num1:222156465,
>        }
>    })
> </script>
> 
> <table>
>        <thead>
>            <tr>
>                <th>姓名</th>
>                <th>年龄</th>
>                <th>性别</th>
>            </tr></thead>
>        <tbody>
>            <tr v-for="item in students">
>                <td>{{item.name}}</td>
>                <td>{{item.age}}</td>
>                <td>{{item.gentle}}</td>
>            </tr>
>        </tbody>
> </table>
> <script>
>    let vue = new Vue({
>         el : '.container',
>        data : {
>            students:[
>                {name:'n1',age:11,gentle:'m'},
>                {name:'n2',age:22,gentle:'f'},
>                {name:'n3',age:33,gentle:'m'},
>                {name:'n4',age:44,gentle:'m'}
>            ]
>        }
>    })
> </script>
> ```
>

### `v-on:event` `@event`

>事件处理器,绑定 事件
>
>event.修饰符
>
>- `.stop` - 调用 `event.stopPropagation()`。阻止冒泡
>- `.prevent` - 调用 `event.preventDefault()`。阻止默认事件
>- `.capture` - 添加事件侦听器时使用 capture 模式（事件捕获模式）。
>- `.self` - 只当事件是从侦听器绑定的元素本身触发时（比如不是子元素）才触发回调。
>- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
>- `.native` - 监听组件根元素的原生事件。
>- `.once` - 只触发一次回调。
>- `.left` - (2.2.0) 只当点击鼠标左键时触发。
>- `.right` - (2.2.0) 只当点击鼠标右键时触发。
>- `.middle` - (2.2.0) 只当点击鼠标中键时触发。
>- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器
>
>```html
><!-- 方法处理器 -->
><button v-on:click="doThis"></button>
><!-- 内联语句 -->
><button v-on:click="doThat('hello', $event)"></button>
><!-- 缩写 -->
><button @click="doThis"></button>
><!-- 停止冒泡 -->
><button @click.stop="doThis"></button>
><!-- 阻止默认行为 -->
><button @click.prevent="doThis"></button>
><!-- 阻止默认行为，没有表达式 -->
><form @submit.prevent></form>
><!--  串联修饰符 -->
><button @click.stop.prevent="doThis"></button>
><!-- 键修饰符，键别名 -->
><input @keyup.enter="onEnter">
><!-- 键修饰符，键代码 -->
><input @keyup.13="onEnter">
><!-- 点击回调只会触发一次 -->
><button v-on:click.once="doThis"></button>
><!-- 对象语法 (2.4.0+) -->
><button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
><script>
>let vm = new Vue({
>   el : '.container',
>   data : {
>       students:[
>           {name:'n1',age:11,gentle:'m'},
>           {name:'n2',age:22,gentle:'f'},
>           {name:'n3',age:33,gentle:'m'},
>           {name:'n4',age:44,gentle:'m'}
>       ],
>   },
>   methods:{
>      doThat(){
>           //console.log('sss');
>      }
>   },
>});
></script>
>```

### `v-once` 

> 只能响应 双向绑定修改数据一次

### `v-pre`

### `v-cloak`

## 组件

### 组件定义和使用
什么是组件： 组件的出现，就是为了拆分Vue实例的代码量的，能够让我们以不同的组件，来划分不同的功能模块，将来我们需要什么样的功能，就可以去调用对应的组件即可；
组件化和模块化的不同：

 + 模块化： 是从代码逻辑的角度进行划分的；方便代码分层开发，保证每个功能模块的职能单一；
 + 组件化： 是从UI界面的角度进行划分的；前端的组件化，方便UI组件的重用；

#### 全局组件 `Vue.component('tabName', {});`

> *注意： 组件中的DOM结构，有且只能有唯一的根元素（Root Element）来进行包裹！*
>
> 1. 使用 `Vue.extend`定义 和 `Vue.component`声明：
>
> ```js
> var login = Vue.extend({
> 	template: '<h1>登录</h1>'
> });
> Vue.component('login', login);
> 
> //或 直接在声明时定义
> Vue.component('register', {
> 	template: '<h1>注册</h1>'
> });
> 
> //语法糖
> let Component1 = {
>      template:`aaaa`,//或 jq选择器'#id' 或 DOM element 对象
>      data() {//在组件中，`data`需要被定义为一个方法
>          return {
>            msg: '大家好！'
>          }
>      },
> }
> Vue.component('component1',Component1)
> ```
>
> 2. 将模板字符串，定义到script标签种：
>
> ```html
> <script id="tmpl" type="x-template">
>    <div><a href="#">登录</a> | <a href="#">注册</a></div>
> </script>
> <script type="text/javascript">
>  Vue.component('account', {
>        template: '#tmpl'
>  });//需要使用 Vue.component 来定义组件：
> </script>
> <!--在子组件中，如果将模板字符串，定义到了script标签中，那么，要访问子组件身上的`data`属性中的值，需要使用`this`来访问-->
> ```


#### 局部组件 `components:{tabName:{}}`

> ```html
> <!--引用组件：-->
> <div id="app">
>  <account></account>
> </div>
> <!--定义组件：使用 components 属性定义-->
> <script>
>  // 创建 Vue 实例，得到 ViewModel
>  var vm = new Vue({
>    el: '#app',
>    data: {},
>    methods: {},
>    components: { // 定义子组件
>      account: { // account 组件
>        template: '<div><h1>这是Account组件{{name}}</h1><login></login></div>', // 在这里使用定义的子组件
>        components: { // 定义子组件的子组件
>          login: { // login 组件
>            template: "<h3>这是登录组件</h3>"
>          }
>        }
>      }
>    }
>  });
> </script>
> ```

### 父子组件间通信 数据传递 

#### 父组件向子组件传值

> 通过父组件中调用的`子标签的属性` 和 子组件`props`属性 传递
>
> ```html
> <div id="app">
>     <son :finfo="msg"></son>
>     <!--v-bind:或简化指令-->
> </div>
> <script>
>     // 创建 Vue 实例，得到 ViewModel
>     var vm = new Vue({
>       el: '#app',
>       data: {
>         msg: '这是父组件中的消息'
>       },
>       components: {
>         son: {
>           template: '<h1>这是子组件 --- {{finfo}}</h1>',
>           props: ['finfo']
>         }
>       }
>     });
> </script>
> ```

#### 子组件向父组件传值 `<son @customName="methodsFunc"></son>` 

> 原理：父组件将方法的引用，传递到子组件内部，子组件在内部调用父组件传递过来的方法，同时把要发送给父组件的数据，在调用方法的时候当作参数传递进去
>
> ```html
> <div id="app">
>     <!-- 引用父组件 父组件将方法的引用传递给子组件 -->
>     <!--其中，`getMsg`是父组件中`methods`中定义的方法名称，`func`是子组件调用传递过来方法时候的方法名称-->
>     <son @func="getMsg"></son>
> </div>
> <script type="x-template" id="son">
>       <div>
>         <input type="button" value="向父组件传值" @click="sendMsg" />
>       </div>
> </script>
> <script>
>     // 子组件的定义方式
>     Vue.component('son', {
>       template: '#son', // 组件模板Id
>       methods: {
>         sendMsg() {// 按钮的点击事件
>           this.$emit('func', 'OK'); // 调用父组件传递过来的方法，同时把数据传递出去 子组件内部通过`this.$emit('方法名', 要传递的数据)`方式，来调用父组件中的方法，同时把数据传递给父组件使用
>         }
>       }
>     });
>     // 创建 Vue 实例，得到 ViewModel
>     var vm = new Vue({
>       el: '#app',
>       data: {},
>       methods: {
>         getMsg(val){ // 子组件中，通过 this.$emit() 实际调用的方法，在此进行定义
>           alert(val);
>         }
>       }
>     });
> </script>
> ```

## slot 插槽

其实是 父组件 传递的DOM结构，留给子组件内部自定义

### 具名插槽

>   `template#temID>slot[name]`和`temID>*[slot]`对应
>
> ```html
> <!--存在于一个组件中-->
> <div class="container">
>     <mySlot>
>         <!-- slot="xx"和 name="xx"一一对应 -->
>         <img slot="slot1" src="" alt="">
>         <p slot='slot2'>士大夫</p>
>     </mySlot>
> </div>
> <template id="mySlot">
>     <div>
>         <h2>模板的头部</h2>
>         <slot name='slot1'>预留的一个插槽</slot>
>         <slot name='slot2'>预留的一个插槽</slot>
>         <slot name='slot3'>预留的一个插槽</slot>
>         <footer>模板尾部 </footer>
>     </div>
> </template>
> ```

### 匿名插槽

> ```html
> <!--存在于一个组件中-->
> <div class="container">
>     <mySlot>
>         <!--里面放任何数量的任何标签代替 template内的slot标签-->
>         <img src="" alt="">
>         <p></p>
>     </mySlot>
> </div>
> <template id="mySlot">
>     <div>
>         <h2>模板的头部</h2>
>         <slot>预留的一个插槽</slot>
>         <footer>模板尾部</footer>
>     </div>
> </template>
> ```

## 过滤器 `filters:{}` `Vue.filter('var',filterFun)`

### 概念

Vue.js 允许你自定义过滤器，**可被用作一些常见的文本格式化**。过滤器可以用在两个地方：**mustache 插值和 v-bind 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符指示；

1. 使用在HTML
2. 定义过滤器 *注意：当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：局部过滤器优先于全局过滤器被调用！*
### 全局过滤器

> ```js
> // 使用在HTML：`<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>` *dataFormat()参数是arguments[ , ..args]*
> // 定义一个全局过滤器
> Vue.filter('dataFormat', function (input, pattern = '') {//第一个参数arguments[0]是html种|左侧的数据
> var dt = new Date(input);
> // 获取年月日
> var y = dt.getFullYear();
> var m = (dt.getMonth() + 1).toString().padStart(2, '0');
> var d = dt.getDate().toString().padStart(2, '0');
> // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
> // 否则，就返回  年-月-日 时：分：秒
> if (pattern.toLowerCase() === 'yyyy-mm-dd') {
>  return `${y}-${m}-${d}`;
> } else {
>  // 获取时分秒
>  var hh = dt.getHours().toString().padStart(2, '0');
>  var mm = dt.getMinutes().toString().padStart(2, '0');
>  var ss = dt.getSeconds().toString().padStart(2, '0');
>  return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
> }
> });
> ```

### 私有过滤器 

> 在`filters`中赋值：
>
> ```js
> filters: { // 私有局部过滤器，只能在 当前 VM 对象所控制的 View 区域进行使用
>     dataFormat(input, pattern = "") { // 在参数列表中 通过 pattern="" 来指定形参默认值，防止报错
>       var dt = new Date(input);
>       // 获取年月日
>       var y = dt.getFullYear();
>       var m = (dt.getMonth() + 1).toString().padStart(2, '0');
>       var d = dt.getDate().toString().padStart(2, '0');
>       // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
>       // 否则，就返回  年-月-日 时：分：秒
>       if (pattern.toLowerCase() === 'yyyy-mm-dd') {
>         return `${y}-${m}-${d}`;
>       } else {
>         // 获取时分秒
>         var hh = dt.getHours().toString().padStart(2, '0');
>         var mm = dt.getMinutes().toString().padStart(2, '0');
>         var ss = dt.getSeconds().toString().padStart(2, '0');
>         return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
>       }
>     }
>   }
> //使用ES6中的字符串新方法 String.prototype.padStart(maxLength, fillString='') 或 String.prototype.padEnd(maxLength, fillString='')来填充字符串；
> ```

## 监视 `watch` `computed`

### `watch`属性的使用，监视单个data 或 路由的变化

考虑一个问题：想要实现 `名` 和 `姓` 两个文本框的内容改变，则全名的文本框中的值也跟着改变；（用以前的知识如何实现？？？）

1. 监听`data`中属性的改变，反馈函数：

   ```html
   <div id="app">
       <input type="text" v-model="firstName"> +
       <input type="text" v-model="lastName"> =
       <span>{{fullName}}</span>
     </div>
   
     <script>
       // 创建 Vue 实例，得到 ViewModel
       var vm = new Vue({
         el: '#app',
         data: {
           firstName: 'jack',
           lastName: 'chen',
           fullName: 'jack - chen',
           obj : ['sss'],//不可监视，因为监视的是对象的地址，地址没改；需要深度监视
         },
         methods: {},
         watch: {//key是data属性的属性名
           'firstName': function (newVal, oldVal) { // 第一个参数是新数据，第二个参数是旧数据
             this.fullName = newVal + ' - ' + this.lastName;
           },
           'lastName': function (newVal, oldVal) {
             this.fullName = this.firstName + ' - ' + newVal;
           },
           obj:{//深度监视 object || array
              deep:true,
              handler:function(newV,oldV){
                  console.log('监视成功')
              }
           }
         }
       });
     </script>
   ```

2. 监听路由对象的改变：

   ```html
   <div id="app">
       <router-link to="/login">登录</router-link>
       <router-link to="/register">注册</router-link>
       <router-view></router-view>
   </div>
   
   <script>
       var login = Vue.extend({
         template: '<h1>登录组件</h1>'
       });
       var register = Vue.extend({
         template: '<h1>注册组件</h1>'
       });
       var router = new VueRouter({
         routes: [
           { path: "/login", component: login },
           { path: "/register", component: register }
         ]
       });
   
       // 创建 Vue 实例，得到 ViewModel
       var vm = new Vue({
         el: '#app',
         data: {},
         methods: {},
         router: router,
         watch: {
           '$route': function (newVal, oldVal) {
             if (newVal.path === '/login') {
               console.log('这是登录组件');
             }
           }
         }
       });
   </script>
   ```


### `computed`计算属性的使用，同时监听多个data的改变
1. 默认只有`getter`的计算属性：

   ```html
   <div id="app">
       <input type="text" v-model="firstName"> +
       <input type="text" v-model="lastName"> =
       <span>{{fullName}}</span>
   </div>
   <script>
       // 创建 Vue 实例，得到 ViewModel
       var vm = new Vue({
         el: '#app',
         data: {
           firstName: 'jack',
           lastName: 'chen'
         },
         methods: {},
         computed: { // 计算属性； 特点：当计算属性中索引来的任何一个 data 属性改变之后，都会重新触发 本计算属性 的重新计算，从而更新 fullName 的值
           fullName() {//或 命名成 fullName:funciont(){return }
             return this.firstName + ' - ' + this.lastName;
           }
         }
       });
   </script>
   ```

2. 定义有`getter`和`setter`的计算属性：

   ```html
   <div id="app">
       <input type="text" v-model="firstName">
       <input type="text" v-model="lastName">
       <!-- 点击按钮重新为 计算属性 fullName 赋值 -->
       <input type="button" value="修改fullName" @click="changeName">
       <span>{{fullName}}</span>
   </div>
   <script>
       // 创建 Vue 实例，得到 ViewModel
       var vm = new Vue({
         el: '#app',
         data: {
           firstName: 'jack',
           lastName: 'chen'
         },
         methods: {
           changeName() {
             this.fullName = 'TOM - chen2';
           }
         },
         computed: {
           fullName: {
             get: function () {
               return this.firstName + ' - ' + this.lastName;
             },
             set: function (newVal) {
               var parts = newVal.split(' - ');
               this.firstName = parts[0];
               this.lastName = parts[1];
             }
           }
         }
       });
   </script>
   ```


### `watch`、`computed`和`methods`之间的对比
1. `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
2. `methods`方法表示一个具体的操作，主要书写业务逻辑；
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；

## 生命周期事件函数（钩子 ）

从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为[生命周期](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)！[声明周期图示](https://cn.vuejs.org/images/lifecycle.png)

| 钩子                                                        | 解释                                                         |
| ----------------------------------------------------------- | :----------------------------------------------------------- |
| [beforeCreate](https://cn.vuejs.org/v2/api/#beforeCreate)   | **实例刚刚在内存中被创建**，组件属性计算之前 \| 数据观测 (data observer) 和 event/watcher 事件配置之前被调用。还没有初始化好 data 和 methods 属性 |
| [created](https://cn.vuejs.org/v2/api/#created)             | **实例已经在内存中创建完成**。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。此时 data 和 methods 已经创建完成，然而挂载阶段还没开始，没开始编译模板，DOM还没生成。`$el` 属性还不存在 |
| [beforeMount](https://cn.vuejs.org/v2/api/#beforeMount)     | **模板编译/挂载之前**：相关的 `render` 函数首次被调用 。此时已经完成了模板的编译，但是还没有挂载到页面中 |
| [mounted](https://cn.vuejs.org/v2/api/#mounted)             | **模板编译/挂载之后**：`el` 被新创建的 `vm.$el` 替换，并挂载到实例上去之后调用该钩子。<br/>如果 root 实例挂载了一个文档内元素，当 `mounted` 被调用时 `vm.$el` 也在文档内 .已经将编译好的模板，挂载到了页面指定的容器中显示 |
| [beforeUpdate](https://cn.vuejs.org/v2/api/#beforeUpdate)   | **组件更新前**：数据更新时，发生在虚拟 DOM 打补丁之前。此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器 |
| [updated](https://cn.vuejs.org/v2/api/#updated)             | **组件更新之后**：由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了 |
| [activated](https://cn.vuejs.org/v2/api/#activated)         | keep-alive **组件激活时调用**。*keep-alive 是vue内部组件只与v-if搭配使用，v-if的原理不是display的控制，是DOM的remove和append* |
| [deactivated](https://cn.vuejs.org/v2/api/#deactivated)     | keep-alive **组件移除时调用**                                |
| [beforeDestroy](https://cn.vuejs.org/v2/api/#beforeDestroy) | **Vue实例销毁之前**。在这一步，实例仍然完全可用。            |
| [destroyed](https://cn.vuejs.org/v2/api/#destroyed)         | **Vue 实例销毁后**。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 |
| [errorCaptured](https://cn.vuejs.org/v2/api/#errorCaptured) | 当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 `false` 以阻止该错误继续向上传播。 |

简略总结：

> `beforecreated`：el 和 data 并未初始化   *举个栗子：可以在这加个loading事件*
>
> `created`:完成了 data 数据的初始化，el没有 *在这结束loading，还做一些初始化，实现函数自执行*
>
> `beforeMount`：完成了 el 和 data 初始化  
>
> `mounted` ：完成挂载 *在这发起后端请求，拿回数据，配合路由钩子做一些事情*
>
> `beforeDestroy`： *你确认删除XX吗？ destroyed ：当前组件已被删除，清空相关内容*
>
> **示例**
>
> ```js
> var app = new Vue({
>    el: '#app',
>    template:`
> <div>
> 	<keep-alive><span v-if="isExit">出现</span></keep-alive>
>     <button type="button" @click="isExit = !isExit">点击</button>
> </div>
> `,
>    data: {
>     message : "xuxiao is boy",
>     isExit:false
>    },
>     //创建期间 4个
>     beforeCreate: function () {
>              console.group('beforeCreate 创建前状态===============》');
>             console.log("%c%s", "color:red" , "el     : " + this.$el); //undefined
>             console.log("%c%s", "color:red","data   : " + this.$data); //undefined 
>             console.log("%c%s", "color:red","message: " + this.message)  
>      },
>      created: function () {
>          console.group('created 创建完毕状态===============》');
>          console.log("%c%s", "color:red","el     : " + this.$el); //undefined
>             console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化 
>             console.log("%c%s", "color:red","message: " + this.message); //已被初始化
>      },
>      beforeMount: function () {
>          console.group('beforeMount 挂载前状态===============》');
>          console.log("%c%s", "color:red","el     : " + (this.$el)); //已被初始化
>          console.log(this.$el);
>             console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化  
>             console.log("%c%s", "color:red","message: " + this.message); //已被初始化  
>      },
>      mounted: function () {
>          console.group('mounted 挂载结束状态===============》');
>          console.log("%c%s", "color:red","el     : " + this.$el); //已被初始化
>          console.log(this.$el);    
>             console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
>             console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
>      },
>     //运行期间 2个
>      beforeUpdate: function () {
>          console.group('beforeUpdate 更新前状态===============》');
>          console.log("%c%s", "color:red","el     : " + this.$el);
>          console.log(this.$el);   
>             console.log("%c%s", "color:red","data   : " + this.$data); 
>             console.log("%c%s", "color:red","message: " + this.message); 
>      },
>      updated: function () {
>          console.group('updated 更新完成状态===============》');
>          console.log("%c%s", "color:red","el     : " + this.$el);
>          console.log(this.$el); 
>             console.log("%c%s", "color:red","data   : " + this.$data); 
>             console.log("%c%s", "color:red","message: " + this.message); 
>      },
>     //销毁期间 2个 对应父组件v-if = false时的销毁当前组件
>      beforeDestroy: function () {
>          console.group('beforeDestroy 销毁前状态===============》');
>          console.log("%c%s", "color:red","el     : " + this.$el);
>          console.log(this.$el);    
>             console.log("%c%s", "color:red","data   : " + this.$data); 
>             console.log("%c%s", "color:red","message: " + this.message); 
>      },
>      destroyed: function () {
>          console.group('destroyed 销毁完成状态===============》');
>          console.log("%c%s", "color:red","el     : " + this.$el);
>          console.log(this.$el);  
>             console.log("%c%s", "color:red","data   : " + this.$data); 
>             console.log("%c%s", "color:red","message: " + this.message)
>      },
>     //keep-alive包裹的组件，在v-if时有缓存
>     activated: function () {//激活时
>     
> 	},
>     deactivated: function () {//未激活时
>         
>     }
>  })
> ```

## DOM操作

- $属性：$refs 获取组件内的元素，根据元素的ref属性 `this.$refs.ele1.focus()`
- $parent：获取当前组件对象的父组件
- $children：获取子组件
- $root：获取new Vue的实例对象 vm
- $el：组件对象的DOM元素












# Vue-router

## 概述

### [路由概述](https://router.vuejs.org/zh/api/)

1. 对于普通的网站，所有的超链接都是URL地址，所有的URL地址都对应服务器上对应的资源；

2. 对于单页面应用程序来说，主要通过URL中的`location.hash`(#号)来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，单页面程序中的页面跳转主要用hash实现；

3. 在单页面应用程序中，这种通过hash改变来切换页面的方式，称作前端路由（区别于后端路由）；

### 使用 vue-router
1. 导入 vue-router 组件类库：`<script src="./lib/vue-router-2.7.0.js"></script><!--如果是模块化调用，需要Vue.use(VueRouter)-->`

2. 使用 router-link 组件来导航

   ```html
   <router-link to="/login">登录</router-link>
   <router-link to="/register">注册</router-link>
   ```

3. 使用 router-view 组件来显示匹配到的组件

   ```html
   <router-view></router-view>
   ```

4. 创建使用`Vue.extend`创建组件

   ```js
   //创建登录组件
   var login = Vue.extend({
         template: '<h1>登录组件</h1>'
   });
   //创建注册组件
   var register = Vue.extend({
         template: '<h1>注册组件</h1>'
   });
   ```

5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则

   ```js
   var router = new VueRouter({
         routes: [
           { path: '/login', component: login },
           { path: '/register', component: register }
         ]
   });
   var register = Vue.extend({
         template: '<h1>注册组件 --- {{this.$route.params.id}}</h1>'
   });//通过 `this.$route.params`来获取路由中的参数
   ```

6. 创建 Vue 实例，得到 ViewModel，使用 router 属性来使用路由规则

   ```js
   var vm = new Vue({
       el: '#app',
       router: router // 使用 router 属性来使用路由规则
   });
   ```

### 路由嵌套*使用 `children` 属性实现*
```html
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
        mode:'history',//默认是hash模式
      routes: [
        { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向
        {
          path: '/account',
          component: account,
          children: [ // 通过 children 数组属性，来实现路由的嵌套
            { path: 'login', component: login }, // 注意，子路由的开头位置，不要加 / 路径符，单个路由嵌套用 component不加s
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

## 使用history模式 

实现 SPA(single-page application)

vue-router默认使用hash模式，即#xxx

# Vuex

## Vuex 状态管理

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**

### 步骤

- 引入Vuex
- 安装插件`vue
- 创建store
- 配置store中的数据对象
- 将sotre对象关联vue实例

### API

- 构造器选项

  1. `state`*Object | Function*

     > 如果你传入返回一个对象的函数，其返回的对象会被用作根 state。这在你想要重用 state 对象，尤其是对于重用 module 来说非常有用

  2. `getters`*{ [type: string]: Function(state,[getter]) }*

     > - getter接受 state 作为其第一个参数
     >
     >   getter也可以接受其他 getter 作为第二个参数
     >
     > - 在 store 上注册 getter，getter 方法接受以下参数：
     >
     >   ```text
     >   state,     // 如果在模块中定义则为模块的局部状态
     >   getters,   // 等同于 store.getters
     >   ```
     >
     >   当定义在一个模块module里时会特别一些：
     >
     >   ```text
     >   state,       // 如果在模块中定义则为模块的局部状态
     >   getters,     // 等同于 store.getters
     >   rootState    // 等同于 store.state
     >   rootGetters  // 所有 getters
     >   ```
     >
     >   注册的 getter 暴露为 `store.getters`。

  3. `mutations`*{ [type: string]: Function(state,[payLoad]) }*

     > 处理函数总是接受 `state` 作为第一个参数（如果定义在模块中，则为模块的局部状态），`payload` 作为第二个参数（可选）。

  4. `actions`*{ [type: string]: Function(context,[payLoad]) }*

     > 处理函数总是接受 `context` 作为第一个参数，`payload` 作为第二个参数（可选）。
     >
     > `context` 对象包含以下属性:
     >
     > ```js
     > {
     >   state,      // 等同于 `store.state`，若在模块中则为局部状态
     >   rootState,  // 等同于 `store.state`，只存在于模块中
     >   commit,     // 等同于 `store.commit`
     >   dispatch,   // 等同于 `store.dispatch`
     >   getters,    // 等同于 `store.getters`
     >   rootGetters // 等同于 `store.getters`，只存在于模块中
     > }
     > ```

  5. `modules`*Object*

     > 每个模块拥有自己的 `state`、`mutations`、`actions`、`getters`，甚至是嵌套子模块——从上至下进行同样方式的分割
     >
     > - 局部
     >
     >   - 对于模块内部的 mutation 和 getter，接收的第一个参数是模块的**局部状态对象state**。
     >   - 对于模块内部的 action，局部状态通过 `context.state` 暴露出来
     >
     > - 在带有命名空间`namespaced:true`的模块下 访问全局时
     >
     >   - 对于模块内部的 action，根节点状态为 `context.rootState`
     >   - 对于模块内部的 getter，`rootState` 和 `rootGetter` 会作为第三和第四参数传入
     >
     >   若需要在全局命名空间内分发 action 或提交 mutation，将 `{ root: true }` 作为第三参数传给 `dispatch` 或 `commit` 即可。
     >
     >   若需要在带命名空间的模块注册全局 action，你可在此action属性中添加 `root: true`，并将这个 action 的定义放在函数 `handler` 属性中

  6. `plugins` *Array |Function*

     > 一个数组，包含应用在 store 上的插件方法。这些插件就是一个函数，接收 store 作为唯一参数，可以监听 mutation（用于外部地数据持久化、记录或调试）或者提交 mutation （用于内部数据，例如 websocket 或 某些观察者）
     >
     > ```js
     > const myPlugin = store => {
     >   // 当 store 初始化后调用
     >   store.subscribe((mutation, state) => {
     >     // 监听 mutation，每次 mutation 之后调用
     >     // mutation 的格式为 { type, payload }
     >   })
     > }
     > const createWebSocketPlugin = function(socket) {
     >   return store => {
     >     socket.on('data', data => {
     >       store.commit('receiveData', data)
     >     })
     >     store.subscribe(mutation => {
     >       if (mutation.type === 'UPDATE_DATA') {
     >         socket.emit('update', mutation.payload)
     >       }
     >     })
     >   }
     > }//同步 websocket 数据源到 store
     > 
     > const store = new Vuex.Store({
     >   // ...
     >   plugins: [myPlugin,createWebSocketPlugin(socket)]
     > })
     > ```

  7. `strict`*Boolean*

     > 默认false，使 Vuex store 进入严格模式，在严格模式下，任何 mutation 处理函数以外修改 Vuex state 都会抛出错误

  8. `devtools`*Boolean*

     > 为某个特定的 Vuex 实例打开或关闭 devtools。对于传入 `false` 的实例来说 Vuex store 不会订阅到 devtools 插件。可用于一个页面中有多个 store 的情况。

- 实例

  - 实例属性(只读)
    - state
    - getters
  - 实例方法
    - `commit` 提交 mutation	`commit(type: string|mutation name, payload?: any, options?: Object)`
    - `dispatch` 分发 action	`dispatch(type: string|action name, payload?: any, options?: Object)`
    - `replaceState` 替换 store 的根状态	`replaceState(state: Object)`
    - `watch` 响应式地侦听 `fn` 的返回值改变时调用回调函数	`watch(fn: Function, callback: Function, options?: Object):  Function` [例子](https://codepen.io/CodinCat/pen/PpNvYr?editors=1111)
    - `subscribe` 订阅 store 的 mutation	`subscribe(handler: Function): Function`
    - `subscribeAction` 订阅 store 的 action	`subscribeAction(handler: Function): Function`
    - `registerModule` 注册一个动态模块	`registerModule(path: string | Array<string>, module: Module, options?: Object)`
    - `unregisterModule` 卸载一个动态模块	`unregisterModule(path: string | Array<string>)`
    - `hotUpdate` 热替换新的 actions ，mutations和modules	`hotUpdate(newOptions: Object)`

- 组件绑定的辅助函数

# Axios 拦截器 - 基于 promise 的 HTTP 库

- 单请求配置options:`axios.post(url,data,options)`
- 全局配置defaults:`this.$axios.defaults`
- config:请求拦截器中的参数
- response.config 响应拦截器中的参数
- options
  - baseURL基础URL路径
  - params查询字符串（对象）
  - transformRequest:function(post请求传递的数据){ 转换请求体数据}
  - transformResponse:function(res){自己转换响应回来的数据} 转换响应体数据
  - headers 请求头信息
  - data 请求体数据
  - timeout 请求超时，请求多久以后没有相应算超时（毫秒）
    基本用法

> ```js
> axios.get('http://baidu.com')
>  .then(res => console.log(res.data))
>  .catch(err => console.log(err))
> ```

合并请求

- 取消请求 `CancelToken.source`





# 其他

## [vue-resource 实现 get, post, jsonp请求](https://github.com/pagekit/vue-resource)

除了 vue-resource 之外，还可以使用 `axios` 的第三方包实现实现数据的请求
1. 之前的学习中，如何发起数据请求？
2. 常见的数据请求类型？  get  post jsonp
3. 测试的URL请求资源地址：
 + get请求地址： http://vue.studyit.io/api/getlunbo
 + post请求地址：http://vue.studyit.io/api/post
 + jsonp请求地址：http://vue.studyit.io/api/jsonp
4. JSONP的实现原理
 + 由于浏览器的安全性限制，不允许AJAX访问 协议不同、域名不同、端口号不同的 数据接口，浏览器认为这种访问不安全；
 + 可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式，称作JSONP（注意：根据JSONP的实现原理，知晓，JSONP只支持Get请求）；
 + 具体实现过程：
  - 先在客户端定义一个回调方法，预定义对数据的操作；
  - 再把这个回调方法的名称，通过URL传参的形式，提交到服务器的数据接口；
  - 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行；
  - 客户端拿到服务器返回的字符串之后，当作Script脚本去解析执行，这样就能够拿到JSONP的数据了；
 + 带大家通过 Node.js ，来手动实现一个JSONP的请求例子；
 ```
    const http = require('http');
    // 导入解析 URL 地址的核心模块
    const urlModule = require('url');

    const server = http.createServer();
    // 监听 服务器的 request 请求事件，处理每个请求
    server.on('request', (req, res) => {
      const url = req.url;

      // 解析客户端请求的URL地址
      var info = urlModule.parse(url, true);

      // 如果请求的 URL 地址是 /getjsonp ，则表示要获取JSONP类型的数据
      if (info.pathname === '/getjsonp') {
        // 获取客户端指定的回调函数的名称
        var cbName = info.query.callback;
        // 手动拼接要返回给客户端的数据对象
        var data = {
          name: 'zs',
          age: 22,
          gender: '男',
          hobby: ['吃饭', '睡觉', '运动']
        }
        // 拼接出一个方法的调用，在调用这个方法的时候，把要发送给客户端的数据，序列化为字符串，作为参数传递给这个调用的方法：
        var result = `${cbName}(${JSON.stringify(data)})`;
        // 将拼接好的方法的调用，返回给客户端去解析执行
        res.end(result);
      } else {
        res.end('404');
      }
    });

    server.listen(3000, () => {
      console.log('server running at http://127.0.0.1:3000');
    });
 ```
5. vue-resource 的配置步骤：
 + 直接在页面中，通过`script`标签，引入 `vue-resource` 的脚本文件；
 + 注意：引用的先后顺序是：先引用 `Vue` 的脚本文件，再引用 `vue-resource` 的脚本文件；
6. 发送get请求：
```
getInfo() { // get 方式获取数据
  this.$http.get('http://127.0.0.1:8899/api/getlunbo').then(res => {
    console.log(res.body);
  })
}
```
7. 发送post请求：
```
postInfo() {
  var url = 'http://127.0.0.1:8899/api/post';
  // post 方法接收三个参数：
  // 参数1： 要请求的URL地址
  // 参数2： 要发送的数据对象
  // 参数3： 指定post提交的编码类型为 application/x-www-form-urlencoded
  this.$http.post(url, { name: 'zs' }, { emulateJSON: true }).then(res => {
    console.log(res.body);
  });
}
```
8. 发送JSONP请求获取数据：
```
jsonpInfo() { // JSONP形式从服务器获取数据
  var url = 'http://127.0.0.1:8899/api/jsonp';
  this.$http.jsonp(url).then(res => {
    console.log(res.body);
  });
}
```

## 配置本地数据库和数据接口API
1. 先解压安装 `PHPStudy`;
2. 解压安装 `Navicat` 这个数据库可视化工具，并激活；
3. 打开 `Navicat` 工具，新建空白数据库，名为 `dtcmsdb4`;
4. 双击新建的数据库，连接上这个空白数据库，在新建的数据库上`右键` -> `运行SQL文件`，选择并执行 `dtcmsdb4.sql` 这个数据库脚本文件；如果执行不报错，则数据库导入完成；
5. 进入文件夹 `vuecms3_nodejsapi` 内部，执行 `npm i` 安装所有的依赖项；
6. 先确保本机安装了 `nodemon`, 没有安装，则运行 `npm i nodemon -g` 进行全局安装，安装完毕后，进入到 `vuecms3_nodejsapi`目录 -> `src`目录 -> 双击运行 `start.bat`
7. 如果API启动失败，请检查 PHPStudy 是否正常开启，同时，检查 `app.js` 中第 `14行` 中数据库连接配置字符串是否正确；PHPStudy 中默认的 用户名是root，默认的密码也是root

## [Vue中的动画](https://cn.vuejs.org/v2/guide/transitions.html)
为什么要有动画：动画能够提高用户的体验，帮助用户更好的理解页面中的功能；

### 使用过渡类名
1. HTML结构：
```
<div id="app">
    <input type="button" value="动起来" @click="myAnimate">
    <!-- 使用 transition 将需要过渡的元素包裹起来 -->
    <transition name="fade">
      <div v-show="isshow">动画哦</div>
    </transition>
  </div>
```
2. VM 实例：
```
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
  el: '#app',
  data: {
    isshow: false
  },
  methods: {
    myAnimate() {
      this.isshow = !this.isshow;
    }
  }
});
```
3. 定义两组类样式：
```
/* 定义进入和离开时候的过渡状态 */
    .fade-enter-active,
    .fade-leave-active {
      transition: all 0.2s ease;
      position: absolute;
    }

    /* 定义进入过渡的开始状态 和 离开过渡的结束状态 */
    .fade-enter,
    .fade-leave-to {
      opacity: 0;
      transform: translateX(100px);
    }
```

### [使用第三方 CSS 动画库](https://cn.vuejs.org/v2/guide/transitions.html#自定义过渡类名)
1. 导入动画类库：
```
<link rel="stylesheet" type="text/css" href="./lib/animate.css">
```
2. 定义 transition 及属性：
```
<transition
	enter-active-class="fadeInRight"
    leave-active-class="fadeOutRight"
    :duration="{ enter: 500, leave: 800 }">
  	<div class="animated" v-show="isshow">动画哦</div>
</transition>
```

### 使用动画钩子函数
1. 定义 transition 组件以及三个钩子函数：
```
<div id="app">
    <input type="button" value="切换动画" @click="isshow = !isshow">
    <transition
    @before-enter="beforeEnter"
    @enter="enter"
    @after-enter="afterEnter">
      <div v-if="isshow" class="show">OK</div>
    </transition>
  </div>
```
2. 定义三个 methods 钩子方法：
```
methods: {
        beforeEnter(el) { // 动画进入之前的回调
          el.style.transform = 'translateX(500px)';
        },
        enter(el, done) { // 动画进入完成时候的回调
          el.offsetWidth;
          el.style.transform = 'translateX(0px)';
          done();
        },
        afterEnter(el) { // 动画进入完成之后的回调
          this.isshow = !this.isshow;
        }
      }
```
3. 定义动画过渡时长和样式：
```
.show{
      transition: all 0.4s ease;
    }
```


### [v-for 的列表过渡](https://cn.vuejs.org/v2/guide/transitions.html#列表的进入和离开过渡)
1. 定义过渡样式：
```
<style>
    .list-enter,
    .list-leave-to {
      opacity: 0;
      transform: translateY(10px);
    }

    .list-enter-active,
    .list-leave-active {
      transition: all 0.3s ease;
    }
</style>
```
2. 定义DOM结构，其中，需要使用 transition-group 组件把v-for循环的列表包裹起来：
```
  <div id="app">
    <input type="text" v-model="txt" @keyup.enter="add">

    <transition-group tag="ul" name="list">
      <li v-for="(item, i) in list" :key="i">{{item}}</li>
    </transition-group>
  </div>
```
3. 定义 VM中的结构：
```
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        txt: '',
        list: [1, 2, 3, 4]
      },
      methods: {
        add() {
          this.list.push(this.txt);
          this.txt = '';
        }
      }
    });
```


### 列表的排序过渡
`<transition-group>` 组件还有一个特殊之处。不仅可以进入和离开动画，**还可以改变定位**。要使用这个新功能只需了解新增的 `v-move` 特性，**它会在元素的改变定位的过程中应用**。
+ `v-move` 和 `v-leave-active` 结合使用，能够让列表的过渡更加平缓柔和：
```
.v-move{
  transition: all 0.8s ease;
}
.v-leave-active{
  position: absolute;
}
```



## 相关文章
1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [pagekit/vue-resource](https://github.com/pagekit/vue-resource)
6. [navicat如何导入sql文件和导出sql文件](https://jingyan.baidu.com/article/a65957f4976aad24e67f9b9b.html)
7. [贝塞尔在线生成器](http://cubic-bezier.com/#.4,-0.3,1,.33)







## 相关文件
1. [URL中的hash（井号）](http://www.cnblogs.com/joyho/articles/4430148.html)





## 在网页中会引用哪些常见的静态资源？
+ JS
 - .js  .jsx  .coffee  .ts（TypeScript  类 C# 语言）
+ CSS
 - .css  .less   .sass  .scss
+ Images
 - .jpg   .png   .gif   .bmp   .svg
+ 字体文件（Fonts）
 - .svg   .ttf   .eot   .woff   .woff2
+ 模板文件
 - .ejs   .jade  .vue【这是在webpack中定义组件的方式，推荐这么用】


## 网页中引入的静态资源多了以后有什么问题？？？
1. 网页加载速度慢， 因为 我们要发起很多的二次请求；
2. 要处理错综复杂的依赖关系


## 如何解决上述两个问题
1. 合并、压缩、精灵图、图片的Base64编码
2. 可以使用之前学过的requireJS、也可以使用webpack可以解决各个包之间的复杂依赖关系；

## 什么是webpack?
webpack 是前端的一个项目构建工具，它是基于 Node.js 开发出来的一个前端工具；


## 如何完美实现上述的2种解决方案
1. 使用Gulp， 是基于 task 任务的；
2. 使用Webpack， 是基于整个项目进行构建的；
+ 借助于webpack这个前端自动化构建工具，可以完美实现资源的合并、打包、压缩、混淆等诸多功能。
+ 根据官网的图片介绍webpack打包的过程
+ [webpack官网](http://webpack.github.io/)

## webpack安装的两种方式
1. 运行`npm i webpack -g`全局安装webpack，这样就能在全局使用webpack的命令
2. 在项目根目录中运行`npm i webpack --save-dev`安装到项目依赖中

## 初步使用webpack打包构建列表隔行变色案例
1. 运行`npm init`初始化项目，使用npm管理项目中的依赖包
2. 创建项目基本的目录结构
3. 使用`cnpm i jquery --save`安装jquery类库
4. 创建`main.js`并书写各行变色的代码逻辑：
```
	// 导入jquery类库
    import $ from 'jquery'

    // 设置偶数行背景色，索引从0开始，0是偶数
    $('#list li:even').css('backgroundColor','lightblue');
    // 设置奇数行背景色
    $('#list li:odd').css('backgroundColor','pink');
```
5. 直接在页面上引用`main.js`会报错，因为浏览器不认识`import`这种高级的JS语法，需要使用webpack进行处理，webpack默认会把这种高级的语法转换为低级的浏览器能识别的语法；
6. 运行`webpack 入口文件路径 输出文件路径`对`main.js`进行处理：
```
webpack src/js/main.js dist/bundle.js
```

## 使用webpack的配置文件简化打包时候的命令
1. 在项目根目录中创建`webpack.dev.js`
2. 由于运行webpack命令的时候，webpack需要指定入口文件和输出文件的路径，所以，我们需要在`webpack.dev.js`中配置这两个路径：
```
    // 导入处理路径的模块
    var path = require('path');

    // 导出一个配置对象，将来webpack在启动的时候，会默认来查找webpack.config.js，并读取这个文件中导出的配置对象，来进行打包处理
    module.exports = {
        entry: path.resolve(__dirname, 'src/js/main.js'), // 项目入口文件
        output: { // 配置输出选项
            path: path.resolve(__dirname, 'dist'), // 配置输出的路径
            filename: 'bundle.js' // 配置输出的文件名
        }
    }
```

## 实现webpack的实时打包构建
1. 由于每次重新修改代码之后，都需要手动运行webpack打包的命令，比较麻烦，所以使用`webpack-dev-server`来实现代码实时打包编译，当修改代码之后，会自动进行打包构建。
2. 运行`cnpm i webpack-dev-server --save-dev`安装到开发依赖
3. 安装完成之后，在命令行直接运行`webpack-dev-server`来进行打包，发现报错，此时需要借助于`package.json`文件中的指令，来进行运行`webpack-dev-server`命令，在`scripts`节点下新增`"dev": "webpack-dev-server"`指令，发现可以进行实时打包，但是dist目录下并没有生成`bundle.js`文件，这是因为`webpack-dev-server`将打包好的文件放在了内存中
 + 把`bundle.js`放在内存中的好处是：由于需要实时打包编译，所以放在内存中速度会非常快
 + 这个时候访问webpack-dev-server启动的`http://localhost:8080/`网站，发现是一个文件夹的面板，需要点击到src目录下，才能打开我们的index首页，此时引用不到bundle.js文件，需要修改index.html中script的src属性为:`<script src="../bundle.js"></script>`
 + 为了能在访问`http://localhost:8080/`的时候直接访问到index首页，可以使用`--contentBase src`指令来修改dev指令，指定启动的根目录：
 ```
 "dev": "webpack-dev-server --contentBase src"
 ```
 同时修改index页面中script的src属性为`<script src="bundle.js"></script>`

## 使用`html-webpack-plugin`插件配置启动页面
由于使用`--contentBase`指令的过程比较繁琐，需要指定启动的目录，同时还需要修改index.html中script标签的src属性，所以推荐大家使用`html-webpack-plugin`插件配置启动页面.
1. 运行`cnpm i html-webpack-plugin --save-dev`安装到开发依赖
2. 修改`webpack.dev.js`配置文件如下：
```
    // 导入处理路径的模块
    var path = require('path');
    // 导入自动生成HTMl文件的插件
    var htmlWebpackPlugin = require('html-webpack-plugin');

    module.exports = {
        entry: path.resolve(__dirname, 'src/js/main.js'), // 项目入口文件
        output: { // 配置输出选项
            path: path.resolve(__dirname, 'dist'), // 配置输出的路径
            filename: 'bundle.js' // 配置输出的文件名
        },
        plugins:[ // 添加plugins节点配置插件
            new htmlWebpackPlugin({
                template:path.resolve(__dirname, 'src/index.html'),//模板路径
                filename:'index.html'//自动生成的HTML文件的名称
            })
        ]
    }
```
3. 修改`package.json`中`script`节点中的dev指令如下：
```
"dev": "webpack-dev-server"
```
4. 将index.html中script标签注释掉，因为`html-webpack-plugin`插件会自动把bundle.js注入到index.html页面中！

## 实现自动打开浏览器、热更新和配置浏览器的默认端口号
**注意：热更新在JS中表现的不明显，可以从一会儿要讲到的CSS身上进行介绍说明！**
### 方式1：
+ 修改`package.json`的script节点如下，其中`--open`表示自动打开浏览器，`--port 4321`表示打开的端口号为4321，`--hot`表示启用浏览器热更新：
```
"dev": "webpack-dev-server --hot --port 4321 --open"
```

### 方式2：
1. 修改`webpack.dev.js`文件，新增`devServer`节点如下：
```
devServer:{
        hot:true,
        open:true,
        port:4321
    }
```
2. 在头部引入`webpack`模块：
```
var webpack = require('webpack');
```
3. 在`plugins`节点下新增：
```
new webpack.HotModuleReplacementPlugin()
```

## 使用webpack打包css文件
1. 运行`cnpm i style-loader css-loader --save-dev`
2. 修改`webpack.dev.js`这个配置文件：
```
module: { // 用来配置第三方loader模块的
        rules: [ // 文件的匹配规则
            { test: /\.css$/, use: ['style-loader', 'css-loader'] }//处理css文件的规则
        ]
    }
```
3. 注意：`use`表示使用哪些模块来处理`test`所匹配到的文件；`use`中相关loader模块的调用顺序是从后向前调用的；

## 使用webpack打包less文件
1. 运行`cnpm i less-loader less -D`
2. 修改`webpack.dev.js`这个配置文件：
```
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
```

## 使用webpack打包sass文件
1. 运行`cnpm i sass-loader node-sass --save-dev`
2. 在`webpack.dev.js`中添加处理sass文件的loader模块：
```
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```

## 使用webpack处理css中的路径
1. 运行`cnpm i url-loader file-loader --save-dev`
2. 在`webpack.dev.js`中添加处理url路径的loader模块：
```
{ test: /\.(png|jpg|gif)$/, use: 'url-loader' }
```
3. 可以通过`limit`指定进行base64编码的图片大小；只有小于指定字节（byte）的图片才会进行base64编码：
```
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=43960' },
```

## 使用babel处理高级JS语法
1. 运行`cnpm i babel-core babel-loader babel-plugin-transform-runtime --save-dev`安装babel的相关loader包
2. 运行`cnpm i babel-preset-es2015 babel-preset-stage-0 --save-dev`安装babel转换的语法
3. 在`webpack.dev.js`中添加相关loader模块，其中需要注意的是，一定要把`node_modules`文件夹添加到排除项：
```
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
```
4. 在项目根目录中添加`.babelrc`文件，并修改这个配置文件如下：
```
{
    "presets":["es2015", "stage-0"],
    "plugins":["transform-runtime"]
}
```
5. **注意：语法插件`babel-preset-es2015`可以更新为`babel-preset-env`，它包含了所有的ES相关的语法；**

## 相关文章
[babel-preset-env：你需要的唯一Babel插件](https://segmentfault.com/p/1210000008466178)
[Runtime transform 运行时编译es6](https://segmentfault.com/a/1190000009065987)







## 注意：

有时候使用`npm i node-sass -D`装不上，这时候，就必须使用 `cnpm i node-sass -D`



## 在普通页面中使用render函数渲染组件



## 在webpack中配置.vue组件页面的解析

1. 运行`cnpm i vue -S`将vue安装为运行依赖；

2. 运行`cnpm i vue-loader vue-template-compiler -D`将解析转换vue的包安装为开发依赖；

3. 运行`cnpm i style-loader css-loader -D`将解析转换CSS的包安装为开发依赖，因为.vue文件中会写CSS样式；

4. 在`webpack.dev.js`中，添加如下`module`规则：

```

module: {

    rules: [

      { test: /\.css$/, use: ['style-loader', 'css-loader'] },

      { test: /\.vue$/, use: 'vue-loader' }

    ]

  }

```

5. 创建`App.js`组件页面：

```

    <template>

      <!-- 注意：在 .vue 的组件中，template 中必须有且只有唯一的根元素进行包裹，一般都用 div 当作唯一的根元素 -->

      <div>

        <h1>这是APP组件 - {{msg}}</h1>

        <h3>我是h3</h3>

      </div>

    </template>



    <script>

    // 注意：在 .vue 的组件中，通过 script 标签来定义组件的行为，需要使用 ES6 中提供的 export default 方式，导出一个vue实例对象

    export default {

      data() {

        return {

          msg: 'OK'

        }

      }

    }

    </script>



    <style scoped>

    h1 {

      color: red;

    }

    </style>

```

6. 创建`main.js`入口文件：

```

    // 导入 Vue 组件

    import Vue from 'vue'



    // 导入 App组件

    import App from './components/App.vue'



    // 创建一个 Vue 实例，使用 render 函数，渲染指定的组件

    var vm = new Vue({

      el: '#app',

      render: c => c(App)

    });

```

## 在使用webpack构建的Vue项目中使用模板对象？
1. 在`webpack.dev.js`中添加`resolve`属性：
```
resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    }
  }
```



## ES6中语法使用总结

1. 使用 `export default` 和 `export` 导出模块中的成员; 对应ES5中的 `module.exports` 和 `export`

2. 使用 `import ** from **` 和 `import '路径'` 还有 `import {a, b} from '模块标识'` 导入其他模块

3. 使用箭头函数：`(a, b)=> { return a-b; }`



## 在vue组件页面中，集成vue-router路由模块

[vue-router官网](https://router.vuejs.org/)

1. 导入路由模块：

```

import VueRouter from 'vue-router'

```

2. 安装路由模块：

```

Vue.use(VueRouter);

```

3. 导入需要展示的组件:

```

import login from './components/account/login.vue'

import register from './components/account/register.vue'

```

4. 创建路由对象:

```

var router = new VueRouter({

  routes: [

    { path: '/', redirect: '/login' },

    { path: '/login', component: login },

    { path: '/register', component: register }

  ]

});

```

5. 将路由对象，挂载到 Vue 实例上:

```

var vm = new Vue({

  el: '#app',

  // render: c => { return c(App) }

  render(c) {

    return c(App);

  },

  router // 将路由对象，挂载到 Vue 实例上

});

```

6. 改造App.vue组件，在 template 中，添加`router-link`和`router-view`：

```

    <router-link to="/login">登录</router-link>

    <router-link to="/register">注册</router-link>



    <router-view></router-view>

```



## 组件中的css作用域问题



## 抽离路由为单独的模块



## 使用 饿了么的 MintUI 组件

[Github 仓储地址](https://github.com/ElemeFE/mint-ui)

[Mint-UI官方文档](http://mint-ui.github.io/#!/zh-cn)

1. 导入所有MintUI组件：

```

import MintUI from 'mint-ui'

```

2. 导入样式表：

```

import 'mint-ui/lib/style.css'

```

3. 在 vue 中使用 MintUI：

```

Vue.use(MintUI)

```

4. 使用的例子：

```

<mt-button type="primary" size="large">primary</mt-button>

```



## 使用 MUI 组件

[官网首页](http://dev.dcloud.net.cn/mui/)

[文档地址](http://dev.dcloud.net.cn/mui/ui/)

1. 导入 MUI 的样式表：

```

import '../lib/mui/css/mui.min.css'

```

2. 在`webpack.dev.js`中添加新的loader规则：

```

{ test: /\.(png|jpg|gif|ttf)$/, use: 'url-loader' }

```

3. 根据官方提供的文档和example，尝试使用相关的组件



## 将项目源码托管到oschina中

1. 点击头像 -> 修改资料 -> SSH公钥 [如何生成SSH公钥](http://git.mydoc.io/?t=154712)

2. 创建自己的空仓储，使用 `git config --global user.name "用户名"` 和 `git config --global user.email ***@**.com` 来全局配置提交时用户的名称和邮箱

3. 使用 `git init` 在本地初始化项目

4. 使用 `touch README.md` 和 `touch .gitignore` 来创建项目的说明文件和忽略文件；

5. 使用 `git add .` 将所有文件托管到 git 中

6. 使用 `git commit -m "init project"` 将项目进行本地提交

7. 使用 `git remote add origin 仓储地址`将本地项目和远程仓储连接，并使用origin最为远程仓储的别名

8. 使用 `git push -u origin master` 将本地代码push到仓储中



## App.vue 组件的基本设置

1. 头部的固定导航栏使用 `Mint-UI` 的 `Header` 组件；

2. 底部的页签使用 `mui` 的 `tabbar`;

3. 购物车的图标，使用 `icons-extra` 中的 `mui-icon-extra mui-icon-extra-cart`，同时，应该把其依赖的字体图标文件 `mui-icons-extra.ttf`，复制到 `fonts` 目录下！

4. 将底部的页签，改造成 `router-link` 来实现单页面的切换；

5. Tab Bar 路由激活时候设置高亮的两种方式：

 + 全局设置样式如下：

 ```

 	.router-link-active{

      	color:#007aff !important;

    }

 ```

 + 或者在 `new VueRouter` 的时候，通过 `linkActiveClass` 来指定高亮的类：

 ```

 	// 创建路由对象

    var router = new VueRouter({

      routes: [

        { path: '/', redirect: '/home' }

      ],

      linkActiveClass: 'mui-active'

    });

 ```



## 实现 tabbar 页签不同组件页面的切换

1. 将 tabbar 改造成 `router-link` 形式，并指定每个连接的 `to` 属性；

2. 在入口文件中导入需要展示的组件，并创建路由对象：

```

    // 导入需要展示的组件

    import Home from './components/home/home.vue'

    import Member from './components/member/member.vue'

    import Shopcar from './components/shopcar/shopcar.vue'

    import Search from './components/search/search.vue'



    // 创建路由对象

    var router = new VueRouter({

      routes: [

        { path: '/', redirect: '/home' },

        { path: '/home', component: Home },

        { path: '/member', component: Member },

        { path: '/shopcar', component: Shopcar },

        { path: '/search', component: Search }

      ],

      linkActiveClass: 'mui-active'

    });

```



## 使用 mt-swipe 轮播图组件

1. 假数据：

```

lunbo: [

        'http://www.itcast.cn/images/slidead/BEIJING/2017440109442800.jpg',

        'http://www.itcast.cn/images/slidead/BEIJING/2017511009514700.jpg',

        'http://www.itcast.cn/images/slidead/BEIJING/2017421414422600.jpg'

      ]

```

2. 引入轮播图组件：

```

<!-- Mint-UI 轮播图组件 -->

    <div class="home-swipe">

      <mt-swipe :auto="4000">

        <mt-swipe-item v-for="(item, i) in lunbo" :key="i">

          <img :src="item" alt="">

        </mt-swipe-item>

      </mt-swipe>

    </div>

  </div>

```



## 在`.vue`组件中使用`vue-resource`获取数据

1. 运行`cnpm i vue-resource -S`安装模块

2. 导入 vue-resource 组件

```

import VueResource from 'vue-resource'

```

3. 在vue中使用 vue-resource 组件

```

Vue.use(VueResource);

```



## [键盘修饰符以及自定义键盘修饰符](https://cn.vuejs.org/v2/guide/events.html#键值修饰符)

1. 通过`Vue.config.keyCodes.名称 = 按键值`来自定义案件修饰符的别名：

```
Vue.config.keyCodes.f2 = 113;
```

2. 使用自定义的按键修饰符：

```
<input type="text" v-model="name" @keyup.f2="add">
```



## [自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)

1. 自定义全局和局部的 自定义指令：

```js
	// 自定义全局指令 v-focus，为绑定的元素自动获取焦点：
    Vue.directive('focus', {
      inserted: function (el) { // inserted 表示被绑定元素插入父节点时调用
        el.focus();
      }
    });
    // 自定义局部指令 v-color 和 v-font-weight，为绑定的元素设置指定的字体颜色 和 字体粗细：
      directives: {
        color: { // 为元素设置指定的字体颜色
          bind(el, binding) {
            el.style.color = binding.value;
          }
        },
        'font-weight': function (el, binding2) { // 自定义指令的简写形式，等同于定义了 bind 和 update 两个钩子函数
          el.style.fontWeight = binding2.value;
        }
      }
```

2. 自定义指令的使用方式：

```html
<input type="text" v-model="searchName" v-focus v-color="'red'" v-font-weight="900">
```



## 相关文章
1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [Vue.js双向绑定的实现原理](http://www.cnblogs.com/kidney/p/6052935.html)



## Vue调试工具`vue-devtools`的安装步骤和使用

[Vue.js devtools - 翻墙安装方式 - 推荐](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN)



## `nrm`的安装使用

作用：提供了一些最常用的NPM包镜像地址，能够让我们快速的切换安装包时候的服务器地址；
什么是镜像：原来包刚一开始是只存在于国外的NPM服务器，但是由于网络原因，经常访问不到，这时候，我们可以在国内，创建一个和官网完全一样的NPM服务器，只不过，数据都是从人家那里拿过来的，除此之外，使用方式完全一样；

1. 运行`npm i nrm -g`全局安装`nrm`包；
2. 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址；
3. 使用`nrm use npm`或`nrm use taobao`切换不同的镜像源地址；

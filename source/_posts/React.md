---
title: React
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-19 18:55:19
tags: ['js', 'react']
password:
summary:
categories:
---
# React
[demo地址](https://github.com/Hojondo/reactDemo)
React 的世界里一切皆是组件，我们使用class语法构建一个最基本的组件，组件的使用方式和HTML相同，组件的render函数返回页面渲染的一个JSX，然后使用ReactDom渲染到页面里

## React 和 Vue的对比
组件化方面
1. 模块化： 从代码的角度进行分析，把一些可复用的代码抽离成单个模块，便于项目维护和开发；如Node
2. 组件化：从UI界面角度进行分析，把一些可复用的UI元素，抽离为单独的组件。
3. Vue如何实现组件化：`.vue`文件
   - template 结构
   - script 行为
   - style 样式
4. React如何实现组件化：一切以js来表现。ES6、ES7（async和await...）

移动APP开发体验方面

- VUE是 结合Week（阿里）
- React, 结合ReactNative

## 几个核心概念

1. **虚拟DOM**

   为了实现DOM元素的高效更新，性能问题，频繁操作DOM

   需要**按需渲染**，

   获取内存中的新旧DOM树，对比，按需更新DOM，浏览器中并无直接提供获取DOM树的API，因此 用js对象模拟新旧DOM树的虚拟DOM出现了。

2. **DIFF算法**

   新旧DOM树的对比

   - tree diff： 新旧两颗DOM树，逐层对比的过程
   - Component diff：在进行tree diff过程中，每一层中 组件级别的对比。
   - Element diff ：在进行component diff过程中，如果两个组件类型相同，则进行元素级别的对比。



实践步骤：

### webpack4.x 和3的简述差别

约定大于配置，如 约定的入口路径是 src -> index.js，不用再写entry字段

新增mode值

## 项目实践流程

1. `npm init -y`

2. 建目录src和dist

3. 安装webpack4及相关

   `npm i webpack webpack-cli -D`

   `npm i webpack-dev-server -D`

4. 新建`webpack.config.js`，配置

   

5. 安装react

   `npm i react react-dom -S`

   react包是专门用来创建组件和js虚拟DOM的，同时组件的生命周期都在这个包里

   React-dom是 专门进行DOM渲染和操作的，最主要的应用场景是`ReactDOM.render()`

6. 在`index.html`中 创建容器以供入口js调用render

   ```html
   <div id="app"></div>
   ```

7. 导入包 在入口文件`index.js`

   ```js
   import React from 'react'
   import ReactDom from 'react-dom'
   
   // 创建虚拟DOM元素
   const myDiv = React.createElement('div', {id: 'xx',style: {display: 'block'}}, '哈哈哈', React.createElement('p',{},'2'));
   
   // const myDiv = <div id="d1">第一个div元素</div>;
   
   // 使用ReactDom 吧虚拟DOM渲染到页面上
   // 参数1 要渲染的那个虚拟DOM元素
   // 参数2 指定页面上的一个DOM容器
   ReactDom.render(myDiv, document.getElementById('app'));
   ```

   

8. 安装`babel`插件

   `npm i babel-core babel-loader babel-plugin-transform-runtime -D`

   语法包 `npm i babel-preset-env babel-preset-stage-0 -D`

   能够识别转换jsx的语法包`npm i babel-preset-react -D`

9. `Webpack.config.js`中配置babel-loader

10. 配置`.babelrc`配置文件

   ```json
   "presets": ["env","stage-0","react"],
   "plugins": []
   ```

   

## JSX语法

- jsx语法的本质：并不是直接把jsx渲染到页面上，而是内部先转换成了createElement形式，再渲染的

- jsx中混合写入js表达式：在jsx语法中把js代码写在`{}`中

  包括：渲染数字、字符串、布尔值、为属性绑定值、渲染jsx元素、渲染jsx元素数组、将普通字符数组转为jsx数组并渲染到页面上

- 注释

- 标签必须成堆出现，如果是单标签，则必须自闭合

- 在jsx中创建DOM的时候，所有的节点，必须有唯一的跟元素进行包裹

- 特殊的属性，使用`className`来代替`class`，`htmlFor`代替label的`for`属性

- 标签上写style的话，要用js对象

- 引入css文件

  `npm i style-loader css-loader -D`；

  配置webpack.config.js的rules

  组件中`import cssobj from './demoStyle.css'`

  给css模块化`use['css-loader?modules']`，但是模块化只针对.class和#id选择器，不会将标签选择器模块化

  给css自定义生成类名格式`use['css-loader?localIdentName=[path][name][local][hash:6]']`

  ​	其可选参数是：

  	- [path]表示样式表相对于项目根目录所在路径
  	- [name]表示样式表文件名称
  	- [local]表示样式的类名定义名称
  	- [hash:length]表示最多32位的hash值

  css可选`:local()`(开启modules之后默认 不用写)或者全局`:global(.xxclass)`(不会被模块化)

  对于第三方的样式表，规定都以.css结尾，这样的话 我们不会为普通的.css启用模块化 直接import 'bootstrap'就好，自己的样式表都以.scss或.less结尾，于是 只为`.scss`或`.less`文件启用模块化

  安装`npm i sass-loader node-sass -D`



## 创建组件的两种方式

1. 使用构造函数创建组件

   如果要接受外间传递的数据，需要在够赞函数的参数列表中使用props来接收

   必须向外return一个合法的jsx创建的虚拟dom

   - 伏组件向子组件传递数据
   - 使用{...obj}属性扩散传递数据
   - 注意：组件的名称必须是大写
   - 将组件封装到单独的文件中
   - 引入时省略`.jsx`后缀名

2. 使用class关键自关键组件

3. 俩者的区别

   1. class关键子创建的组件有自己的私有数据和生命周期，但是function创建的组件 只有props，没有自己的私有数据和生命周期
   2. 用构造函数创建的组件：叫做“无状态组件”，无state和生命周期，但是运行效率较高，很少用
   3. 用class关键字创建出来的组件：叫做“有状态组件”
   



## 引入样式

1. `import styles from './style.js' `

2. `import cssObj from '@/css/demoStyle.scss'`

   ```js
   // webpack.config.js 
   {
        test: /\.scss$/,
        use: ['style-loader', {
            loader: 'css-loader',
            options: {
                modules: {
                    localIdentName: '[path][name]-[local]-[hash:5]'
                },
                sourceMap: true
            }
        }, 'sass-loader']
    },
   ```
```
   
   

## 绑定事件

事件绑定机制，相对于原生，是小驼峰命名，事件值必须是函数

`button onClick={function(){}}>按钮</button>`

​```js
// <button className="Btn" onClick={() => this.myClickFn()}>点击</button>
// 或 class 内 需要使用箭头函数
```

## setState

React 是单向绑定 单向数据流(js=>界面)

如果要为state中的数据重新赋值，不要用 `this.state.xx = xxx`

应用react提供的`this.setState({xx: xxx})`

对于input，需要三步，1手动监听onChange事件，2在事件中拿到最新的e.target.value，3调用this.setState({})

## react的生命周期

**旧的生命周期：**

![image-20191227111757608.png](https://i.loli.net/2019/12/27/LS2K13xhbVQZWT8.png)

1. 组件创建阶段，只执行一次
   - `componentWillMount`
   - `render`
   - `componentDidMount`
2. 组件运行阶段，根据props属性或state状态数据的改变，有选择的执行0到多次
   - `componentWillReceiveProps`
   - `shouldComponentUpdate`
   - `componentWillUpdate`
   - `render`
   - `componentDidupdate`
3. 组件销毁阶段，只执行一次
   - `componentWillUnmount`

**新的v16.2之后生命周期**

> 图示[来源](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
>
> ![image.png](https://i.loli.net/2019/12/27/ApcXH3ybk7KGd9h.png)

[生命周期详解 参考教程](https://juejin.im/post/5b6f1800f265da282d45a79a)

1. 挂载阶段

   - `constructor`

   - `static getDerivedStateFromProps` ：一个静态方法，不能在此函数里面使用this，这个函数有两个参数props和state，分别指接收到的新参数和当前的state对象，这个函数会**返回一个对象用来更新当前的state对象**，如果不需要更新可以返回null

     该函数会在挂载时，接收到新的props，调用了`setState`和`forceUpdate`时被调用,这个方法就是为了取代之前的`componentWillMount`、`componentWillReceiveProps`和`componentWillUpdate`

     *getDeriveStateFromProps被设计成一个static方法，就是纯粹的获取父组件props，增量更新本组件的state*

   - [废弃]`componentWillMount/UNSAFE_componentWillMount`

   - `render`

   - `componentDidMount`

2. 更新阶段

   - [废弃]`componentWillReceiveProps/UNSAFE_componentWillReceiveProps`

   - `static getDerivedStateFromProps`

   - `shouldComponentUpdate`

   - [废弃]`componentWillUpdate/UNSAFE_componentWillUpdate`

   - `render`

   - `getSnapshotBeforeUpdate` ：这个方法在`render`之后，`componentDivUpdate`之前调用，表示之前的属性和之前的state，这个函数有一个返回值，会作为第三个参数传给`componentDidUpdate`，如果你不想要返回值，可返回null，不写的话控制台会warning，所以**此方法必须配合`componentDivUpdate`方法一起使用**

     此方法 为了取代之前的`componentWillUpdate`

   - `componentDivUpdate`

3. 卸载阶段

   - `componentWillUnmount`
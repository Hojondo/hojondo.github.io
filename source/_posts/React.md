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
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
开坑
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

4. 安装react

   `npm i react react-dom -S`

   react包是专门用来创建组件和js虚拟DOM的，同时组件的生命周期都在这个包里

   React-dom是 专门进行DOM渲染和操作的，最主要的应用场景是`ReactDOM.render()`

5. 在`index.html`中 创建容器以供入口js调用render



1. 安装`babel`插件

   `npm i babel-core babel-loader babel-plugin-transform-runtime -D`

   语法包 `npm i babel-preset-env babel-preset-stage-0 -D`

2. 安装能够识别转换jsx语法的包

   `npm i babel-preset-react -D`

3. 配置`.babelrc`配置文件

## JSX语法


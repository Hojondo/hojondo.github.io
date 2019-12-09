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
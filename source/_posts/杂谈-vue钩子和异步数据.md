---
title: vue-钩子顺序和数据异步请求
top: false
cover: false
toc: true
mathjax: false
date: 2019-12-12 15:04:23
tags: ["vue"]
password:
summary:
categories: ["杂谈"]
---

vue 生命周期钩子函数，作者就没有设计成阻塞。但是为了实现上一个钩子的异步请求回调成功之前不允许下一个钩子的需求的话，就做不到了。

问题相关[segmentfault](https://segmentfault.com/q/1010000013787292)

所以目前有两种实现方式：

1. `vi-if`先禁止渲染，直到异步回来赋值之后才允许渲染

2. 同方法 1，在入口 js 里，先异步请求，拿到数据后才允许`new Vue({render})`

3. **放弃** 用[组件内路由守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#路由独享的守卫)的`next()`来控制，思路来源[掘金](https://juejin.im/post/5bfa4bb951882558ae3c171e)。

   _补充：beforeRouteEnter 的函数体中是访问不到当前组件的上下文的，需要在回调参参数 next（这是个函数引用）中，使用 next 这个函数的回调参数`vm（next(vm => {})）`中的`vm`才能访问到当前组件的上下文；beforeRouterEnter 的阻塞作用基本就废掉了_

   *二次补充：或者在 next 之前把数据存在 localStorage 里，next 之后在 create 钩子里拿。*结论是绕远了，更不如直接用 v-if 控制。

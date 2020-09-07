---
title: 杂谈-react-架构演变
top: false
cover: false
toc: true
mathjax: false
date: 2020-09-02 16:53:41
tags: ["react"]
password:
summary:
categories: ["杂谈"]
---

#

# 状态管理框架演变

## redux

### 中间件 middleware

### redux-saga/react-redux

## dva/rematch

两者的区别主要在于对异步的处理，dva 选择了用 generator，而 rematch 选择了用 async/await。

## dva

> dva 是个啥？

看 github 首页就知道了。是基于 React, Redux, redux-saga, react-router 封装的轻量级框架。不过看这个介绍可以知道，它的假设是，你要用 redux, redux-saga, react-router，可能还要用 umi。或者可以这样讲，作者认为这三个库是数据管理、副作用管理、路由管理的最佳实践或默认配置（在 [官方文档](https://dvajs.com/guide/introduce-class.html#数据流问题) 可以看到，这个假设是基于数据的）。

所以这么说吧：

```
dva = React-Router + Redux + Redux-Saga
```

> 解决了啥问题？

特性部分。讲到 4 点：

- 易学。作为进阶用户，我就忽略易学部分。提到配合 umi 后 0 API。那么是要增加一个假设/依赖
- elm 概念。暂且理解为是，更加「模块化」？这可能是一个优点
- 插件机制和 HMR。这是默认延续 redux middleware 和 HMR 的配置，是做产品的细活，但可能算不上竞争优势。而且 HMR 还需要另外配置？

所以，让我们关注在 **效率提升** 和 **API 简单** 这两点优点上。看了一下官方 demo，通过很多约定优于配置，将模板代码减少到了最少。节省的效率确实很爽！

> 生态圈问题咋解决？

可以看到，三大块：数据、路由、副作用分别只是包装了社区的最佳实践。

> 带来了啥新的问题？

我会考察的一些点：

- 社区支持程度：对 Alipay 团队的依赖。这个团队的响应力会不会成为产品的瓶颈？
- 现成方案搜索 🔍
- 假设变动的可能性和应对方案：通过约定优于配置，这在目前看来是个很好的提高效率的方案。但如果放在「时间演化」和「团队项目」（当然后者不存在）这个向度上去检验，当其中一者依赖发生更新，它需要这个产品团队去更新 dva；当其中一者不再是社区最佳实践时，这个项目还怎么办？换个角度考虑，不用 dva 而自己引入这三种依赖，是否能让你在同样变化来临时，具备迁移应用的路径？

> 更具体的一些问题

- 文件结构：简化了，通过把 actions/reducers/saga 写到一块，不仅导航起来方便一些，还形成了「业务模型」的概念
- 文件即路由：简化了 router 的配置
- 最佳实践：它定位是框架而非 library，这是设计思想，非常好
- 高级定制：可能会有一些很细微的场景是没法 100% out of the box 支持的，但对于简单应用来说应该还好

> 竞品是啥？

- [rematch](https://github.com/rematch/rematch)

> 学习资料。带着问题去看。写写评论

- https://dvajs.com/guide
- https://umijs.org/guide

总的来说，看完一些简介，目前对 dva 的认识是说：它纯粹是为了**提高开发效率**而创造，没有新的东西，是对前端三大框架的封装。它怎么达到这个目标呢？通过**约定式**、**框架本身隐含最佳实践**的方式。

#

> [参考 架构选型](https://github.com/linesh-simplicity/linesh-simplicity.github.io/issues/208) > [参考 架构选型演进](https://segmentfault.com/a/1190000017406286) > [参考状态管理框架演变](https://juejin.im/post/6844904138124296205)

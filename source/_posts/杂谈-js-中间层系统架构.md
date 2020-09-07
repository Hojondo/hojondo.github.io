---
title: 杂谈-js-中间层系统架构
top: false
cover: false
toc: true
mathjax: false
date: 2020-09-02 16:14:38
tags: ["js"]
password:
summary:
categories: ["杂谈"]
---

## Nextjs

![nextjs流程示例图](https://i.loli.net/2020/09/02/dfeiv39XGDkYyR5.png)
假设这是一个 blog 网站，首页是文章列表 (/ 和 /articles 都是渲染文章列表)，整个工作流程是这样的：

第一步，打开浏览器，访问这个 blog 网站的文章列表页面 (/ 或者 /articles)。
第二步，请求到达服务器的 nginx，nginx 设置的规则是，如果请求匹配 /api/v1/_，则转给 API 服务处理，否则如果匹配 /_，则转给 next.js 服务处理，所以 /articles 请求发送给了 next.js 服务。
第三步到第六步，next.js 收到 /articles 请求后，会选择渲染文章列表界面，假如这个 page 叫 ArticleListPage，在这个 component 中，next.js 的 getInitialProps() 方法会去访问 API 服务的 /api/v1/articles API，获得所有的 articles 数据 (不考虑分页)，然后渲染出 html，html head 中 link 了 bundle 的所有 js 代码，html 返给浏览器。
第七步，浏览器得到了 html 后，进行首屏渲染，用户马上就看到了内容。浏览器解析完 html 后，会继续下载 html 中 link 的 js 代码并执行，然后该网站就变成了一个 SPA，js 接管剩余的所有路由，并且 js 会把此页面用 js 再重新渲染一遍 (用户没有感知)。
第八步到第九步，用户点击某篇文章查看详情，比如 /articles/1，由于 js 在客户端已经接管了路由，所以这个请求并不会发送到服务端的 nginx，js 代码根据路由，切换到详情页面 (比如 ArticlePage) 组件，在这个组件在 getInitialProps() 方法中，它会发送 /api/v1/articles/1 的 ajax 请求去访问 API 获取文章的详情数据。
第十步到第十一步，/api/v1/articles/1 的请求到达服务端的 nginx 后，转发给 API 服务处理，API 从数据库查询得到结果返回给浏览器，浏览器渲染之。整个流程结束。
从上面可以看出，next.js 在服务端仅负责首屏渲染，剩余的都在客户端进行渲染，只有首次访问的请求会经过服务端的 next.js，剩余所有请求都直接到达 API 服务。

# Koa

> [流程详解参考](https://baurine.netlify.app/2019/08/16/website-architectures/#%E9%9C%80%E8%A6%81-ssr---nextjs)

---
title: EggJs
top: false
cover: false
toc: true
mathjax: false
date: 2020-07-17 15:55:34
tags: ["node框架"]
password:
summary:
categories: ["笔记"]
---

# 科普

https://segmentfault.com/a/1190000020510357
[egg vs nest](https://juejin.im/post/6844903906028290061)
[umi](http://www.fly63.com/article/detial/588)

# 对比

eggJs, NestJs, Koa, Express, Umi , Dva

## 基于插件的 Swagger-doc 接⼝定义

统⼀一异常处理理
基于扩展的 helper 响应统⼀一处理理
Validate 接⼝口格式检查
三层结构
jwt 统⼀一鉴权
⽂文件上传

## 手写 eggjs

- 下载`npm i egg-init -g`脚手架
- 约定大于配置 3 层结构： controler,model,service
- model 层 安装 mysql2 和 egg-sequelize 插件，在 config/plugin.js 中指定调用
- 手写简单版 egg - 文件夹 kgg
  - routes, controller, service, model, middleware, schedule
  - loader.js 加载以上

## 最佳实践例子

- api 接口文档 - egg-swagger-doc-feat
- restful 服务
- 表单校验
- 鉴权
- 生命周期函数
- 上传
- 持久化 - egg-mongoose

### 流程

1. controller -> user.js
   - 注释中 接口描述定义 contract 文件夹
   - npm i swagger-doc-feat，配置在 config->plugin 和 config.default.js
   - 接口文档自动生成 & 自动注册路由
2. 统一异常处理
   - middleware 文件夹 -> error_handle.js
3. 统一正常处理
   - eggJS 提供一个扩展 extend 文件夹 -> helper
4. 校验 validate
   - npm i egg-validate -S，配置在 config->plugin 和 config.default.js
   - ctx.validate(ctx.rule.createUserRequest)
   - 注： ctx.rule 是 swagger-doc-feat 插件自动生成的给到了 ctx
5. 持久化 mogoose
   - 密码 hash 化 插件 npm i egg-bcrypt -S
   - npm i egg-mongoose -S
   - controller 下
   ```js
   const payload = ctx.request.body || {};
   // 调用 Service 进行业务处理
   const res = await service.user.create(payload);
   ```
   - service 下
   ```js
   async create(payload) {
    const { ctx, service } = this
    payload.password = await this.ctx.genHash(payload.password) // genHash是插件bcrypt提供的
    return ctx.model.User.create(payload)
   }
   ```
6. 生命周期中实现 startup,teardown 方便测试接口
   egg 并没有暴露生命周期，如果想使用生命周期函数就声明一个 app.js。在生命周期 didReady 中执行
7. 鉴权
   - npm i jwt -S 配置在 config....
   - service -> actionToken.js 派发 token
   - service -> userAccess.js 登录，登出和获取当前用户 实现
8. 上传下载优化
   - 必须保证上传完才返回 responese , 需要使用插件 await-stream-ready，封装 stream.pipe()
   - 存在一种情况：还没上传完，流还没有完全消化，服务器崩溃，导致了前端页面卡死卡顿，就需要使用插件 stream-wormhole,使输入流完全消化掉，防止前端卡死

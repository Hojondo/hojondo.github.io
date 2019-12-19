---
title: webpack
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-20 11:36:11
tags: ['前端', '打包']
password:
summary:
categories:
---
开坑 webpack 4.x
配置备注





4 约定大于配置：

其中 入口是`src->index.js`

打包输出目录是`dist->main.js`



### 几个插件

#### html-webpack-plugin

#### clean-webpack-plugin

[npmjs地址](https://www.npmjs.com/package/clean-webpack-plugin)

每次构建前清理 `/dist` 文件夹，这样只会生成用到的文件

#### copy-webpack-plugin

[npmjs地址](https://www.npmjs.com/package/copy-webpack-plugin)



#### `webpack.config.js`配置

```js
module.exports = {
  mode: '',//
  plugins: [], //
  module: {}, //
}
```


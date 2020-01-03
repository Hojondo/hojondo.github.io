---
title: 杂谈-webpack-多入口多出口打包
top: false
cover: false
toc: true
mathjax: false
date: 2020-01-03 16:43:56
tags: ['webpack', '杂谈']
password:
summary:
categories:
---
## 需求背景
某天老板看到了京东商城不一样的二级域名，但是他们的UI风格和某些页面块是完全一致的。也要这样，项目代码根据业务构建目录，用同一个package.json，打包3个二级目录。。。OK.
## 第一步
entry入口多个
```js
// entry: [
//     "babel-polyfill",
//     path.join(__dirname, './src/index.js')
// ],
entry: {
    index1: path.join(__dirname, './src/index.js'),
    index2: path.join(__dirname, './src/index2.js'),
},
```
## 第二步
区分出口文件名和目录
```js
output: {
    filename: '[name]/[name].bundle.js',
    path: path.resolve(__dirname, './dist'),
    chunkFilename: '[name].bundle.js'
},
```
## 第三步
多次`HtmlWebpackPlugin`
```js
new HtmlWebpackPlugin({
    title: '测试1',
    filename: 'index1/index.html',
    chunks:['index1'],
    template: path.join(__dirname, './src/index.html')
}),
new HtmlWebpackPlugin({
    title: '2',
    filename: 'index2/index2.html',
    chunks:['index2'],
    template: path.join(__dirname, './src/index.html')
}),
```

> [教程来源](https://blog.csdn.net/xyphf/article/details/79824777)
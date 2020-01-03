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

## 封装 示例
```js
//config.js
const glob = require('glob')
const HtmlWebpackPlugin = require('html-webpack-plugin')

const getEntrys = function(entryPattern){
  let entrys = []
  let entry = {}
  glob.sync(entryPattern).forEach((path) => {
    let length = path.split('/').length - 1
    path = path.split('/')[length].split('.')[0]
    entry[path] = `./src/pages/${path}/${path}.js`
  })
  return entry
}

const getOutHtmls = function(entryPattern){
  let outHtmls = []
  glob.sync(entryPattern).forEach((path) => {
    let length = path.split('/').length - 1
    let filename = path.split('/')[length]
    let template = path
    pageName = path.split('/')[length].split('.')[0]
    outHtmls.push(new HtmlWebpackPlugin({
      filename,
      template,
      inject:true,
      chunks: [pageName, 'common'],
      minify: {
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
      },
      // chunksSortMode: 'dependency'
    }))
  })
  return outHtmls
}

module.exports = {
  getEntrys,
  getOutHtmls
}
```
```js
// webpack.config.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const CleanWebpackPlugin = require('clean-webpack-plugin')
const ManifestWebpackPlugin = require('webpack-manifest-plugin')
const ExtractTextPlugin = require("extract-text-webpack-plugin")
const webpack = require('webpack')
const multipe = require('./config/config.js')
const getEntrys = multipe.getEntrys
const getOutHtmls = multipe.getOutHtmls
//根据具体目录结构来确定路径
const entryPattern = './src/pages/**.html';

module.exports = {
  entry:getEntrys(entryPattern),
  output: {
    filename: '[name]/js/[name].[hash].bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  resolve: {
    extensions: ['.js', '.json'],
    alias: {
      '@': path.resolve(__dirname, './src'),
      'config':path.resolve(__dirname, './config'),
    }
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use:['babel-loader'],
        exclude: "/node_modules"
      },
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: ["css-loader?importLoaders=1&&minimize","postcss-loader"]
        }),
        exclude:"/node_modules"
      },
      // LESS
      {
        test: /\.less$/,
        use: ExtractTextPlugin.extract({ 
          fallback: 'style-loader', 
          use: ['css-loader?importLoaders=1&&minimize', 'postcss-loader', 'less-loader'] })
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        loader: 'file-loader',
        options:{
          limit:10000,
          name:'[name]/img/[name].[hash].[ext]'
        },
        exclude: "/node_modules"
      }
    ]
  },
  plugins: [
    new ManifestWebpackPlugin(),
    new CleanWebpackPlugin(['dist']),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'common' // 指定公共 bundle 的名称。
    }),
    new ExtractTextPlugin({
      filename:"[name]/css/[name].[contenthash].css",
      allChunks:true
    }),
    ...getOutHtmls(entryPattern)
  ]
}
```
> [教程来源](https://blog.csdn.net/xyphf/article/details/79824777)
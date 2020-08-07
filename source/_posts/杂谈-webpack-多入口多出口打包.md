---
title: webpack-多入口多出口打包
top: false
cover: false
toc: true
mathjax: false
date: 2020-01-03 16:43:56
tags: ["webpack"]
password:
summary:
categories: ["杂谈"]
---

## 需求背景

某天老板看到了京东商城不一样的二级域名，但是他们的 UI 风格和某些页面块是完全一致的。也要这样，项目代码根据业务构建目录，用同一个 package.json，打包 3 个二级目录。。。OK.

同时有两个硬核需求：

项目越来越多，维护量较大。起初，只有一两个项目时，代码管理和开发还是比较轻松的。但当老系统逐渐拆分，新项目的不断启动，问题就来了。项目越来越多，每一个都需要单独 vue-cli 生成，然后配置一堆参数。当一个配置的参数有变，所有的其他项目都需要联动调整，维护量较大。

每个项目，所属的业务不同。但是整体上应当： 系统 UI 一致、部分相同业务逻辑代码处理一致。重复 coding 很多，维护量也很大。当编码不受控时，各个项目的处理方式也可能各不相同。

**解决方法**
对于各种发现的问题，简单构想一下可以整合前台项目，通过 Webpack 按项目打包生成多个 SPA。

## 第一步

entry 入口多个

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

## 第四步

生产环境没问题了，开发环境 dev-server 需要额外配置一下

以上用了多页配置，加上了 HtmlWebpackPlugin 实现了，但是每次只能启动一个单页。虽然两个模板关系不是特别紧密，但是如果能一次直接启动全部肯定是更好的。

devServer.historyApiFallback。参照文档描述，只需要自己指定路由重写机制，就可以实现开发模式多页面注入并导航。

```js
devServer: {
    historyApiFallback: {
        rewrites: [
            { from: /^\/$/, to: '/Home/index.html' },
            { from: /^\/Course/, to: '/Course/index.html' },
          ]
    }
},
```

## 封装 示例

```js
//config.js
const glob = require("glob");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const getEntrys = function (entryPattern) {
  let entrys = [];
  let entry = {};
  glob.sync(entryPattern).forEach((path) => {
    let length = path.split("/").length - 1;
    path = path.split("/")[length].split(".")[0];
    entry[path] = `./src/pages/${path}/${path}.js`;
  });
  return entry;
};

const getOutHtmls = function (entryPattern) {
  let outHtmls = [];
  glob.sync(entryPattern).forEach((path) => {
    let length = path.split("/").length - 1;
    let filename = path.split("/")[length];
    let template = path;
    pageName = path.split("/")[length].split(".")[0];
    outHtmls.push(
      new HtmlWebpackPlugin({
        filename,
        template,
        inject: true,
        chunks: [pageName, "common"],
        minify: {
          removeComments: true,
          collapseWhitespace: true,
          removeAttributeQuotes: true,
        },
        // chunksSortMode: 'dependency'
      })
    );
  });
  return outHtmls;
};

module.exports = {
  getEntrys,
  getOutHtmls,
};
```

```js
// webpack.config.js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CleanWebpackPlugin = require("clean-webpack-plugin");
const ManifestWebpackPlugin = require("webpack-manifest-plugin");
const ExtractTextPlugin = require("extract-text-webpack-plugin");
const webpack = require("webpack");
const multipe = require("./config/config.js");
const getEntrys = multipe.getEntrys;
const getOutHtmls = multipe.getOutHtmls;
//根据具体目录结构来确定路径
const entryPattern = "./src/pages/**.html";

module.exports = {
  entry: getEntrys(entryPattern),
  output: {
    filename: "[name]/js/[name].[hash].bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  resolve: {
    extensions: [".js", ".json"],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      config: path.resolve(__dirname, "./config"),
    },
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use: ["babel-loader"],
        exclude: "/node_modules",
      },
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: ["css-loader?importLoaders=1&&minimize", "postcss-loader"],
        }),
        exclude: "/node_modules",
      },
      // LESS
      {
        test: /\.less$/,
        use: ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: [
            "css-loader?importLoaders=1&&minimize",
            "postcss-loader",
            "less-loader",
          ],
        }),
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        loader: "file-loader",
        options: {
          limit: 10000,
          name: "[name]/img/[name].[hash].[ext]",
        },
        exclude: "/node_modules",
      },
    ],
  },
  plugins: [
    new ManifestWebpackPlugin(),
    new CleanWebpackPlugin(["dist"]),
    new webpack.optimize.CommonsChunkPlugin({
      name: "common", // 指定公共 bundle 的名称。
    }),
    new ExtractTextPlugin({
      filename: "[name]/css/[name].[contenthash].css",
      allChunks: true,
    }),
    ...getOutHtmls(entryPattern),
  ],
};
```

> [教程来源](https://blog.csdn.net/xyphf/article/details/79824777)

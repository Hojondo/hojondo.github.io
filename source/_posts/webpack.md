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





# webpack

是基于Node.js开发的前端项目构建工具

- 常见的静态资源

  .js .jsx .coffee .ts(typescript)

  .css .less .scss 

  .jpg .png .gif .bmp .svg

  .svg .ttf .eot .woff .woff2

  模版文件 .wja .jade .vue

- 网页中引入的静态资源多了后的问题

  网页加载速度慢，发起很多二次请求；

  要处理各个包之间的依赖关系，比如bootstrap 是依赖jquery的

- 如何解决上述问题

  合并压缩js css文件、

  对于图片来说 精灵图、base64、

  使用requireJS、webpack 解决各个包直接的依赖关系

## webpack和gulp区别

Gulp基于各个小task任务，webpack基于整个项目进行构建

## 安装

`npm i webpack -g`全局安装

`npm i jquery -S` 

```js
import $ from 'jquery'
$(function () {
  $('li:odd').css('backgroundColor', 'blue')
})
```

执行全局安装的话，命令行 `webpack ./src/main.js ./dist/bundle.js`



## 配置文件

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const webpack = require('webpack');

// 对于命令行直接输入webpack的时候，如果没有指定入口和出口，就会默认在根目录查找webpack.config.js文件
module.exports = {
    mode: 'development',
    entry: [ //entry为入口,webpack从这里开始编译
        "babel-polyfill",
        path.join(__dirname, './src/index.js')
    ],
    output: { //output为输出 path代表路径 filename代表文件名称
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, './dist'),
        chunkFilename: '[name].bundle.js'
    },
    module: { //module是配置所有模块要经过什么处理
        rules: [ //test:处理什么类型的文件,use:用什么,include:处理这里的,exclude:不处理这里的；// webpack 默认只能打包处理.js后缀名类型的文件;像.png.vue之类的无法主动处理，所以要配置第三方的loader
            {
                test: /\.js|jsx$/,
                use: ['babel-loader'],
                include: path.join(__dirname, 'src'),
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'], // 打包处理css样式表的第三方loader，从右向左依次处理；可在css-loader之后通过?追加参数 其中固定参数叫modules表示为普通的css样式表启动模块化
            },
            {
                test: /\.less$/, use: ['style-loader', 'css-loader', {
                    loader: 'less-loader',
                    options: { javascriptEnabled: true }
                }]
            },
            {
                test: /\.scss$/,
                use: ['style-loader', {
                    loader: 'css-loader',
                    options: {
                        modules: {
                            localIdentName: '[path][name]-[local]-[hash:5]'
                        },
                        sourceMap: true
                    }
                }, 'sass-loader']
            },
            {
                test: /\.(jpg|png|gif|bmp|jpeg|ico|svg|woff|woff2|eot|ttf|ttc|otf)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            limit: '10240',
                            name: 'assets/[name].[ext]'
                        }
                    }
                ]
            }
            // { test: /\.(woff|woff2|eot|ttf|ttc|otf)$/, use: 'file-loader' }
        ]
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            //   title: '测试',
            template: path.join(__dirname, './src/index.html')
        }),
        new CopyWebpackPlugin([
            {
                from: 'static',
                to: 'static'
            }
        ]),
        // new webpack.NamedModulesPlugin() // NamedModulesPlugin，以便更容易查看要修补(patch)的依赖
    ],
    resolve: {
        extensions: ['.js', '.jsx', 'json'], // 表示这几个文件的后缀名，可以省略不写，程序会从左到右以此查找对应后缀名的文件是否存在并对应
        alias: { // 别名
            '@': path.join(__dirname, './src'),
        }
    }
};
```


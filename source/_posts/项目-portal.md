---
title: 项目介绍 官网展示React
top: false
cover: false
toc: true
mathjax: false
date: 2020-03-22 00:13:32
tags:
password:
summary:
categories: ['项目经验']
---
# 官网 React版
> [线上预填充版 预览地址](/about/portal/Home)

## 业务简介
本项目 是使用react搭建的官网页面，特点在于多入口多出口打包，实现各页面在服务器端分隔，优化单页面应用导致的庞大js的问题。

## 技术栈
- `React`前端开发框架.
- `antd`UI库
- `scss`css预处理
- `gulp`vinyl-ftp自动部署静态资源到服务器
- `webpack`自搭建.webpack-devServer、webpack-merge、html-webpack-plugin、resolve.alias
- `babel` stage-3、ployfill、react

## 项目目录
```shell
    |-dist*
    |-build                     # webpack 配置文件
    |-public                    # 静态文件
    |-src
        |-main.js               # webpack 的入口文件；
        |-App.vue               # 根组件
        |-layout                # 整体布局
        |-api                   # 存放api请求(文件名与模型名称基本一致,文件名使用小驼峰, 方法名称与后端restful控制器一致)
        |-components            # 存放项目共用的组件,通常是一些可复用的组件会单独存放在该目录
        |-pages                 # 存放项目业务代码
            |-page1
                |-app.js        # 独立页面入口文件
                |-index.js      # 单页面jsx
                |-index.scss    # 样式
                |-component.js  # 局部组件
            |-...
        |-common                # 存放项目共用的资源，如：常用的图片、图标，样式，常量文件等等
            |-assets            # 存放项目共用的代码以外的资源，如：图片、图标、视频、字体 等
            |-styles            #
                |-index.scss    # 全局样式，出口文件
                |-mixin.scss    # 混合指令 定义可重复使用的样式（字体通用样式
                |-constant.scss # 存放scss的常量（主题相关通用颜色
                |-...
            |-...
    |-package.json              # npm包配置文件，里面定义了项目的npm脚本，依赖包等信息
    |-README.md                 # 项目说明文件
    |-.babelrc                  # babel 的配置文件
```

## 重点Mark
### Webpack多入口多出口 实现各页面独立index.html/js
```javascript
// webpack.base.js
const Creator_entries = (arr)=>{
    const a = {};
    arr.forEach(element => {
       
        a[element.name] = path.join(__dirname, `../src/pages/${element.name}/app.js`)
    });
    return a;
}
const Creator_HtmlWebpackPlugin =(arr)=>{
    const a = [];
    arr.forEach(title => {
        a.push(new HtmlWebpackPlugin({
            title:title.title,
            filename: `${title.name}/index.html`,
               chunks:[title.name],
            template: path.join(__dirname, '../public/index.html')
        }))
    });
    return a;
}
module.exports = {
    entry: Creator_entries(outArr),
    output: {
        filename: '[name]/[name].bundle.js',
        path: path.resolve(__dirname, '../dist'),
        chunkFilename: '[name].bundle.js',// 关键
    },
    plugins: [
        ...Creator_HtmlWebpackPlugin(outArr)
    ]
}
```
---
title: nodeJS-常用轮子整理
top: false
cover: false
toc: true
mathjax: false
date: 2019-12-25 19:16:31
tags:
password:
summary:
categories:
---
# 示例
```json
"autoprefixer": "^7.1.2",
"babel-core": "^6.22.1",
"babel-eslint": "^8.2.1",
"babel-helper-vue-jsx-merge-props": "^2.0.3",
"babel-jest": "^21.0.2",
"babel-loader": "^7.1.1",
"babel-plugin-dynamic-import-node": "^1.2.0",
"babel-plugin-syntax-jsx": "^6.18.0",
"babel-plugin-transform-es2015-modules-commonjs": "^6.26.0",
"babel-plugin-transform-runtime": "^6.22.0",
"babel-plugin-transform-vue-jsx": "^3.5.0",
"babel-preset-env": "^1.3.2",
"babel-preset-stage-2": "^6.22.0",
"babel-register": "^6.22.0",
"chalk": "^2.0.1",
"chromedriver": "^2.27.2",
"copy-webpack-plugin": "^4.0.1",
"cross-spawn": "^5.0.1",
"css-loader": "^0.28.0",
"eslint": "^4.15.0",
"eslint-config-standard": "^10.2.1",
"eslint-friendly-formatter": "^3.0.0",
"eslint-loader": "^1.7.1",
"eslint-plugin-import": "^2.7.0",
"eslint-plugin-node": "^5.2.0",
"eslint-plugin-promise": "^3.4.0",
"eslint-plugin-standard": "^3.0.1",
"eslint-plugin-vue": "^4.0.0",
"extract-text-webpack-plugin": "^3.0.0",
"file-loader": "^1.1.4",
"friendly-errors-webpack-plugin": "^1.6.1",
"html-webpack-plugin": "^2.30.1",
"jest": "^22.0.4",
"jest-serializer-vue": "^0.3.0",
"nightwatch": "^0.9.12",
"node-notifier": "^5.1.2",
"optimize-css-assets-webpack-plugin": "^3.2.0",
"ora": "^1.2.0",
"portfinder": "^1.0.13",
"postcss-import": "^11.0.0",
"postcss-loader": "^2.0.8",
"postcss-url": "^7.2.1",
"rimraf": "^2.6.0",
"selenium-server": "^3.0.1",
"semver": "^5.3.0",
"shelljs": "^0.7.6",
"uglifyjs-webpack-plugin": "^1.1.1",
"url-loader": "^0.5.8",
"vue-jest": "^1.0.2",
"vue-loader": "^13.3.0",
"vue-style-loader": "^3.0.1",
"vue-template-compiler": "^2.5.2",
"webpack": "^3.6.0",
"webpack-bundle-analyzer": "^2.9.0",
"webpack-dev-server": "^2.9.1",
"webpack-merge": "^4.1.0"
```
# 轮子介绍

## babel 集合

- **babel-core**
- **babel-polyfill**
- **babel-helper-vue-jsx-merge-props**
- **babel-loader**
- **babel-plugin-import**
- **babel-plugin-transform-remove-strict-mode**
- **babel-plugin-dynamic-import-node**
- **babel-plugin-syntax-jsx**
- **babel-plugin-transform-es2015-modules-commonjs**
- **babel-plugin-transform-runtime**
- **babel-plugin-transform-vue-jsx**
- **babel-preset-env**
- **babel-preset-stage-0**
- **babel-register**

## webpack 集合

- **webpack**
- **webpack-bundle-analyzer**
- **webpack-dev-server**
- **webpack-merge**
- **clean-webpack-plugin**
- **copy-webpack-plugin**
- **uglifyjs-webpack-plugin**
- **optimize-css-assets-webpack**
- **extract-text-webpack-plugin**
- **friendly-errors-webpack-plug**
- **html-webpack-plugin**
- **eslint-loader**
- **style-loader**
- **css-loader**
- **file-loader**
- **url-loader**

## ESlint 集合

- **eslint**
- **babel-eslint**
- **eslint-config-standard**
- **eslint-config-google**
- **eslint-config-airbnb**
- **eslint-friendly-formatter**
- **eslint-plugin-import**
- **eslint-plugin-node**
- **eslint-plugin-promise**
- **eslint-plugin-standard**
- **eslint-plugin-vue**
- **eslint-plugin-react**
- **eslint-plugin-react-hooks**
- **eslint-plugin-jsx-a11y**

## 测试框架 Jest、selenium 集合

- **jest**
- **babel-jest**
- **jest-serializer-vue**
- **vue-jest**
- **selenium-server** ：自动化测试框架[selenium](https://zhuanlan.zhihu.com/p/52646463)的Node容器
- **semver** ：是[ 语义化版本（Semantic Versioning）规范](http://semver.org/lang/zh-CN/) 的一个实现，实现了版本和版本范围的解析、计算、比较。[介绍](https://cnodejs.org/topic/570f8331510629637266685a)
- **chromedriver** ：是 google 为网站开发人员提供的自动化测试接口，它是 **selenium2** 和 **chrome浏览器** 进行通信的桥梁，[详细介绍](https://www.jianshu.com/p/31c8c9de8fcd)
- **nightwatch** ：端到端的测试工具，内置断言库，基于Selenium，[非官方简介](https://zhuanlan.zhihu.com/p/48361267)

## Postcss、less、scss、stylus集合

- **postcss-import** ：postcss是后处理器框架，[整体介绍](https://juejin.im/post/5c022f4a6fb9a049ca371684)
- **postcss-url**
- **postcss-loader**
- **autoprefixer** ：后处理器postcss框架其中一个插件，适用场景是：添加前缀、转换单位、低版本浏览器的hack等。[详细介绍](http://www.html-js.com/article/Postcss-postcss-pre-processor-and-post-processor) [详细教程](https://juejin.im/post/5c17bf93f265da611510b5ea)
- **less**
- **less-loader**
- **node-sass**
- **sass-loader**
- **stylus-loader**

## vue必备

- **vue-loader**
- **vue-style-loader**
- **vue-template-compiler** ：webpack生态下使用*`*vue-loader*`*进行vue解析，*`*vue-loader*`*自带*`*vue-template-compiler*`*，会预编译您的template模版，生成javascript可执行函数。[源码详解](https://blog.csdn.net/u014787301/article/details/85842963)
- **vue**
- **vue-router**
- **vuex**
- **axios**

### vue环境下的简单组件库

- **iview** && **iview-loader**
- **vue-nav-tabs**

## React必备

## 工具组件库

- **mavon-editor**
- **highlight.js**
- **mockjs**
- **clipboard**
- **echarts**
- **Lodash**
- **qs**
- **fs**
- **fs-extra** ：实现了一些fs模块不包含的文件操（比如递归复制、删除等等）的模块
- **url-search-params-polyfill**
- **md5** ：用MD5哈希加密string
- **gulp** ：一个自动化构建工具，一般配合webpack实现webpack未提供的图片压缩、项目上线部署等功能
- **vinyl-ftp** ：配合gulp实现自动ftp部署项目文件到服务器

## 杂项

- **chalk** ：实现terminal控制台彩色文字输出的模块
- **cross-spawn** ：使用npm命令的跨平台解决方案
- **node-notifier** ：基于NodeJs发送跨平台原生系统通知，Electron已内置，可以测试时用于提醒
- **ora** ：优雅好玩的terminal控制台spinner加载图标 模块
- **portfinder** ：自动获取端口，一般用于webpack.dev.config.js内
- **rimraf** ：以包的形式包装`rm -rf`命令，就是用来删除文件和文件夹的，不管文件夹是否为空，都可以删除，[简介](https://segmentfault.com/a/1190000011831802)
- **shelljs** ：Node.js下的脚本语言解析器，模块重新包装了`child_process`，实现在javascript中编写shell命令实现功能，[介绍](https://www.kancloud.cn/outsider/clitool/313191)、[介绍2](https://juejin.im/post/5cdb76166fb9a032196ef1ff)
- **commander** ：封装了命令行参数的接口，让commander能够根据用户输入参数选择执行不同代码。。一般用于脚手架开发[例子用commander做一个自己的eslint脚手架](https://cloud.tencent.com/developer/article/1541086)
- **validate-npm-package-name** ：对于用户输入的工程名的可用性进行验证的模块，[例子](https://www.geekjc.com/post/5de4eb6269116702fb6f61e3)







> 专有名词注解：
>
> 三个打包器 [对比](https://stackshare.io/stackups/gulp-vs-parcel-vs-webpack)：
>
> webpack、gulp、[Parcel](https://www.html.cn/doc/parcel/getting_started.html) 
>
> 还有更多对比 [NPM vs. Bower vs. Browserify vs. Gulp vs. Grunt vs. Webpack - Stack Overflow](https://stackoverflow.com/questions/35062852/npm-vs-bower-vs-browserify-vs-gulp-vs-grunt-vs-webpack)
---
title: Webpack
top: true
cover: false
toc: true
mathjax: false
date: 2019-09-20 11:36:11
tags: ["打包"]
password:
summary:
categories: ["笔记"]
---

# webpack

是基于 Node.js 开发的前端项目构建工具

- 常见的静态资源

  .js .jsx .coffee .ts(typescript)

  .css .less .scss

  .jpg .png .gif .bmp .svg

  .svg .ttf .eot .woff .woff2

  模版文件 .wja .jade .vue

- 网页中引入的静态资源多了后的问题

  网页加载速度慢，发起很多二次请求；

  要处理各个包之间的依赖关系，比如 bootstrap 是依赖 jquery 的

- 如何解决上述问题

  合并压缩 js css 文件、

  对于图片来说 精灵图、base64、

  使用 requireJS、webpack 解决各个包直接的依赖关系

## webpack 和 gulp 区别

Gulp 基于各个小 task 任务，webpack 基于整个项目进行构建

## 安装

- 全局安装`npm i webpack -g`

  _不推荐全局安装 webpack，这会导致命令行运行 webpack 的时候锁定版本_

  示例安装 jq：`npm i jquery -S`

  ```js
  import $ from "jquery";
  $(function () {
    $("li:odd").css("backgroundColor", "blue");
  });
  ```

  执行全局安装的话，可在 terminal 写命令行 `webpack ./src/入口文件.js ./dist/bundle.js`

- 局部安装

  `npm install webpack webpack-cli -D`

  _webpack 和 webpack-cli 曾经是在一起的，在 4.x 版本中进行了拆分，所以必须同时安装这两个_

## 配置文件结构`module.exports`

在最新的 [官方文档](https://webpack.js.org/configuration/) 中，webpack4 有两个新的配置项，`mode` 和 `optimization`

[教程来源](https://juejin.im/post/5b53f977518825620f57dd4c)

### `mode`

默认值 production。

4.x 特点是约定大于配置，mode 为对应值的时候，其他字段会有对应的默认设置。

- mode: production 或 development 时，optimization 都会开启的优化

  ```json
  {
    "mode": "production" || "development",
    "optimization": {
      "removeAvailableModules": true,
      "removeEmptyChunks": true,
      "mergeDuplicateChunks": true,
      "nodeEnv": "production" || "development"
    }
  }
  ```

- `mode: production`时，`optimization`开启的优化

  ```js
  {
    "mode": "production",
    "devtool": false,
    "cache": false,
    "output": {
      "pathinfo": false
    },
    "optimization": {
      "flagIncludedChunks": true,
      "occurrenceOrder": true,
      "usedExports": true,
      "sideEffects": true,
      "concatenateModules": true,
      "minimize": true,
      "removeAvailableModules": false, // 会降低webpack性能，下个大版本中会默认false
      "mangleExports": true,
      "innerGraph": true,
    },
    "performance": {
      "hints": "error"
      // ...
    }
  }
  ```

- `mode: development`时，`optimization`开启的优化

  ```json
  {
    "mode": "development",
    "devtool": "eval",
    "cache": true,
    "module": {
      "unsafeCache": true
    },
    "output": {
      "pathinfo": true
    },
    "optimization": {
      "providedExports": true,
      "splitChunks": true,
      "runtimeChunk": true,
      "noEmitOnErrors": true,
      "namedModules": true,
      "namedChunks": true
    }
  }
  ```

### `optimization`

_从 webpack 4 开始，会根据你选择的 [`mode`](https://webpack.docschina.org/concepts/mode/) 来执行不同的优化，不过所有的[优化](https://webpack.docschina.org/configuration/optimization/)还是可以手动配置和重写。_

- `minimize` ：告诉 **webpack** 使用 **TerserPlugin** 最小化打包文件，对打包文件进行 **压缩**、**混淆**。 **production 模式** 下， 默认为 **true**
- `minimizer` ：允许你通过提供一个或多个定制过的 [TerserPlugin](https://webpack.docschina.org/plugins/terser-webpack-plugin/) 实例，覆盖默认压缩工具(minimizer)。
- `splitChunks` ：找到 chunk 中共享的模块,取出来生成单独的 chunk。对于动态导入模块，默认使用 webpack v4+ 提供的全新的通用分块策略
- `runtimeChunk` ：会为每个仅含有 runtime 的入口起点添加一个额外 chunk
- `noEmitOnErrors` ：在编译出错时，使用来跳过生成阶段，可以确保没有生成出错误资源不写入到输出
- `namedModules` ：告知 webpack 使用可读取模块标识符来帮助更好地调试
- `namedChunks` ：告知 webpack 使用可读取 chunk 标识符来帮助更好地调试
- `moduleIds` ： 给模块有意义的名称代替 ids
- `chunkIds` ：给 chunk 有意义的名称代替 ids
- `nodeEnv` ：告知 webpack 将 process.env.NODE_ENV 设置为一个给定字符串
- `mangleWasmImports` ：在设置为 true 时，告知 webpack 通过将导入修改为更短的字符串，来减少 WASM 大小。这会破坏模块和导出名称。
- `removeAvailableModules` ：如果模块已经包含在所有父级模块中，告知 webpack 从 chunk 中检测出这些模块，或移除这些模块
- `removeEmptyChunks` ：如果 chunk 为空，告知 webpack 检测或移除这些 chunk
- `mergeDuplicateChunks` ：告知 webpack 合并含有相同模块的 chunk
- `flagIncludedChunks` ：告知 webpack 确定和标记出作为其他 chunk 子集的那些 chunk，其方式是在已经加载过较大的 chunk 之后，就不再去加载这些 chunk 子集
- `occurrenceOrder` ：排序最终打包结果中各个模块
- `providedExports` ：在可能的情况下确定每个模块的导出,被用于其他优化或代码生成，`export * from ...`时指定是否更高效的生成 export 代码
- `usedExports` ：依赖字段 providedExports，告知 webpack 是否对每个模块都查找引用的 exports
- `concatenateModules` ：依赖字段 providedExports 和字段 usedExports，是否查找模块图表中可以安全连接到单个模块中的片段
- `sideEffects` ：是否 识别模块的 package.json 中 sideEffects 字段 或 rules 来跳过该模块，用于移除 JavaScript 上下文 中的 未引用代码(dead-code) - tree shaking
- `portableRecords` ：告诉 webpack 生成 具有相对路径的记录，以便能够 移动上下文文件夹
- `mangleExports` ：控制 export 压缩
- `innerGraph` ：是否处理未使用的 exports 的内部模块图

### `devtool`

该配置项用以控制是 **否生成 source map** 以及 **怎样生成 source map**，方便调试定位源码

`eval`|`source-map`|`cheap`|`module`|`inline`|`hidden`|`nosources`

### `context`

_webpack 处理打包文件的时候的初始目录_

基础目录，**绝对路径**，用于从配置中解析 **入口起点(entry point)** 和 **loader**。

`context: path.resolve(__dirname, '../')`

### `entry`

_入口文件，webapck 4.x 默认的就是 src/index.js4.x 约定大于配置：其中 入口是`src->index.js`打包输出目录是`dist->main.js`_

> 1. 如果 **entry** 配置项的值的类型为 **string**， 对应 **单页面单入口应用**。在编译过程中， 生成 **一个模块依赖关系图**
> 2. 如果 **entry** 配置项的值的类型为 **[string]**, 对应 **单页面多入口应用**。应用程序启动时，数组中的入口文件按序执行。在编译过程中，生成 **一个模块依赖关系图**。
> 3. 如果 **entry** 配置项的值为 **object**， 对应 **多页面(单入口/多入口)应用**。在编译过程中，生成 **多个模块依赖关系图**。
> 4. 如果 **entry** 配置项的值的类型为 **function**，则是 **动态入口**

### `output`

- `path` ：output 目录对应一个**绝对路径**，指定 打包文件 的 输出目录，即打包后 文件在硬盘中的存储位置。

- `filename` ：此选项决定了每个输出 bundle 的名称。这些 bundle 将写入到 [`output.path`](https://webpack.docschina.org/configuration/output/#output-path) 选项指定的目录下

  > 我们可以使用以下常用方式为每一个 **打包文件** 提供 **唯一** 的 **文件名**：
  >
  > - **'bundle.js'** - 指定 **入口文件(main.js)** 所在的 **chunk** 的 **文件名**， **仅适用于单页面应用**；
  > - **'[name].js'** - 使用 **chunk** 的 **name**；
  > - **'[name].[hash].js'** - 使用 **compilation** 的 **hash**，**所有 chunk 文件名中 hash 值一样**；
  > - **'[name].[chunkhash].js'** - 使用 **chunk** 内容的 **hash**，**每个 chunk 文件名中的 hash 值都不一样**；
  > - **'[name].[id].js'** - 使用 **chunk** 的 **id**；
  > - **[name].[contenthash].js** - 使用提取内容的 **hash**？？
  >
  > 我们也可以将 **filename** 指定一个 **function** 来返回输出的文件名， **返回值** 的格式和上面一样。
  >
  > 常见命名格式： `[name].[chunkhash].js`

- `publicPath` ：对于按需加载(on-demand-load)或加载外部资源(external resources)（如图片、文件等）来说，会为所有的资源指定一个基础路径。**设置了 publicPath 后，会为所有资源引用添加此前缀**，publicPath 并不会对 生成文件的路径 造成影响，主要是 对页面里面引入的资源的路径做对应的补全

- `chunkFilename` ：指定 输出的打包文件 的 文件名

- `pathinfo` ：告诉 webpack 在 chunk 中引入 所包含模块信息的相关注释，不应该用于生产环境

- `chunkLoadTimeout` ：chunk 请求超时的时间，单位为 毫秒，默认为 120000。

- `crossOriginLoading` ：告诉 webpack 是否启用 chunk 的 跨域加载， 默认值为 false。

- `jsonpScriptType` ：在 动态添加 script 元素 的方式来进行 懒加载时，指定 script 元素 的类型，即 设置 type 属性的值。

- `devtoolFallbackModuleFilenameTemplate` ：

- `devtoolModuleFilenameTemplate` ：

- `devtoolNamespace` ：

- `assetModuleFilename` ：

- `libraryExport` ：用于指定 具体的入口文件的返回值

- `library` ：常用于 开发类库， 需和 libraryTarget 属性 配合使用

- `libraryTarget` ：配置如何暴露 library

- `globalObject` ：和 library、libraryTarget 一起使用，指示将 入口文件的返回值 分配给哪个 全局对象，默认值为 window

- `auxiliaryComment` ：在和 output.library 和 output.libraryTarget 一起使用时，此选项允许用户向 导出容器(export wrapper) 中插入 注释

- `hashDigest` ：

- `hashDigestLength` ：

- `hashFunction` ：

- `hashSalt` ：

- `hotUpdateChunkFilename` ：

- `hotUpdateFunction` ：

- `hotUpdateMainFilename` ：

- `jsonpFunction` ：当 输出多个 chunk 时， webpack 会构建一个 全局方法， 用于 安装 chunk。jsonpFunction 可用于指定 安装 chunk 的 方法名。

- `sourceMapFilename` ：配置 source map 的命名方式。默认使用 '[file].map'。

- `sourcePrefix` ：

- `strictModuleExceptionHandling` ：如果一个模块在导入时抛出异常，告诉 webpack 从 模块实例缓存 中 删除 这个模块

- `umdNamedDefine` ：会对 UMD 的构建过程中的 AMD 模块进行命名。否则就使用 匿名 的 define

- `futureEmitAssets` ：告诉 webpack 在 输出文件到指定位置 时，使用 未来的版本， 它允许输出以后 释放内存

- `ecmaVersion` ：

- `compareBeforeEmit` ：

- `iife` ：

- `module` ：

### `module`

用于处理项目中的 **不同类型(.js、.css、.sass、.vue、.png ...)** 的模块规则

- `noParse`

  在构建 **依赖关系图** 时，**webpack** 会读取 **模块源文件** 的内容，然后 **将源文件内容解析成一颗 AST 树**。 分析 **AST 树**，获取模块的 **依赖** 的 **静态模块**、**懒加载模块**、**全局变量**。如果 **模块的 url(绝对路径)** 匹配 **noParse 配置项指定的表达式**， 那么 **模块对应的源文件的代码结构** 不会被解析。 不解析大型的 library 可以 **提高构建性能**

- `rules`

  ```js
  rules: [
    {
      test: /\.scss$/,
      use: [
        "style-loader",
        {
          loader: "css-loader",
          options: {
            modules: {
              localIdentName: "[path][name]-[local]-[hash:5]",
            },
            sourceMap: true,
          },
        },
        "sass-loader",
      ],
    },
  ];
  ```

### `plugins`

[]

### `resolve`

设置 **源文件**、**npm 包** 如何被 **解析**。

> webpack 在 **构建依赖关系图** 时会通过 **resolve** 配置项 **解析模块的 url** 来获取 **模块源文件的绝对地址**。 通过 **源文件的绝对地址**，读取源文件的内容，然后使用 **解析器(parser)** **解析源文件的代码结构**,获取 **依赖模块**。
>
> 此外，**webpack** 使用 **loader** 处理 **源文件** 时，也会通过 **resolve** 配置项 **解析 loader 包 的 入口文件 的绝对地址**，然后使用 **入口文件** 提供的方法处理 **源文件** 内容。

### `resolveLoader`

用法和 resolve 配置项完全一致，区别在于 它仅用于解析 webpack 的 loader 包

### `devServer`

_使用了 webpack-dev-server 之后就需要有的配置，在这里可以配置详细的开发环境_

- `contentBase` ：决定了 webpackDevServer 启动时服务器资源的根目录。只有在你想要提供**非通过 webpack 打包的静态文件**时才需要。[`devServer.publicPath`](https://webpack.docschina.org/configuration/dev-server/#devserver-publicpath-) 将用于确定应该从哪里提供 bundle，并且此选项优先。推荐使用一个绝对路径,例:`path.join(__dirname, 'public')`，默认 webpack dev server 是从项目的根目录提供服务，如果要从不同的目录提供服务，可以通过 contentBase 来配置。类似`output.path`一样的作用。假定服务器里生成的内存 index.html 所在位置，并且优先级是最高的
- `publicPath` ：此路径下的打包文件可在浏览器中访问。即在浏览器中访问的路径的前缀（包括 index.html 的访问路径），若是 devServer 里面的 publicPath 没有设置，则会认为是 output 里面设置的 publicPath 的值，只影响于 webpackDevServer，其他该怎么 build 就怎么 build
- `clientLogLevel` ：当使用*内联模式(inline mode)*时，会在开发工具(DevTools)的控制台(console)显示消息
- `historyApiFallback` ：启用后任意的 `404` 响应都可能需要被替代为 `index.html`
- `hot` ：启用 webpack 的 [模块热替换](https://webpack.docschina.org/concepts/hot-module-replacement/) 功能
- `compress` ：一切服务都启用 [gzip 压缩](https://betterexplained.com/articles/how-to-optimize-your-site-with-gzip-compression/)
- `host` ：指定使用一个 host。默认是 `localhost`。如果希望服务器外部可访问指定为`0.0.0.0`
- `port` ：自指定一个端口
- `open` ：自动打开浏览器
- `overlay` ：当出现编译器错误或警告时，在浏览器中显示全屏覆盖层，默认禁用
- `proxy` ：如果你有单独的后端开发服务器 API，并且希望在同域名下发送 API 请求 ，那么代理某些 URL 会很有用
- `quiet` ：启用后，除了初始启动信息之外的任何内容都不会被打印到控制台
- `watchOptions` ：与监视文件相关的控制选项
- ...

### `target`

webpack 能够为 **多种环境** 编译构建， 可通过 **target 配置项** 指定一个 **具体的环境**
字符串可选值

- `web` ：应用于 web 环境(浏览器)。对于需要 懒加载的 chunk，会通过 动态添加 script 元素的方式加载，然后通过一个 全局方法(webpackJsonp) 来安装
- `webworker` ：应用于 web worker 环境。使用 chunk 时，通过 importScripts 方法加载。
- `node` ：应用于 node 环境。 非入口 chunk 的代码符合 CMD 规范。使用 chunk 时，通过 require 方法加载
- `async-node` ：应用于 node 环境。 非入口 chunk 的代码符合 CMD 规范。使用 chunk 时，通过 fs 模块异步加载
- `node-webkit` ：node-webkit 是一个基于 Chromium 和 Node.js 的 Web 运行环境，可让你直接在 DOM 中调用 Node.js 模块，并可使用任何现有的 Web 技术来编写本地应用
- `electron-main` ：应用于 electron 主进程
- `elctron-renderer` ：应用于 electron render 进程
- `electron-preload` ：应用于 electron render 进程

### `externals`

在某些情况下，我们是 **不希望将依赖的类库** 打包到最后输出的 **bundle** 中的。此时， 我们通过 **externals 配置项指定不参与编译打包的类库**。

### `performance`

用于配置如何展示性能提示。例如，如果一个资源超过 250kb，webpack 会对此输出一个警告来通知你。

- `hits` ：false， 关闭提示；'warning',当输出文件的体积超过指定体积时，输出 警告信息， 依然完成编译打包工作, 适用于 开发模式；'error'，当输出文件的体积超过指定体积时，中断编译打包，并输出 错误信息， 适用于生产模式。
- `maxEntrypointSize` ：指定 入口文件 的 最大体积， 默认值为 250000 bytes，超出警告
- `maxAssetSize` ：指定 单个资源(包括入口文件) 的 最大体积，默认值为 250000 bytes，超出警告
- `assetFilter` ：自定义 哪些文件的 体积超过最大体积 时，输出 警告/错误 信息

### `node`

这些选项可以配置是否 polyfill 或 mock 某些 [Node.js 全局变量](https://nodejs.org/docs/latest/api/globals.html)和模块。这可以使最初为 Node.js 环境编写的代码，在其他环境（如浏览器）中运行

### `stats`

统计信息

---

### `name`

当前配置的名称，一般用于加载多个配置文件时

### `cache`

缓存生成的 webpack 模块和 chunk，来改善构建速度。缓存默认在观察模式(watch mode)启用

### `loader`

在 loader 上下文中暴露自定义值

### `profile`

捕获一个应用程序"配置文件"，包括统计和提示，然后可以使用 [Analyze](https://webpack.github.io/analyse/) 分析工具进行详细分析

### `parallelism`

限制并行处理模块的最大数量，可用于微调性能获得更优性能

### `amd`

设置 `require.amd` 或 `define.amd` 的值，某些流行的模块是按照 AMD 规范编写的，为 false 时将禁用 webpack 对 AMD 的支持

### `bail`

在第一个错误出现时抛出失败结果，而不是容忍它。将迫使 webpack 退出其打包过程

### `recordsPath`

开启这个选项可以生成一个 JSON 文件，其中含有 webpack 的 "records" 记录 - 即「用于存储跨多次构建(across multiple builds)的模块标识符」的数据片段。可以使用此文件来跟踪在每次构建之间的模块变化

### `recordsInputPath`

指定读取最后一条记录的文件的名称

### `recordsOutputPath`

指定记录要写入的位置

## webpack-dev-server

1. `webpack-dev-server`的常用命令参数 （推荐）

   - `--open` 自动打开浏览器
   - `--port nnnn` 手动指定端口号
   - `--contentBase src` 指定根目录是 src
   - `--hot` 热更新，局部更新，不是刷新页面没有重新生成新的 bundle.js，而是加了两个`xxx.hot-update.js/json`文件

2. 通过配置文件内添加`devServer`字段来指定

   ```js
   module.exports = {
     devServer: {
           open: true, // 自动打开浏览器
           port: 3000, // 设置启动时候的监听端口，默认是 8080
           contentBase: 'src', // 指定托管根目录
           inline: false, // development模式 下，webpack-dev-server 会 监听源文件是否发生变化。 如果发生变化，会 自动刷新页面。
           hot: true, // 启动热更新 的第一步，同时满足以下条件：启用inline模式；显示声明module.hot.accept('url',callback)否则只能刷新页面
           hotOnly: false, // 默认情况为 false，当 HMR 失败以后，浏览器端通过 重新加载页面 来 响应服务端更新
           useLocalIp: true, // 设置为true，dev server 会以 本地IP 打开默认浏览器
           openPage: '默认打开的跳转页面', // 指定 dev-server 自动打开 默认浏览器 以后的 默认跳转页面
           host: '', // 指定要使用的 host， 默认是 localhost
           liveReload: , // 默认情况下，dev-server 将在 检测到参与编译打包的文件文件更改 时 重新加载/刷新页面。  即如果如果一个文件 未参与编译打包过程， 那么它发生变化时 不会触发重新加载/刷新页面。通过将 liveReload 设置为 false 来 禁用 它(修改什么文件也 不会触发重新加载/刷新页面)
           watchContentBase: , // 告诉服务器从哪里提供内容，默认值为 当前工作目录
           watchOptions: , // 告诉 dev-server 监视 devServer.contentBase选项 所服务的文件， 默认情况下 禁用 它
           writeToDisk: , // 默认为 false， 即 不将编译打包以后的内容写入磁盘
           lazy: false, // 如果设置为 true， dev-server会进入 懒惰模式， 仅在请求时编译该bundle。 这意味着 webpack不会监视任何文件更改。
           filename: '', // 此选项可让您减少 lazy模式 下的 编译。 默认情况下，在 lazy模式 下，每个请求都会产生 新的编译。 使用 filename，只能在请求某个文件时进行编译
           overlay: {}, // 当存在 编译错误或者警告 时，将 错误或者警告 以 全屏覆盖 的形式在浏览器中展示
           proxy: {
               'test': {
                   target: 'https://localhost:3000',
                   changeOrigin: true,
                   pathRewrite: {},
                   router: {...}
               }
           }, // 在 前后端分离 的开发过程中，使用 proxy 可以有效的帮我们解决 跨域请求问题。通常，会在 proxy配置项 中指定每个 API请求 对应的 代理。
           progress: false, // 在 控制台 输出 进度信息，仅适用于 CLI模式
           allowedHosts: [] // 将允许访问 dev server 的 服务列入白名单。

     },
     plugins: [
       new webpack.HotModuleReplacementPlugin(), // 启动热更新的第二步
     ]
   }
   ```

## 几个常用插件介绍

### `html-webpack-plugin`

[详解文章](https://www.cnblogs.com/wonyun/p/6030090.html)

简化了 HTML 文件的创建，以便为你的 webpack 包提供服务。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。 你可以让插件为你生成一个 HTML 文件，使用[lodash 模板](https://lodash.com/docs#template)提供你自己的模板，或使用你自己的[loader](https://webpack.docschina.org/loaders)。

两个作用：

1. 根据指定页面自动在内存中生成对应页面。可以生成创建 html 入口文件，比如单页面可以生成一个 html 文件入口，配置**N**个`html-webpack-plugin`可以生成**N**个页面入口
2. 自动创建合适的 script 标签，引入对应路径的 bundle.js。为 html 文件中引入的外部资源如`script`、`link`动态添加每次 compile 后的 hash，防止引用缓存的外部文件问题

```js
new HtmlWebpackPlugin({
  template: path.join(__dirname, './src/index.html'), // 指定模版页面，根据指定页面路径生成内存中的页面
  filename: 'index.html', // 最终生成的内存页面的文件名，默认为index.html，不配置就是该文件名；此外，还可以为输出文件指定目录位置（例如'html/index.html'）
  hash: false, // true|false，是否为所有注入的静态资源添加webpack每次编译产生的唯一hash值
  inject: true, // 向template或者templateContent中注入所有静态资源，不同的配置值注入的位置不尽相同。
  compile: true,
  favicon: false, // 添加特定favicon路径到输出的html文档中，同title配置项，需要在模板中动态获取其路径值
  minify: false,
  cache: true,
  showErrors: true, // true|false，默认true；是否将错误信息输出到html页面中
  chunks: 'all', // 允许插入到模板中的一些chunk，不配置此项默认会将entry中所有的chunk注入到模板中。在配置多个页面时，每个页面注入的chunk应该是不相同的，需要通过该配置为不同页面注入不同的thunk
  excludeChunks: [], // 这个与chunks配置项正好相反，用来配置不允许注入的thunk
  title: 'Webpack App', // 生成的html文档的标题，存在模版文件的情况下需要在html中放置<title><%= htmlWebpackPlugin.options.title %></title>
  xhtml: false // true|fasle, 默认false；是否渲染link为自闭合的标签，true则为自闭合标签
}),
```

### `clean-webpack-plugin`

在每次构建前清理 `/dist` 文件夹，这样只会生成用到的文件

### `copy-webpack-plugin`

将静态资源单个文件或整个目录复制到构建目录

## 几个常用 loader

Webpack 默认只能打包处理 js 类型的文件，无法处理其他的非 js 类型文件，需要手动安装一些合适的第三方 loader 加载器

> **分析调用第三方 loader 的过程**
>
> 1. 发现要处理的文件不是 js 文件，就会去配置文件中，查找有没有对应的第三方 loader 规则
> 2. 如果能找到对应的规则，就会调用对应的 loader 处理这种文件类型
> 3. 在调用 loader 的时候，是从后往前依次调用的
> 4. 当最后一个 loader 调用完毕，会把处理的结果直接交给 webpack 进行打包合并，最终输出到 bundle.js 中去

### `url-loader`

不管是图片还是自体库，只要是 url 地址 都用这个处理，内部依赖了`file-loader`

_limit 参数是指 maxSizeToBeBase64 单位是 KB_

```js
{
  	test: /\.(jpg|png|gif|bmp|jpeg)$/,
    use: 'url-loader?limit=6000&name=[hash:8]-[name].[ext]'
}
```

### `sass-loader`

### `less-loader`

### `babel-loader`

在 webpack 中 运行如下两套命令，安装两套包，去安装 babel 相关的 loader 功能

`npm i babel-core babel-loader babel-plugin-transform-runtime -D`

`npm i babel-preset-env babel-preset-stage-0 -D`

在 webpack 配置中加入 rules

新建`.babelrc`文件，写 babel 配置，必须符合 JSON 语法规范

```js
{
   test: /\.js|jsx$/, // test:处理什么类型的文件
   use: ['babel-loader'], // use:用什么
   include: path.join(__dirname, 'src'), // include:处理这里的
   exclude: /node_modules/ // exclude:不处理这里的；
},
```

_如果不排除 node_modules 中的所有第三方 js 文件，都打包编译的话 最终 babel 把所有 node_modules 中的 js 转换完毕了耶无法正常运行项目_

## 示例

```js
const path = require("path"); // 由于webpack是基于node进行构建的，所以 webpack配置文件中，任何合法的node代码都是支持的
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const CopyWebpackPlugin = require("copy-webpack-plugin");
const webpack = require("webpack");

// 对于命令行直接输入webpack的时候，如果没有指定入口和出口，就会默认在根目录查找webpack.config.js文件
module.exports = {
  mode: "development",
  entry: [
    //entry为入口,webpack从这里开始编译
    "babel-polyfill",
    path.join(__dirname, "./src/index.js"),
  ],
  output: {
    //output为输出 path代表路径 filename代表文件名称
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "./dist"),
    chunkFilename: "[name].bundle.js",
  },
  devServer: {
    open: true, // 自动打开浏览器
    port: 3000, // 设置启动时候的运行端口
    contentBase: "src", // 指定托管根目录
    hot: true, // 启动热更新 的第一步
  },
  module: {
    //module是配置所有模块要经过什么处理，用于配置所有第三方loader的规则，webpack 默认只能打包处理.js后缀名类型的文件;像.png.vue之类的无法主动处理，所以要配置第三方的loader
    rules: [
      {
        test: /\.js|jsx$/, // test:处理什么类型的文件
        use: ["babel-loader"], // use:用什么
        include: path.join(__dirname, "src"), // include:处理这里的
        exclude: /node_modules/, // exclude:不处理这里的；
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"], // 打包处理css样式表的第三方loader，从右向左依次处理；可在css-loader之后通过?追加参数 其中固定参数叫modules表示为普通的css样式表启动模块化
      },
      {
        test: /\.less$/,
        use: [
          "style-loader",
          "css-loader",
          {
            loader: "less-loader",
            options: { javascriptEnabled: true },
          },
        ],
      },
      {
        test: /\.scss$/,
        use: [
          "style-loader",
          {
            loader: "css-loader",
            options: {
              modules: {
                localIdentName: "[path][name]-[local]-[hash:5]",
              },
              sourceMap: true,
            },
          },
          "sass-loader",
        ],
      },
      {
        test: /\.(jpg|png|gif|bmp|jpeg|ico|svg)$/,
        use: [
          {
            loader: "url-loader",
            options: {
              limit: "10240",
              name: "assets/[name].[ext]",
            },
          },
        ],
      },
      {
        // 按理说可以图片字体格式写在一起用url-loader，但是为了方便管理查看，分开写
        test: /\.(woff|woff2|eot|ttf|ttc|otf)$/,
        use: "url-loader",
      },
      // { test: /\.(woff|woff2|eot|ttf|ttc|otf)$/, use: 'file-loader' }
    ],
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      //   title: '测试',
      template: path.join(__dirname, "./src/index.html"),
    }),
    new CopyWebpackPlugin([
      {
        from: "static",
        to: "static",
      },
    ]),
    new webpack.HotModuleReplacementPlugin(), // 启动热更新的第二步
    // new webpack.NamedModulesPlugin() // NamedModulesPlugin，以便更容易查看要修补(patch)的依赖
  ],
  resolve: {
    extensions: [".js", ".jsx", "json"], // 表示这几个文件的后缀名，可以省略不写，程序会从左到右以此查找对应后缀名的文件是否存在并对应
    alias: {
      // 别名
      "@": path.join(__dirname, "./src"),
    },
  },
};
```

> [🌟 用法详解-掘金](https://juejin.im/post/5d6350c5e51d4561b072dd24)
>
> [比较：output.path、output.publicPath、devServer.publicPath 和 devServer.contentBase](https://github.com/fi3ework/blog/issues/39) copy-webpack-plugin 的原理会覆盖此改动

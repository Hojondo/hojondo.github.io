---
title: webpack
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-20 11:36:11
tags: ['前端', '打包', 'webpack']
password:
summary:
categories:
---

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

- 全局安装`npm i webpack -g`

  *不推荐全局安装 webpack，这会导致命令行运行 webpack 的时候锁定版本*

  示例安装jq：`npm i jquery -S` 

  ```js
  import $ from 'jquery'
  $(function () {
    $('li:odd').css('backgroundColor', 'blue')
  })
  ```

  执行全局安装的话，可在terminal写命令行 `webpack ./src/入口文件.js ./dist/bundle.js`

- 局部安装

  `npm install webpack webpack-cli -D`
  
  *webpack和 webpack-cli 曾经是在一起的，在 4.x 版本中进行了拆分，所以必须同时安装这两个*



## 配置文件结构`module.exports`

在最新的 [官方文档](https://webpack.js.org/configuration/) 中，webpack4有两个新的配置项，`mode` 和 `optimization`

[教程来源](https://juejin.im/post/5b53f977518825620f57dd4c)

### `mode` 

默认值production。

4.x 特点是约定大于配置，mode为对应值的时候，其他字段会有对应的默认设置。

- mode: production或development时，optimization都会开启的优化

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
      "namedChunks": true,
    }
  }
  ```

  

### `optimization`

*从 webpack 4 开始，会根据你选择的 [`mode`](https://webpack.docschina.org/concepts/mode/) 来执行不同的优化，不过所有的[优化](https://webpack.docschina.org/configuration/optimization/)还是可以手动配置和重写。*

- `minimize` ：告诉 **webpack** 使用 **TerserPlugin** 最小化打包文件，对打包文件进行 **压缩**、**混淆**。 **production模式** 下， 默认为 **true**
- `minimizer` ：允许你通过提供一个或多个定制过的 [TerserPlugin](https://webpack.docschina.org/plugins/terser-webpack-plugin/) 实例，覆盖默认压缩工具(minimizer)。
- `splitChunks` ：找到chunk中共享的模块,取出来生成单独的chunk。对于动态导入模块，默认使用 webpack v4+ 提供的全新的通用分块策略
- `runtimeChunk` ：会为每个仅含有 runtime 的入口起点添加一个额外 chunk
- `noEmitOnErrors` ：在编译出错时，使用来跳过生成阶段，可以确保没有生成出错误资源不写入到输出
- `namedModules` ：告知 webpack 使用可读取模块标识符来帮助更好地调试
- `namedChunks` ：告知 webpack 使用可读取 chunk 标识符来帮助更好地调试
- `moduleIds` ： 给模块有意义的名称代替ids
- `chunkIds` ：给chunk有意义的名称代替ids
- `nodeEnv` ：告知 webpack 将 process.env.NODE_ENV 设置为一个给定字符串
- `mangleWasmImports` ：在设置为 true 时，告知 webpack 通过将导入修改为更短的字符串，来减少 WASM 大小。这会破坏模块和导出名称。
- `removeAvailableModules` ：如果模块已经包含在所有父级模块中，告知 webpack 从 chunk 中检测出这些模块，或移除这些模块
- `removeEmptyChunks` ：如果 chunk 为空，告知 webpack 检测或移除这些 chunk
- `mergeDuplicateChunks` ：告知 webpack 合并含有相同模块的 chunk
- `flagIncludedChunks` ：告知 webpack 确定和标记出作为其他 chunk 子集的那些 chunk，其方式是在已经加载过较大的 chunk 之后，就不再去加载这些 chunk 子集
- `occurrenceOrder` ：排序最终打包结果中各个模块
- `providedExports` ：在可能的情况下确定每个模块的导出,被用于其他优化或代码生成，`export * from ...`时指定是否更高效的生成export代码
- `usedExports` ：依赖字段providedExports，告知webpack是否对每个模块都查找引用的exports
- `concatenateModules` ：依赖字段providedExports和字段usedExports，是否查找模块图表中可以安全连接到单个模块中的片段
- `sideEffects` ：是否 识别模块的package.json中sideEffects字段 或 rules 来跳过该模块，用于移除 JavaScript上下文 中的 未引用代码(dead-code) - tree shaking
- `portableRecords` ：告诉 webpack 生成 具有相对路径的记录，以便能够 移动上下文文件夹
- `mangleExports` ：控制export压缩
- `innerGraph` ：是否处理未使用的exports的内部模块图

### `devtool`

配置项的值为 true，生成 **.map** 文件，方便调试定位源码

### `context`

*webpack处理打包文件的时候的初始目录*

基础目录，**绝对路径**，用于从配置中解析 **入口起点(entry point)** 和 **loader**。

`context: path.resolve(__dirname, '../')`

### `entry`

*入口文件，webapck 4.x 默认的就是 src/index.js4.x 约定大于配置：其中 入口是`src->index.js`打包输出目录是`dist->main.js`*

> 1. 如果 **entry** 配置项的值的类型为 **string**， 对应 **单页面单入口应用**。在编译过程中， 生成 **一个模块依赖关系图**
> 2. 如果 **entry** 配置项的值的类型为 **[string]**, 对应 **单页面多入口应用**。应用程序启动时，数组中的入口文件按序执行。在编译过程中，生成 **一个模块依赖关系图**。
> 3. 如果 **entry** 配置项的值为 **object**， 对应 **多页面(单入口/多入口)应用**。在编译过程中，生成 **多个模块依赖关系图**。
> 4. 如果 **entry** 配置项的值的类型为 **function**，则是 **动态入口**

### `output`

- `path` ：output 目录对应一个**绝对路径**
- `filename` ：此选项决定了每个输出 bundle 的名称。这些 bundle 将写入到 [`output.path`](https://webpack.docschina.org/configuration/output/#output-path) 选项指定的目录下
- `publicPath` ：对于按需加载(on-demand-load)或加载外部资源(external resources)（如图片、文件等）来说，会为所有的资源指定一个基础路径。**设置了publicPath后，会为资源添加一个前缀**
- `chunkFilename` ：
- `auxiliaryComment` ：
- `chunkLoadTimeout` ：
- `crossOriginLoading` ：
- `jsonpScriptType` ：
- `devtoolFallbackModuleFilenameTemplate` ：
- `devtoolModuleFilenameTemplate` ：
- `devtoolNamespace` ：
- `assetModuleFilename` ：
- `globalObject` ：
- `hashDigest` ：
- `hashDigestLength` ：
- `hashFunction` ：
- `hashSalt` ：
- `hotUpdateChunkFilename` ：
- `hotUpdateFunction` ：
- `hotUpdateMainFilename` ：
- `jsonpFunction` ：
- `library` ：
- `libraryExport` ：
- `libraryTarget` ：
- `pathinfo` ：
- `sourceMapFilename` ：
- `sourcePrefix` ：
- `strictModuleExceptionHandling` ：
- `umdNamedDefine` ：
- `futureEmitAssets` ：
- `ecmaVersion` ：
- `compareBeforeEmit` ：
- `iife` ：
- `module` ：

### `module`

用于处理项目中的 **不同类型(.js、.css、.sass、.vue、.png ...)** 的模块规则

- `noParse`

  在构建 **依赖关系图** 时，**webpack** 会读取 **模块源文件** 的内容，然后 **将源文件内容解析成一颗AST树**。 分析 **AST树**，获取模块的 **依赖** 的 **静态模块**、**懒加载模块**、**全局变量**。如果 **模块的url(绝对路径)** 匹配 **noParse配置项指定的表达式**， 那么 **模块对应的源文件的代码结构** 不会被解析。 不解析大型的library可以 **提高构建性能**

- `rules`

  ```js
  rules: [
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
  ]
  ```

  

### `plugins`

### `resolve`

设置 **源文件**、**npm包** 如何被 **解析**。

> webpack在 **构建依赖关系图** 时会通过 **resolve** 配置项 **解析模块的url** 来获取 **模块源文件的绝对地址**。 通过 **源文件的绝对地址**，读取源文件的内容，然后使用 **解析器(parser)**  **解析源文件的代码结构**,获取 **依赖模块**。
>
> 此外，**webpack** 使用 **loader** 处理 **源文件** 时，也会通过 **resolve** 配置项 **解析 loader包 的 入口文件 的绝对地址**，然后使用 **入口文件** 提供的方法处理 **源文件** 内容。

### `resolveLoader`

用法和resolve配置项完全一致，区别在于 它仅用于解析webpack的loader包

### `devServer`

*使用了 webpack-dev-server 之后就需要有的配置，在这里可以配置详细的开发环境*

- `contentBase` ：告诉服务器从哪个目录中提供内容。只有在你想要提供静态文件时才需要。[`devServer.publicPath`](https://webpack.docschina.org/configuration/dev-server/#devserver-publicpath-) 将用于确定应该从哪里提供 bundle，并且此选项优先。推荐使用一个绝对路径,例:`path.join(__dirname, 'public')`
- `publicPath` ：此路径下的打包文件可在浏览器中访问。
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

webpack 能够为 **多种环境** 编译构建， 可通过 **target配置项** 指定一个 **具体的环境**

### `externals`

在某些情况下，我们是 **不希望将依赖的类库** 打包到最后输出的 **bundle** 中的。此时， 我们通过 **externals配置项指定不参与编译打包的类库**。

### `performance`

用于配置如何展示性能提示。例如，如果一个资源超过 250kb，webpack 会对此输出一个警告来通知你。

### `node`

这些选项可以配置是否 polyfill 或 mock 某些 [Node.js 全局变量](https://nodejs.org/docs/latest/api/globals.html)和模块。这可以使最初为 Node.js 环境编写的代码，在其他环境（如浏览器）中运行

### `stats`

统计信息

***

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

设置 `require.amd` 或 `define.amd` 的值，某些流行的模块是按照 AMD 规范编写的，为false时将禁用webpack对AMD的支持

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
   - `--contentBase src`  指定根目录是src
   - `--hot` 热更新，局部更新，不是刷新页面没有重新生成新的bundle.js，而是加了两个`xxx.hot-update.js/json`文件

2. 通过配置文件内添加`devServer`字段来指定

   ```js
   module.exports = {
     devServer: {
           open: true, // 自动打开浏览器
           port: 3000, // 设置启动时候的运行端口
           contentBase: 'src', // 指定托管根目录
           hot: true // 启动热更新 的第一步
     },
     plugins: [
       new webpack.HotModuleReplacementPlugin(), // 启动热更新的第二步
     ]
   }
   ```

## 几个常用插件介绍

### `html-webpack-plugin`

[详解文章](https://www.cnblogs.com/wonyun/p/6030090.html)

简化了HTML文件的创建，以便为你的webpack包提供服务。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。 你可以让插件为你生成一个HTML文件，使用[lodash模板](https://lodash.com/docs#template)提供你自己的模板，或使用你自己的[loader](https://webpack.docschina.org/loaders)。

两个作用：

1. 根据指定页面自动在内存中生成对应页面。可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置**N**个`html-webpack-plugin`可以生成**N**个页面入口
2. 自动创建合适的script标签，引入对应路径的bundle.js。为html文件中引入的外部资源如`script`、`link`动态添加每次compile后的hash，防止引用缓存的外部文件问题

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

## 几个常用loader

Webpack默认只能打包处理js类型的文件，无法处理其他的非js类型文件，需要手动安装一些合适的第三方loader加载器

> **分析调用第三方loader的过程**
>
> 1. 发现要处理的文件不是js文件，就会去配置文件中，查找有没有对应的第三方loader规则
> 2. 如果能找到对应的规则，就会调用对应的loader处理这种文件类型
> 3. 在调用loader的时候，是从后往前依次调用的
> 4. 当最后一个loader调用完毕，会把处理的结果直接交给webpack进行打包合并，最终输出到bundle.js中去

### `url-loader` 

不管是图片还是自体库，只要是url地址 都用这个处理，内部依赖了`file-loader`

*limit参数是指maxSizeToBeBase64 单位是KB*

```js
{
  	test: /\.(jpg|png|gif|bmp|jpeg)$/,
    use: 'url-loader?limit=6000&name=[hash:8]-[name].[ext]'
}
```



### `sass-loader`

### `less-loader`

### `babel-loader`

在webpack中 运行如下两套命令，安装两套包，去安装babel相关的loader功能

`npm i babel-core babel-loader babel-plugin-transform-runtime -D`

`npm i babel-preset-env babel-preset-stage-0 -D`

在webpack配置中加入rules

新建`.babelrc`文件，写babel配置，必须符合JSON语法规范

```js
{
   test: /\.js|jsx$/, // test:处理什么类型的文件
   use: ['babel-loader'], // use:用什么
   include: path.join(__dirname, 'src'), // include:处理这里的
   exclude: /node_modules/ // exclude:不处理这里的；
},
```

*如果不排除node_modules中的所有第三方js文件，都打包编译的话 最终babel把所有node_modules中的js转换完毕了耶无法正常运行项目*


## 示例

```js
const path = require('path'); // 由于webpack是基于node进行构建的，所以 webpack配置文件中，任何合法的node代码都是支持的
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
    devServer: {
        open: true, // 自动打开浏览器
        port: 3000, // 设置启动时候的运行端口
        contentBase: 'src', // 指定托管根目录
        hot: true // 启动热更新 的第一步
    },
    module: { //module是配置所有模块要经过什么处理，用于配置所有第三方loader的规则，webpack 默认只能打包处理.js后缀名类型的文件;像.png.vue之类的无法主动处理，所以要配置第三方的loader
        rules: [
            {
                test: /\.js|jsx$/, // test:处理什么类型的文件
                use: ['babel-loader'], // use:用什么
                include: path.join(__dirname, 'src'), // include:处理这里的
                exclude: /node_modules/ // exclude:不处理这里的；
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
                test: /\.(jpg|png|gif|bmp|jpeg|ico|svg)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            limit: '10240',
                            name: 'assets/[name].[ext]'
                        }
                    }
                ]
            },
            { // 按理说可以图片字体格式写在一起用url-loader，但是为了方便管理查看，分开写
                test: /\.(woff|woff2|eot|ttf|ttc|otf)$/,
                use: 'url-loader'
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
        new webpack.HotModuleReplacementPlugin() // 启动热更新的第二步
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



> [🌟用法详解-掘金](https://juejin.im/post/5d6350c5e51d4561b072dd24)
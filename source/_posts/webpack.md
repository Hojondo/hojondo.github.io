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



## 配置文件结构

在最新的 [官方文档](https://webpack.js.org/configuration/) 中，webpack4有两个新的配置项，`mode` 和 `optimization`

[教程来源](https://juejin.im/post/5b53f977518825620f57dd4c)

- `module.exports`
  
  - `mode` 
  
    *[mode是个啥](https://juejin.im/post/5b53f977518825620f57dd4c#heading-3)production || development*
  
  - `optimization`
  
  - `devtool`
  
  - `context`
  
    *webpack处理打包文件的时候的厨师目录*
  
  - `entry`
  
    *入口文件，webapck 4.x 默认的就是 src/index.js4.x 约定大于配置：其中 入口是`src->index.js`打包输出目录是`dist->main.js`*
  
  - `output`
  
  - `module`
  
  - `plugins`
  
  - `resolve`
  
  - [devServer]
  
    *使用了 webpack-dev-server 之后就需要有的配置，在这里可以配置详细的开发环境*
    
    - `clientLogLevel`
    - `historyApiFallback`
    - `hot`
    - `compress`
    - `host`
    - `port`
    - `open`
    - `overlay`
    - `publicPath`
    - `proxy`
    - `quiet`
    - `watchOptions`



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

- `html-webpack-plugin`

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
    chunks: 'all', // 允许插入到模板中的一些chunk，不配置此项默认会将entry中所有的thunk注入到模板中。在配置多个页面时，每个页面注入的thunk应该是不相同的，需要通过该配置为不同页面注入不同的thunk
    excludeChunks: [], // 这个与chunks配置项正好相反，用来配置不允许注入的thunk
    title: 'Webpack App', // 生成的html文档的标题
    xhtml: false // true|fasle, 默认false；是否渲染link为自闭合的标签，true则为自闭合标签
  }),
  ```

  

- `clean`

## 几个常用loader

Webpack默认只能打包处理js类型的文件，无法处理其他的非js类型文件，需要手动安装一些合适的第三方loader加载器

### 分析调用第三方loader的过程

1. 发现要处理的文件不是js文件，就会去配置文件中，查找有没有对应的第三方loader规则
2. 如果能找到对应的规则，就会调用对应的loader处理这种文件类型
3. 在调用loader的时候，是从后往前依次调用的
4. 当最后一个loader调用完毕，会把处理的结果直接交给webpack进行打包合并，最终输出到bundle.js中去

- `url-loader` 不管是图片还是自体库，只要是url地址 都用这个处理，内部依赖了`file-loader`

  *limit参数是指maxSizeToBeBase64 单位是KB*

  ```js
  {
    	test: /\.(jpg|png|gif|bmp|jpeg)$/,
      use: 'url-loader?limit=6000&name=[hash:8]-[name].[ext]'
  }
  ```

  

- `sass-loader`

- `less-loader`

- `babel`

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


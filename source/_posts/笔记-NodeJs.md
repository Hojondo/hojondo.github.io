---
title: NodeJs
top: false
cover: false
toc: true
mathjax: false
date: 2020-01-15 16:39:08
tags: ["node"]
password:
summary:
categories: ["笔记"]
---

[TOC]

# Node.js

## 前言

### 浏览器的组成

- 人机交互部分 UI
- 网络请求部分 socket
- javascript 引擎部分（解析之行 javascript）
- 渲染引擎部分（渲染 HTML、CSS）
- 数据存储部分（cookie、localStorage、sessionStorage）

### 渲染引擎

#### 主流渲染引擎

- chrome Blink(Webkit 分支)
- safari Webket 引擎
- firefox Gecko
- opera Blink
- IE Trident
- Edge EdgeHTML 引擎（Trident 的一个分支

#### 引擎工作原理

- DOM 树 CSS 规则树 合并一起成 渲染树。
  ![主流webkit/gecko工作原理](https://s2.ax1x.com/2020/02/09/1f1QzR.png)

- Layout、reflow 的过程

  用`documentFragment`

- 浏览器访问网址过程

  1. 把网址封装成 http 请求报文(包括 get、host、connection 等..)
  2. 浏览器发起 DNS 解析请求，将域名转换成 IP 地址
  3. 浏览器将请求报文发送给服务器
  4. 服务器接收请求报文，并解析
  5. 服务器处理用户请求，将处理结果封装成 http 响应报文（包括 ContentType、Timing-Allow-Origin 等）
  6. 服务器将 HTTP 响应报文发送给浏览器
  7. 浏览器接收服务器响应报文，解析
  8. 浏览器解析 HTML 页面并展示，在解析过程中遇到新的资源时需要再次发起请求
  9. 最终展示页面

- 请求报文/响应报文的格式

  [教程](https://www.cnblogs.com/klguang/p/4618526.html)

  ![报文结构示意图](https://images0.cnblogs.com/blog2015/776887/201507/241034588189239.png)

  1. 开始行/起始行`start line`（请求行/响应行

     - 请求行

       例`POST /infoNewsAction_uploadxheditorfile.action?immediate=1 HTTP/1.1`

       1. 方法：GET、POST、PUT、HEAD、DELETE、OPTIONS、TRACE、CONNECT、LINK、UNLINK
       2. URL
       3. HTTP 版本

     - 响应行

       例`HTTP/1.1 200 OK`

       1. 状态码：1xx、2xx、3xx、4xx、5xx
       2. HTTP 版本

  2. 首部行/报文头`header`（用来说明浏览器、服务器或报文主题的一些信息。每个首部行在结束地方都有`CRLF换行`

     例`Cache-Control:max-age=60`

     1. [首部字段名](https://www.processon.com/view/link/58025201e4b0d6b27dd4c8af#map)：**通用首部字段**、**请求首部字段**、**响应首部字段**、**实体首部字段**
     2. 值

  3. `CRLF空行`：主体和首部行之间有空行

  4. 【可选】实体/主体`entity-body`

     [报文主体和实体的概念](https://www.zhihu.com/question/263752229)

     在请求报文中，一般是 post/put 提交的表单信息

- DNS 解析过程 [教程](https://www.maixj.net/ict/dns-chaxun-9208)

- WEB 开发本质

  - 请求，客户端发起请求
  - 处理，服务器处理请求
  - 响应，服务器将处理结果发送给客户端
  - 客户端处理响应：C/S 架构和 B/S 架构

## 介绍

Node.js 是个一开发平台【开发平台的概念：有对应的变成语言，有语言运行时（Runtime），有能实现特定功能的 API(SDK:Software Development Kit)】，类似于 PHP 开发平台、Apple 开发平台、.net 开发平台。

- 该平台使用的语言是 javascript。
- 该平台 Runtime 是基于 Chrome V8 Javascript 引擎构建。
- 基于 node.js 可以开发控制台程序(命令行程序、CLI 程序)、桌面应用程序(GUI，借助 electron 等)、Web 应用程序(网站)

Node.js 全栈开发技术栈-MEAN：MongoDb 数据库、Express WEB 开发框架、Angular 前台、Node.js 后台

特点：

- 事件驱动（当事件被触发时，执行传递过去的回调函数）
- 非阻塞 I/O
- 单线程
- 拥有开源生态环境 - npm

应用场景：
服务器开发、web 请求和响应过程、了解服务器端如何和客户端配合
了解服务器端渲染
服务器端为客户端写接口

### NODE & NVM

`nvm`工具 可以实现在随时切换和管理 node 版本：建议先装 nvm 再装 node

- nvm version
- nvm install stable
- nvm install 版本号
- nvm uninstall 版本号
- nvm list
- nvm use 版本号 来应用该版本

## 快速入门几个要点

### nodejs 和传统 php 等开发网站的区别

传统的后端：需要处于一个服务器容器 apache 等-监听用户请求并且根据不同请求作出不处理
nodeJS：既是 http 服务器，继续自己处理

### REPL 介绍 read-eval-print-loop(交互式解释器)

类似于 devtool 里的 console tab
terminal 输入 nodej 进入，control+C 退出

### 第一个项目小实践

1. 新建 js 文件
2. 执行 node file.js

### 全局模块 Globals

比如 process，其他的 非全局模块需要 reqire 来加载

查看 API 的稳定性 分 3 级：stablity0 表示该 api 已经过时了红色；1 表示正在测试开发阶段橙色；2 是可正常使用蓝色

### Buffer 类型

二进制数组对象。主要用于方便数据缓冲，易于传输。
js 中没有读取或操作饿紧致数据流的机制。NodeJS 中引入了 Buffer 使我们可以操作 TCP 流或文件流；Buffer 累踩坑的对象类似于整数数组，萏 Buffer 的大小是固定的，且在 V8 堆外分配物理内存。Buffer 的大小(Buffer.length)在被创建时确定，且无法调整;Buffer 是全局的，所以使用的时候无需 require()的方式加载

- `Buffer.from()`创建一个 Buffer 对象
- `Buffer.byteLength` 获取字符串对应的字节个数
- `Buffer.isBuffer(obj)` 判断一个对象是否是 Buffer 类型对象
- `buf[index]` 某个 buffer 实例的某个字节
- `bug.length` 某个 buffer 实例的字节个数

### fs 读写文件操作

`let fs = require('fs')`

- 读文件 fs.writeFile(file, data [,options], callback)
- 写文件 fs.readFile(file [,options], callback(err,Bufferdata))

### node 的单线程的异步操作 - 非阻塞 I/O 解释

node 的 event loop: 6 步
链接(https://developer.aliyun.com/lesson_1730_14094#_14094)
在线动画演示网址：http://latentflip.com/loupe
参考视频：www.youtube.com/watch?v=8aGhZQkoFbQ

### 文件路径

js 文件内的./路径是指 执行 js 文件时所处的目录，执行时是相对于这个来查找文件；
而非根据 js 文件所在目录 来查找文件。
**\_\_dirname** 表示该 js 文件所在的绝对路径名
**\_\_filename** 表示该 js 文件的完整绝对路径名（相比于 dirname 多一个自身的名字）
_这两个变量并非全局变量，可以理解成 node 执行时将文件内代码封装成(functiong(**dirname,**filename){xxx})('/c/user/local/sss', 'c/user/local/sss/xx.js')_

### 使用 path 模块进行路径拼接

为了替代`var filePath = __dirname + '/'+ 'xx.txt'`,不同操作系统或**dirname 内多少个/的边界问题
`var path require('path')`
`var filePath = path.join(**dirname, 'xx.txt')`

### http 服务

```js
var http = require("http");
var server = http.createServer();
// 监听用户的请求事件(request事件) 回调函数两个行参(request,response)
server.on("request", function (req, res) {
  // if(req ???)
  res.setHeader("Content-Type", "text/html;charset=utf-8"); // 服务器设置http响应报文头，告诉浏览器使用响应的编码来解析网页
  res.write("响应内容");
  res.end(); // 对于每个请求，服务器必须结束响应，否则客户端会一直等待服务器响应结束
});
// 启动服务
server.listen(8080, function () {
  console.log("服务器启动了");
});
```

#### 回调函数行参-request 对象 常用 api

`http.IncomingMessage`

- .headers 报文头
- .rawHeaders 原生报文头（和 headers 的区别是：headers 返回`key1:val1,key2:val2...`的对象，而 rawHeaders 返回`[key1,val1,key2,val2...]`）
- .httpVersion 拿到请求客户端所使用的 http 协议版本
- .method 客户端请求方式 get、post、delete 等
- .url 获取本次请求的路径 不包含主机名称 端口号 域名等

#### 回调函数行参-response 对象 常用 api

`http.httpServerResponse`

- .setHeader() 设置响应报文头
- .statusCode = 404 设置 http 响应状态码
- .statusMessage = 'message' 设置 http 响应状态码对应的消息
- .writeHead() 直接向客户端写入 http 响应报文头,优先级大于其他所有，如果没写这个，end()时默认执行。如 res.writeHead(404, 'not found', {'Content-Type': 'text/html;charset=utf-8'})
- .write() 响应内容
- .end() 通知服务器 所有响应头和响应主体已被发送，服务器将其视为已完成

### NPM & NRM

# Express 框架

基于 NodeJs 的 web 开发框架

## 介绍

- 实现路由功能 没必要自己写很多 if(req.url) else
- 中间件(函数)功能 把监听 request 事件拆分成了很多方法
- 对 req 和 res 对象的扩展
- 本身并没有模版引擎，可以集成其他模版引擎

## 路由

1. get/post/put/delete...

```js
/**
 * 通过中间件监听指定的路由的请求
 * 支持：put/delete/get/post 等http methods
 * 虚拟路径pathname 接受 正则RegExp 匹配
 */
app.get("/", (req, res) => {
  // 只监听get请求
  res.send("在首面");
  /**
   * send()相当于原生的end();
   * 区别如下：
   * 1. send()自动封装了很多比如setHeader(‘Content-Type’, ‘text/html;charset=utf-8’)等优化
   * 2. end()只接受string or Buffer；send()可以是 string, Buffer, Object or Array
   */
});
app.get(/^\/index(\/.+)*$/, (req, res) => {
  res.send("在index及子路径下 的 get请求");
});
app.post("/add", (req, res) => {
  res.send("post add");
});
```

2. use

```javascript
/**
 * mark 当两次相同路由匹配，执行了不同的express.static到不同文件夹下请求回调，则会优先第一次回调结果。逻辑是先找第一个资源 找不到的话再找第二个
 */
const fn = express.static(path.join(__dirname, "static")); // 处理静态资源的方法，指定静态资源路径
app.use("/", fn); // 实现所有静态资源 托管
/**
 * use 和 以上几种 method 的区别是：
 * 1. 路由匹配时 不限定方法，什么请求方法都可以
 * 2. 请求路径中的第一部分(以/分割)只要与 /index 相等即可，并不要求pathname ===
 */
app.use("/home", (req, res) => {
  res.send("在home及子路径下");
});
```

3. all

```javascript
/**
 * all
 * 1. 路由匹配时 不限定方法，什么请求方法都可以
 * 2. 请求路径中 pathname 必须 === 完全匹配
 */
app.all("/all", (req, res) => {});
```

4. paramas

```javascript
/**
 * req.params获取路由中的参数
 * :开头 表示占位符
 * 并且需要严格匹配占位符数量
 */
app.get("/news/:year/:month/:day", (req, res) => {
  res.send(req.params);
});
```

## res API

```javascript
res.json({ a: "1", b: 2 }); // 将object或array转为json作为响应发，等同与res.send(json)
res.redirect([状态码默认302], path); // 重定向 封装原生流程：1. 设置状态码res.statusCode = 301或302 2. 设置消息 res.statusMessage = '重定向' 3. 设置相应报文头setHeader('location', 'path') 4. res.end()
res.redirect("https://google.com");
res.sendFile(path, function (err) {
  if (err) throw err;
}); // 封装原生的 readFile('xx.txt', (err,data)=>{res.end(data)})
res.status(404).end("文件不存在"); // 封装原生流程： 1. 设置状态码res.statusCode = 301或302 2. 设置消息 res.statusMessage = '重定向' 3. res.end()
/**
 * res.render(viewpath模版文件路径 [,locals一个替换模版中占位符key的值val的object] [, callback(err, html)])
 * 需要给express配置一个模版引擎render才能工作，比如jade，ejs，pug, 然后在应用中进行如下设置才能让Express渲染模版文件
 * 1. views 放模版文件的目录，比如 app.set('views', './views')
 * 2. view engine 模版引擎，比如 app.set('view engine', 'ejs')
 * 3. res.render('xx.html', {username: 'x'}, (err,html)=>{})
 */
```

## req API

## 拆分封装 config & router

```js
// 不推荐该方法
// in router.js
module.exports = function(app) {
  app.get('/', (req,res)=>{
    res.send('所有')
  })
}
// in server.js
const router = require(./router.js)
const express = require('express');
const app = express()
router(app)
```

更推荐如下

```js
// in handleRouter.js 封装所有路由监听回调
module.exports.index = function(req,res){
  res.send('这里是handler index')
}

// in router.js
const express = require('express');
const handler = require('./handleRouter');
const router = express.Router(); // 创建一个router对象
router.app.get('/', handler.index)
router.app.get('/home', (req,res)=>{
  res.send('首页')
})
module.exports = router
// in server.js
const router = require(./router.js)
const express = require('express');
const app = express()
// 设置app与router相关联，此处router是作为中间件 既是object又是function
app.use('/', router) // 关键点去看下use源码逻辑 并且tips一下：等价于app.use(function(req,res){}) 即此处 app.use(router)
```

---

[node 全栈课程大纲](https://www.processon.com/view/link/5d4b852ee4b07c4cf3069fec)

> [devDependencies vs dependencies](https://stackoverflow.com/questions/40143357/do-you-put-babel-and-webpack-in-devdependencies-or-dependencies/40143446#40143446)

## 安装

全局安装 `npm i nodemon -g` 代替 node 自动重启 `nodemon file.js`

## 运行

1. bash 运行： `node xx.js`
2. nodemon
3. vs code 进行 debug: RUN and Debug

## 单元测试

`npm install jest -g`
`__test__ > xxname.spec.js`
`jest foldername --watch`

```js
test('测试备注名', ()=>{
  const ret = require('../index)
  console.log(ret)
  expect(ret)
    .toBe('期望值')
})
```

## 测试代码 自动生成工具

```js
// index.spec.js
const fs = require("fs");
const path = require("path");
test("集成测试 测试生成测试代码文件", () => {
  // 删除测试文件夹
  fs.rmdirSync(path.join(__dirname, "..", "/data/__test__"), {
    recursive: true, // 递归为true 则同时迭代清除文件夹下的所有文件
  });
  const src = new (require("../index"))();
  src.getJsetSource(path.join(__dirname, "..", "/data"));
});
```

```js
const path = require("path");
const fs = require("fs");
module.exports = class TestGenerator {
  getJsetSource(sourcePath = path.resolve("./")) {
    const testPath = `${sourcePath}/__test__`;
    if (!fs.existsSync(testPath)) fs.mkdirSync(testPath);
    // 遍历代码文件
    let list = fs.readdirSync(sourcePath);
    list
      .map((v) => `${sourcePath}/${v}`) // 添加为完整路径
      // 过滤文件
      .filter((v) => fs.statSync(v).isFile())
      // 排除测试代码
      .filter((v) => v.indexOf(".spec") === -1)
      .map((v) => this.genTestFile(v));
  }
  genTestFile(filename) {
    const testFileName = this.getTestFileName(filename);
    // 判断此文件是否存在
    if (fs.existsSync(testFileName)) {
      console.log(`该测试代码已存在${testFileName}`);
      return;
    }
    const mod = require(filename);
    let source;
    if (typeof mod === "object") {
      const baseName = path.basename(filename);
      source = Object.keys(mod)
        .map((v) => this.getTestSource(v, baseName, true))
        .join("\n");
    } else if (typeof mod === "function") {
      const baseName = path.basename(filename);
      source = this.getTestSource(baseName.replace(".js", ""), baseName);
    }
    fs.writeFileSync(testFileName, source);
  }
  getTestSource(methodName, classFile, isClass = false) {
    return `
test('TEST ${methodName}', ()=>{
  const ${
    isClass ? "{" + methodName + "}" : methodName
  } = require('../${classFile}')
  const ret = ${methodName}()
  // expect(ret)
  //  .toBe('test return')
})
    `;
  }
  getTestFileName(filename) {
    const dirName = path.dirname(filename);
    const baseName = path.basename(filename);
    const extName = path.extname(filename);
    const testName = baseName.replace(extName, `.spec${extName}`);
    return path.format({
      root: dirName + "/__test__/",
      base: testName,
    });
  }
};
```

# 异步编程

[教程链接](https://www.josephxia.com/document/node/)

- js 的执行环境是单线程
- I/O 处理需要回调函数异步处理（异步 I/O）
- 前端异步 IO 可以消除 UI 阻塞，提高用户体验
- 后端异步可以提高 CPU 和内存利用率

javascript 异步解决方案的进化

- callback
- promise
- generator
- async & await:
  > 任何一个 await 语句后面的 Promise 对象变为 reject 状态，那么整个 async 函数都会中断执行。
  > async 函数返回的 Promise 对象，必须等到内部所有 await 命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到 return 语句或者抛出错误。也就是说，只有 async 函数内部的异步操作执行完，才会执行 then 方法指定的回调函数。
- eventEmitter 事件监听方式 event.emit()&event.on()

# node.js 基础

## I/O 处理

- 同步阻塞
- 同步非阻塞
- 异步阻塞
- 异步非阻塞

## node 文档

英文 https://nodejs.org/dist/latest-v10.x/docs/api/
中文 http://nodejs.cn/api/

## 基础 API

- readFileSync & readFile
- promisify
  `const { promisify } = require('util')` `const readFile = promisify(fs.readFile)`
- Buffer 读取数据类型为 Buffer。用于在 TCP 流、文件系统操作、以及其他上下文中与八位字节流进行交互。 八位字节组 成的数组，可以有效的在 JS 中存储二进制数据
- readFile 和 writeFile 是会占用服务器缓存空间的，所以用 stream
- Http 创建一个 http 服务器
  - response.end() 即 中止这个 stream
  - 流 stream
  - [res.setHeader()和 res.writeHead()](https://www.jianshu.com/p/4418de5d6183)

## CLI 工具

实现一个 cli 工具（vue 路由约定）

- npm init
- 新建脚本 xx.js，开头是`#!/usr/bin/env node`对 shell 指定使用 node 解析脚本
- 在 package.json > bin > xx.js 指定开始脚本
  - init**初始化 clone & spawn & open**: spawn
  - refresh**约定路由**: hbs([handlebars 模版引擎](https://zhuanlan.zhihu.com/p/32742178)/[hbs 最佳实践](https://www.jianshu.com/p/6dccc8459cd8))
  - serve
- [npm link](https://www.jianshu.com/p/aaa7db89a5b2) 其实就是相当于 ln 指令操作
- publish 发布自己的库 执行`publish.sh`脚本

## Koa 源码

- koa 的产生原因： 原生 http 的不足：1.令人困惑的 request 和 response;2. 对描述复杂业务逻辑的描述比如 AOP(Aspect-oriented-programing)/切面描述需要
- 为了简化 API，引入上下文 context 概念，核心是 `洋葱圈模型 - use, next`
- 简单实现一个 koa 框架
  - index.js
    - use
    - listen
  - kkb.js
    - use
      - createContext context.js
        - request.js
        - response.js
    - listen

## 网络工程学

- [OSI 7 层协议和 TCP/IP 4 层协议](https://blog.csdn.net/mccand1234/article/details/51590804)
  ![图示](https://i.loli.net/2020/08/17/iUX4ZmkSJxytK2O.png)
- TCP 面向可靠性连接，UDP 不可靠
- TELNET 传输的是明文，SSH 更安全

- TCP 协议写个聊天室 js `require('net')`
- **[http 协议 - 前端角度](https://www.processon.com/view/link/5ec52841e0b34d5f261e14e0#map)[http 协议- 网工角度](https://processon.com/view/5c5157f7e4b0f0908a8c996e)**查看 req & res 例子： `curl -v http://www.baidu.com`
  - request: 请求行（method.etc），消息报头（Accept 系,Content-Type.etc），请求正文(根据 Content-Type 确定)
  - response： 状态行（1xx...5xx），实体报头，响应正文
  - [http 缓存](https://juejin.im/post/6844904116972421128)
  - http 例子， img.src 埋点
  - 浏览器跨域三层封印： http request 发送时，已经发送给了后端也返回给了前端，但是浏览器不显示 response。协议端口域名 3 者任一不同就是非同源.
    - node 层设置`res.setHeader('Access-Control-Allow-Origin', '*'或特写某个url)`
    - [预检请求，即在请求阶段被浏览器拦截](https://www.jianshu.com/p/b55086cbd9af) option
    - 如果携带 cookie 信息：`res.setHeader('Access-Control-Allow-Credentials', 'true')`
    - 服务器反向代理： 让同源服务器去请求非同源服务器，返回给同源的前端。`const proxy = require('http-proxy-middleware')`
- bodyParer
- 实现一个爬虫 `request`
- 又补充了 http/socket.io 两种方法写 im 通讯程序
- 用 socket.io 写了浏览器模拟 terminal，推拉流：后端的流推到前端
- monaco-editor 编辑器，尤雨溪也写了个基于这个的 docker 在线编辑器

# todo

`path`相关方法

- path.dirname
- path.basename
- path.extname
- path.resolve
- \_\_dirname

`fs`相关方法

- fs.mkdirSync
- fs.rmdirSync
- fs.existsSync
- fs.statSync 是不是一个文件

# 参考

> [Koa, Redux, Express 中间件对比](https://github.com/nanjixiong218/analys-middlewares/tree/master/src) > [对 Compose 详尽的总结](https://segmentfault.com/a/1190000016707187#item-7-5) > [责任链模式](https://blog.csdn.net/liuwenzhe2008/article/details/70199520)

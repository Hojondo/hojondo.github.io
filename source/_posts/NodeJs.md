---
title: NodeJs
top: false
cover: false
toc: true
mathjax: false
date: 2020-01-15 16:39:08
tags: ['node']
password:
summary:
categories:
---

[TOC]

# Node.js

## 前言

### 浏览器的组成

- 人机交互部分 UI
- 网络请求部分 socket
- javascript 引擎部分（解析之行javascript）
- 渲染引擎部分（渲染HTML、CSS）
- 数据存储部分（cookie、localStorage、sessionStorage）

### 渲染引擎

#### 主流渲染引擎

- chrome  Blink(Webkit分支)
- safari Webket引擎
- firefox Gecko
- opera Blink
- IE Trident
- Edge EdgeHTML引擎（Trident的一个分支

#### 引擎工作原理

- DOM树 CSS规则树 合并一起成 渲染树。
  ![主流webkit/gecko工作原理](https://s2.ax1x.com/2020/02/09/1f1QzR.png)

- Layout、reflow的过程

  用`documentFragment`

- 浏览器访问网址过程

  1. 把网址封装成http请求报文(包括get、host、connection等..)
  2. 浏览器发起DNS解析请求，将域名转换成IP地址
  3. 浏览器将请求报文发送给服务器
  4. 服务器接收请求报文，并解析
  5. 服务器处理用户请求，将处理结果封装成http响应报文（包括ContentType、Timing-Allow-Origin等）
  6. 服务器将HTTP响应报文发送给浏览器
  7. 浏览器接收服务器响应报文，解析
  8. 浏览器解析HTML页面并展示，在解析过程中遇到新的资源时需要再次发起请求
  9. 最终展示页面

- 请求报文/响应报文的格式

  [教程](https://www.cnblogs.com/klguang/p/4618526.html)

  ![报文结构示意图](https://images0.cnblogs.com/blog2015/776887/201507/241034588189239.png)

  1. 开始行/起始行`start line`（请求行/响应行

     - 请求行

       例`POST /infoNewsAction_uploadxheditorfile.action?immediate=1 HTTP/1.1`

       1. 方法：GET、POST、PUT、HEAD、DELETE、OPTIONS、TRACE、CONNECT、LINK、UNLINK
       2. URL
       3. HTTP版本

     - 响应行

       例`HTTP/1.1 200 OK`

       1. 状态码：1xx、2xx、3xx、4xx、5xx
       2. HTTP版本

  2. 首部行/报文头`header`（用来说明浏览器、服务器或报文主题的一些信息。每个首部行在结束地方都有`CRLF换行`

     例`Cache-Control:max-age=60`

     1. [首部字段名](https://www.processon.com/view/link/58025201e4b0d6b27dd4c8af#map)：**通用首部字段**、**请求首部字段**、**响应首部字段**、**实体首部字段**
     2. 值

  3. `CRLF空行`：主体和首部行之间有空行

  4. 【可选】实体/主体`entity-body`

     [报文主体和实体的概念](https://www.zhihu.com/question/263752229)

     在请求报文中，一般是post/put提交的表单信息

- DNS解析过程 [教程](https://www.maixj.net/ict/dns-chaxun-9208)

- WEB开发本质

  - 请求，客户端发起请求
  - 处理，服务器处理请求
  - 响应，服务器将处理结果发送给客户端
  - 客户端处理响应：C/S架构和B/S架构

## 介绍

Node.js是个一开发平台【开发平台的概念：有对应的变成语言，有语言运行时（Runtime），有能实现特定功能的API(SDK:Software Development Kit)】，类似于PHP开发平台、Apple开发平台、.net开发平台。

- 该平台使用的语言是javascript。
- 该平台Runtime是基于Chrome V8 Javascript引擎构建。
- 基于node.js可以开发控制台程序(命令行程序、CLI程序)、桌面应用程序(GUI，借助electron等)、Web应用程序(网站)

Node.js全栈开发技术栈-MEAN：MongoDb数据库、Express WEB开发框架、Angular前台、Node.js后台

特点： 
- 事件驱动（当事件被触发时，执行传递过去的回调函数）
- 非阻塞I/O
- 单线程
- 拥有开源生态环境 - npm

应用场景：
服务器开发、web请求和响应过程、了解服务器端如何和客户端配合
了解服务器端渲染
服务器端为客户端写接口
### NODE & NVM
`nvm`工具 可以实现在随时切换和管理node版本：建议先装nvm再装node
- nvm version
- nvm install stable
- nvm install 版本号
- nvm uninstall 版本号
- nvm list
- nvm use 版本号 来应用该版本

## 快速入门几个要点
### nodejs 和传统php等开发网站的区别
传统的后端：需要处于一个服务器容器apache等-监听用户请求并且根据不同请求作出不处理
nodeJS：既是http服务器，继续自己处理

### REPL介绍 read-eval-print-loop(交互式解释器)
类似于devtool里的console tab
terminal输入nodej进入，control+C退出

### 第一个项目小实践
1. 新建js文件
2. 执行 node file.js

### 全局模块Globals
比如process，其他的 非全局模块需要reqire来加载

查看API 的稳定性 分3级：stablity0表示该api已经过时了红色；1表示正在测试开发阶段橙色；2是可正常使用蓝色

### Buffer类型
二进制数组对象。主要用于方便数据缓冲，易于传输。
js中没有读取或操作饿紧致数据流的机制。NodeJS中引入了Buffer使我们可以操作TCP流或文件流；Buffer累踩坑的对象类似于整数数组，萏Buffer的大小是固定的，且在V8堆外分配物理内存。Buffer的大小(Buffer.length)在被创建时确定，且无法调整;Buffer是全局的，所以使用的时候无需require()的方式加载
- `Buffer.from()`创建一个Buffer对象
- `Buffer.byteLength` 获取字符串对应的字节个数
- `Buffer.isBuffer(obj)` 判断一个对象是否是Buffer类型对象
- `buf[index]` 某个buffer实例的某个字节
- `bug.length` 某个buffer实例的字节个数 


### fs读写文件操作
`let fs = require('fs')`
- 读文件 fs.writeFile(file, data [,options], callback)
- 写文件 fs.readFile(file [,options], callback(err,Bufferdata))

### node的单线程的异步操作 - 非阻塞I/O解释
node 的event loop: 6步
链接(https://developer.aliyun.com/lesson_1730_14094#_14094)
在线动画演示网址：http://latentflip.com/loupe
参考视频：www.youtube.com/watch?v=8aGhZQkoFbQ

### 文件路径
js文件内的./路径是指 执行js文件时所处的目录，执行时是相对于这个来查找文件；
而非根据 js文件所在目录 来查找文件。
**__dirname** 表示该js文件所在的绝对路径名
**__filename** 表示该js文件的完整绝对路径名（相比于dirname多一个自身的名字）
*这两个变量并非全局变量，可以理解成 node执行时将文件内代码封装成(functiong(__dirname,__filename){xxx})('/c/user/local/sss', 'c/user/local/sss/xx.js')*

### 使用path模块进行路径拼接
为了替代`var filePath = __dirname + '/'+ 'xx.txt'`,不同操作系统或__dirname内多少个/的边界问题
`var path require('path')`
`var filePath = path.join(__dirname, 'xx.txt')`

### http服务
```js
var http = require('http')
var server = http.createServer()
// 监听用户的请求事件(request事件) 回调函数两个行参(request,response)
server.on('request', function(req, res){
  // if(req ???)
  res.setHeader('Content-Type', 'text/html;charset=utf-8');// 服务器设置http响应报文头，告诉浏览器使用响应的编码来解析网页
  res.write('响应内容');
  res.end();// 对于每个请求，服务器必须结束响应，否则客户端会一直等待服务器响应结束
})
// 启动服务
server.listen(8080, function(){
  console.log('服务器启动了')
})
```
#### 回调函数行参-request对象 常用api
`http.IncomingMessage`
- .headers 报文头
- .rawHeaders 原生报文头（和headers的区别是：headers返回`key1:val1,key2:val2...`的对象，而rawHeaders返回`[key1,val1,key2,val2...]`）
- .httpVersion 拿到请求客户端所使用的http协议版本
- .method 客户端请求方式 get、post、delete等
- .url 获取本次请求的路径 不包含主机名称 端口号 域名等

#### 回调函数行参-response对象 常用api
`http.httpServerResponse`
- .setHeader() 设置响应报文头
- .statusCode = 404 设置http响应状态码
- .statusMessage = 'message' 设置http响应状态码对应的消息
- .writeHead() 直接向客户端写入http响应报文头,优先级大于其他所有，如果没写这个，end()时默认执行。如res.writeHead(404, 'not found', {'Content-Type': 'text/html;charset=utf-8'})
- .write() 响应内容
- .end() 通知服务器 所有响应头和响应主体已被发送，服务器将其视为已完成

### NPM & NRM


# Express框架
基于NodeJs的web开发框架
## 介绍
- 实现路由功能 没必要自己写很多if(req.url) else
- 中间件(函数)功能 把监听request事件拆分成了很多方法
- 对req 和 res对象的扩展
- 本身并没有模版引擎，可以集成其他模版引擎


## 路由
1. get/post/put/delete...
```js
/**
 * 通过中间件监听指定的路由的请求 
 * 支持：put/delete/get/post 等http methods
 * 虚拟路径pathname 接受 正则RegExp 匹配
 */
app.get('/', (req, res) => { // 只监听get请求
    res.send('在首面')
    /**
     * send()相当于原生的end();
     * 区别如下：
     * 1. send()自动封装了很多比如setHeader(‘Content-Type’, ‘text/html;charset=utf-8’)等优化
     * 2. end()只接受string or Buffer；send()可以是 string, Buffer, Object or Array
     */
})
app.get(/^\/index(\/.+)*$/, (req,res)=>{
    res.send('在index及子路径下 的 get请求')
})
app.post('/add', (req,res)=>{
    res.send('post add')
})
```
2. use
```javascript
/**
 * mark 当两次相同路由匹配，执行了不同的express.static到不同文件夹下请求回调，则会优先第一次回调结果。逻辑是先找第一个资源 找不到的话再找第二个
 */
const fn = express.static(path.join(__dirname, 'static')) // 处理静态资源的方法，指定静态资源路径
app.use('/', fn) // 实现所有静态资源 托管
/**
 * use 和 以上几种 method 的区别是：
 * 1. 路由匹配时 不限定方法，什么请求方法都可以
 * 2. 请求路径中的第一部分(以/分割)只要与 /index 相等即可，并不要求pathname ===
 */
app.use('/home',(req, res)=>{res.send('在home及子路径下')})
```
3. all
```javascript
/**
 * all
 * 1. 路由匹配时 不限定方法，什么请求方法都可以
 * 2. 请求路径中 pathname 必须 === 完全匹配
 */
app.all('/all',(req, res)=>{})
```
4. paramas
```javascript
/**
 * req.params获取路由中的参数
 * :开头 表示占位符
 * 并且需要严格匹配占位符数量
 */
app.get('/news/:year/:month/:day', (req,res)=>{
    res.send(req.params)
})
```
## res API
```javascript
res.json({a:'1',b: 2}) // 将object或array转为json作为响应发，等同与res.send(json)
res.redirect([状态码默认302], path) // 重定向 封装原生流程：1. 设置状态码res.statusCode = 301或302 2. 设置消息 res.statusMessage = '重定向' 3. 设置相应报文头setHeader('location', 'path') 4. res.end()
res.redirect('https://google.com')
res.sendFile(path, function(err){if(err) throw err}) // 封装原生的 readFile('xx.txt', (err,data)=>{res.end(data)})
res.status(404).end('文件不存在') // 封装原生流程： 1. 设置状态码res.statusCode = 301或302 2. 设置消息 res.statusMessage = '重定向' 3. res.end()
/** 
 * res.render(viewpath模版文件路径 [,locals一个替换模版中占位符key的值val的object] [, callback(err, html)])
 * 需要给express配置一个模版引擎render才能工作，比如jade，ejs，pug, 然后在应用中进行如下设置才能让Express渲染模版文件
 * 1. views 放模版文件的目录，比如 app.set('views', './views')
 * 2. view engine 模版引擎，比如 app.set('view engine', 'ejs')
 * 3. res.render('xx.html', {username: 'x'}, (err,html)=>{})
 */ 
```
## req API

## 拆分封装config & router
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
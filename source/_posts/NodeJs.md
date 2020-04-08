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

`nvm`工具 可以实现在随时切换和管理node版本
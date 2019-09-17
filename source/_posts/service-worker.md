---
title: Service Worker
top: false
cover: false
toc: true
mathjax: true
date: 2019-08-09 10:33:17
tags: ['网络请求','性能优化','缓存']
password:
summary:
categories:
---

# Service Worker

用户对站点的体验期望值越来越高，前端工程师有时候为了几十毫秒的速度优化而费劲心思，消耗大量时间。想要让自己的产品在无数产品中脱颖而出，就必须提升产品的性能和体验。在时间成本高昂的今天，响应速度的提升是开发者不得不面对的话题。

前端工程师有很多性能优化的手段，包括 CDN、CSS Sprite、文件的合并压缩、异步加载、资源缓存等等。其实我们绝大部分情况是在干一件事情，那就是尽量**降低一个页面的网络请求成本从而缩短页面加载资源的时间并降低用户可感知的延时**。当然减少用户可感知的延时也不仅仅是在网络请求成本层面，还有浏览器渲染效率，代码质量等等。

## 简介 - 如何让网页的用户体验做到极致

浏览器中的 javaScript 都是运行在一个单一主线程上的，在同一时间内只能做一件事情。随着 Web 业务不断复杂，我们逐渐在 js 中加了很多耗资源、耗时间的复杂运算过程，这些过程导致的性能问题在 WebApp 的复杂化过程中更加凸显出来。W3C 组织早早的洞察到了这些问题可能会造成的影响，这个时候有个叫 Web Worker 的 API 被造出来了，这个 API 的唯一目的就是解放主线程，Web Worker 是脱离在主线程之外的，将一些复杂的耗时的活交给它干，完成后通过 postMessage 方法告诉主线程，而主线程通过 onMessage 方法得到 Web Worker 的结果反馈。

一切问题好像是解决了，但 Web Worker 是临时的，每次做的事情的结果还不能被持久存下来，如果下次有同样的复杂操作，还得费时间的重新来一遍。那我们能不能有一个Worker 是一直持久存在的，并且随时准备接受主线程的命令呢？基于这样的需求推出了最初版本的 Service Worker ，[Service Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API) 在 Web Worker 的基础上加上了持久离线缓存能力。

目前原生App跟HTML5相比具有如下优势:富离线体验、消息推送、定时默认更行等功能，这些优势决定了HTML5无法取代native。service worker(以下简称sw)就是在这样的背景下提出来的。sw是一段运行在浏览器后端的脚本，独立于页面，是一个worker，也可以理解为一个网络代理服务器。因此sw是无法与DOM进行交互的，但是可以与js主线程进行通信。

## 特点

- 一个独立的 worker 线程，独立于当前网页进程，有自己独立的 worker context。
- 一旦被 install，就永远存在，除非被手动 unregister
- 用到的时候可以直接唤醒，不用的时候自动睡眠
- 可编程拦截代理请求和返回，缓存文件，缓存的文件可以被网页进程取到（包括网络离线状态）
- 离线内容开发者可控
- 能向客户端推送消息
- 不能直接操作 DOM
- 必须在 HTTPS 环境下才能工作
- 异步实现，内部大都是通过 Promise 实现

## 使用示例

[教程来自lavas](https://lavas.baidu.com/pwa/offline-and-cache-loading/service-worker/how-to-use-service-worker)

[lavas视频教程b站](https://www.bilibili.com/video/av36315901/)

```html
<!--  xx.html  -->
<html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Service Worker</title>
    </head>
    <body>
        <script>
            // 为了防止sw.js被浏览器缓存，导致sw延迟
            window.onload = function() {
                var script = document.createElement('script');
                var firstScript = document.getElementsByTagName('script')[0];
                script.type = 'text/javascript';
                script.async = true;
                script.src = '/sw-register.js?v='+Data.now();
                firstScript.parentElement.insertBefore(script, firstScript);
            }
        </script>
        <script>
            // service worker 更新后通知用户 对应sw内的监听activate
            if('serviceWorker' in navigator) {
                navigator.serviceWorker.addEventListener('message', function(e){
                    if(e.data === 'sw.update'){
                        // window.location.reload();
                        alter('更新资源了，请刷新页面')
                    }
                })
            }
        </script>
        <script>
            /* 注册. 在 js 主线程（常规的页面里的 js ）注册 Service Worker 来启动安装，这个过程将会通知浏览器我们的 Service Worker 线程的 javaScript 文件在什么地方呆着 */
            if ('serviceWorker' in navigator) { // 判断 Service Worker API 的可用情况，支持的话咱们才继续谈实现
                window.addEventListener('load', function () {
                    navigator.serviceWorker.register('/sw.js', {scope: '/'}) //scope作用域，如果不指定，默认是sw.js所在文件夹（scope取值必须在sw所在文件夹下的深层路径）
                        .then(function (registration) {// 注册成功
                            console.log('ServiceWorker registration successful with scope: ', registration.scope);
                        })
                        .catch(function (err) {// 注册失败
                            console.log('ServiceWorker registration failed: ', err);
                        });
                });
            }
        </script>
    </body>
</html>
```

```js
/** sw.js **/
/*监听 service worker 的 install 事件*/
this.addEventListener('install', function (event) {// 如果监听到了 service worker 已经安装成功的话，就会调用 event.waitUntil 回调函数
    event.waitUntil(// 安装成功后操作 CacheStorage 缓存，使用之前需要先通过 caches.open() 打开对应缓存空间。
        caches.open('my-test-cache-v1').then(function (cache) {// 通过 cache 缓存对象的 addAll 方法添加 precache 缓存
            cache.add('dog.jpg');
            cache.add('monkey.jpg'); // install 时 填充缓存
            return cache.addAll([
                '/',
                '/index.html',
                '/main.css',
                '/main.js',
                '/image.jpg'
            ]);
        })
    );
});
/* 监听sw激活 */
this.addEventListener('activate', function(event){
    console.log('service worker activate..');
    event.waitUntil(
        caches.keys().then(keys => Promise.all(
            keys.map(key => {
                if(!['sw-demo-precache'].includes(key)){
                    return caches.delete(key)
                }
            })
        ))
        .then(()=>{
            console.log('service worker now ready to handle fetch events');
            return this.clients.matchAll()
            .then(function(clients){
                if(clients && clients.length){
                    clients.forEach((client) => {
                        client.postMessage('sw.update');
                    });
                }
            })
        })
    )
})
/** service worker 更新后通知用户 */
this.addEventListener('activate', function(event){
    caches.open(cacheName)
        .then(function(cache){
            // 老缓存清除
        })
        .then(function(cache){
            return this.clients.matchAll()
            .then(function(clients){
                if(clients && clients.length){
                    clients.forEach(function(client){
                        // 给每个已经打开的标签都postMessage
                        client.postMessage('sw.update');
                    })
                }
            })
        })
})
/*自定义请求响应，每次任何被 Service Worker 控制的资源被请求到时，都会触发 fetch 事件*/
this.addEventListener('fetch', function (event) {
    event.respondWith(
        caches.match(event.request).then(function (response) {// 来来来，代理可以搞一些代理的事情
            if (response) {// 如果 Service Worker 有自己的返回，就直接返回，减少一次 http 请求
                return response;
            }
            // 如果 service worker 没有返回，那就得直接请求真实远程服务
            var request = event.request.clone(); // 把原始请求拷过来
            return fetch(request).then(function (httpRes) {// http请求的返回已被抓到，可以处置了。
                if (!httpRes || httpRes.status !== 200) {// 请求失败了，直接返回失败的结果就好了。。
                    return httpRes;
                }
                var responseClone = httpRes.clone();// 请求成功的话，将请求缓存起来。
                caches.open('my-test-cache-v1').then(function (cache) {
                    cache.put(event.request, responseClone);
                });
                return httpRes;
            });
        })
    );
});
```

```js
/** sw-register.js **/
if ('serviceWorker' in navigator) {
    window.addEventListener('load', function () {
        navigator.serviceWorker.getRegistrations().then(function(regs){
            for(var reg of regs) {
                reg.unregister();
            }
        }) // 在注册前清空所有已注册的sw，防止作用域污染
        navigator.serviceWorker.register('/sw.js', {scope: '/'})
            .then(function (registration) {
                console.log('ServiceWorker registration successful with scope: ', registration.scope);
            })
            .catch(function (err) {
                console.log('ServiceWorker registration failed: ', err);
            });
        this.navigator.serviceWorker.addEventListener('message', function(e){
            if(e.data && e.data === 'sw.update') {
                window.location.reload();
            }
        })
    });
}
```

## 生命周期

教程来自[github.com/Leslie2014](https://github.com/Leslie2014/blog/issues/1)

![img](https://camo.githubusercontent.com/c3bff6403d8a948370384b5a3e5586a93ad4e9b5/68747470733a2f2f696d672e616c6963646e2e636f6d2f7466732f54423130795578534658585858586b5858585858585858585858582d3730322d3638352e706e67)


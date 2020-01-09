---
title: 杂谈-js-跨越请求jsonp
top: false
cover: false
toc: true
mathjax: false
date: 2020-01-09 19:22:48
tags:
password:
summary:
categories:
---

```js
const jsonp = (url) => {
    if (!url) {
      console.error('Axios.JSONP 至少需要一个url参数!');
      return;
    }
    return new Promise((resolve, reject) => {
      const JSONP = document.createElement('script');
      JSONP.type = 'text/javascript';
      JSONP.src = `${url}`;
      document.getElementsByTagName('head')[0].appendChild(JSONP);
    });
};
jsonp('http://192.168.0.11:8079/HWCollaborationDashboard/passport/removeFreeLogin');
```
> [CORB警告](https://juejin.im/post/5cc2e3ecf265da03904c1e06#heading-19)
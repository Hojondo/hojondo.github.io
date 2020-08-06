---
title: 项目介绍 洗衣管家
top: false
cover: false
toc: true
mathjax: false
date: 2020-03-22 00:13:00
tags:
password:
summary:
categories: ['项目经验']
---

# 洗衣管家
> [线上Mock版 预览地址](/about/wash) *无需输入表单*

## 业务简介
本系统 服务于产品-洗衣管家-致力于实现线上线下提供一站式洗衣服务的终端系统

## 技术栈
- `Vue`前端开发框架.vuex、vue-router
- `element`UI库
- `scss`css预处理
- `eslint`recommended
- `mockJs`拦截Ajax请求 生成随机数据
- `axios`promis库 前后端对接
- `vue-cli`脚手架

## 项目目录
```shell
    |-dist*
    |-public                    # 静态文件
    |-src
        |-main.js               # webpack 的入口文件；
        |-App.vue               # 根组件
        |-app                   # 存放项目业务代码
            |-...
        |-api                   # 存放api请求(文件名与模型名称基本一致,文件名使用小驼峰, 方法名称与后端restful控制器一致)
        |-components            # 存放项目共用的组件,通常是一些可复用的组件会单独存放在该目录
        |-assets                # 存放项目共用的代码以外的资源，如：图片、图标、视频、字体 等
            |-css
            |-img
        |-router                # 存放前端路由相关配置
        |-store                 # vuex的目录
        |-layout                # 整体布局
        |-util                  # 第三方静态引用库
    |-package.json              # npm包配置文件，里面定义了项目的npm脚本，依赖包等信息
    |-README.md                 # 项目说明文件
    |-babel.config.js           # babel 的配置文件
    |-vue.config.js             # vue-cli的额外自定配置文件
```

## 页面组件嵌套逻辑
![思维导图](http://assets.processon.com/chart_image/5e76f366e4b092510f6b12e0.png)

## 重点Mark
### 自定义通用组件Tabs/TabPane
```html
// CustomTabs.vue
<template>
    <div class="clothsort">
      <ul><!--标题页的标题 v-for遍历, :class 动态绑定class-->
        <li
          :class="tabCls(item)"
          v-for="(item,index) in navList"
          :key="index"
          @click="handleChange(index)"
        >{{item.label}}</li>
      </ul>
    </div>
    <div class="cloth_con">
      <slot></slot><!--这里的slot就是嵌套的pane组件的内容-->
    </div>
</template>
<script>
props: {
    value: {
      type: [String]
    }
},
data: {
    navList,
    currentValue,
},
watch() {
    value(val) {
      this.currentValue = val;
    },
    currentValue() {
      this.updateStatus();//tab发生变化时，更新pane的显示状态
    }
},
mounted() {
    updateNav();
},
methods: {
    updateNav() {
        this.$children.forEach((pane, index)=>{
            _this.navList.push({
            label: pane.label,
            name: pane.name || index
            });
        });
        this.$children.forEach((tab)=>{
            return (tab.show = tab.name === _this.currentValue);
        });
    },
    handleChangeTab(index) {
        _this.currentValue = name;//改变当前选中的tab，触发watch
        _this.$emit("input", name);//实现子组件与父组件通信
    }
}
</script>

```
```html
// CustomPane.vue
<template>
  <div class="pane" v-show="show">
    <slot></slot>
  </div>
</template>
<script>
props: {
    //设置pane的标识
    name: {
      type: String
    },
    //label是设置标题
    label: {
      type: String,
      default: ""
    }
},
data: {
    show: true
}
</script>
```
### 自定义通用组件 Modal
```html
<template>
  <div v-if="show" style="z-index: 999999">
    <div class="mask" @click="$emit('close')"></div>
    <div class="main">
      <i class="icon icon-close" @click="$emit('close')"></i>
      <slot></slot>
      <div class="pop-footer"><slot name="footer"></slot></div>
    </div>
  </div>
</template>
<script>
    props: {
        show: {
            type: Boolean,
            required: true
        }
  },
</script>
```
### 自定义通用组件 Alert
```html
// CustomAlert.vue
<template>
  <div class="customAlert">
    <div class="box">
      {{ message }}
    </div>
  </div>
</template>
<script>
    data: {
      message: ''
    }
</script>
```
```javascript
// CustomAlert.js
import Vue from 'vue'
import Alert from './customAlert.vue'

const AlertBox = Vue.extend(Alert);
Alert.install = function (data) {
  let instance = new AlertBox({
    data
  }).$mount();
  document.body.appendChild(instance.$el);
  setTimeout(() => document.body.removeChild(instance.$el), 1000)
};

export default Popup
```
```javascript
// main.js
import CustomAlert from '_components/customAlert.js'
Vue.prototype.$customAlert = CustomAlert.install;
```

### 调用H5 navigator API 打开摄像头和canvas截图
```javascript
  function getCompetence () {
      const _this = this;
      this.thisCanvas = document.getElementById('canvasCamera')
      this.thisContext = this.thisCanvas.getContext('2d')
      this.thisVideo = document.getElementById('videoCamera')
      // 旧版本浏览器可能根本不支持mediaDevices，我们首先设置一个空对象
      if (navigator.mediaDevices === undefined) {
        navigator.mediaDevices = {}
      }
      // 一些浏览器实现了部分mediaDevices，我们不能只分配一个对象
      // 使用getUserMedia，因为它会覆盖现有的属性。
      // 这里，如果缺少getUserMedia属性，就添加它。
      if (navigator.mediaDevices.getUserMedia === undefined) {
        navigator.mediaDevices.getUserMedia = function (constraints) {
          // 首先获取现存的getUserMedia(如果存在)
          var getUserMedia = navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.getUserMedia
          // 有些浏览器不支持，会返回错误信息
          // 保持接口一致
          if (!getUserMedia) {
            return Promise.reject(new Error('getUserMedia is not implemented in this browser'))
          }
          // 否则，使用Promise将调用包装到旧的navigator.getUserMedia
          return new Promise(function (resolve, reject) {
            getUserMedia.call(navigator, constraints, resolve, reject)
          })
        }
      }
      setTimeout(()=>{
        var constraints = { audio: false, video: { width: this.$refs.videoCamera.clientWidth, height: this.$refs.videoCamera.clientHeight } }
        navigator.mediaDevices.getUserMedia(constraints).then(function (stream) {
          // 旧的浏览器可能没有srcObject
          if ('srcObject' in _this.thisVideo) {
            _this.thisVideo.srcObject = stream
          } else {
            // 避免在新的浏览器中使用它，因为它正在被弃用。
            _this.thisVideo.src = window.URL.createObjectURL(stream)
          }
          _this.thisVideo.onloadedmetadata = function (e) {
            _this.thisVideo.play()
          }
        }).catch(err => {
          console.log(err)
        })
      },100)
    }
    function stopNavigator () { // 关闭摄像头
      this.thisVideo.srcObject.getTracks()[0].stop();
    }
```
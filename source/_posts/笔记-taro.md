---
title: Taro多端统一开发解决方案
top: false
cover: false
toc: true
mathjax: false
date: 2020-03-02 21:46:25
tags: ["小程序开发"]
password:
summary:
categories: ["笔记"]
---

# [Taro](https://taro.aotu.io/)

## 简介

使用 Taro，我们可以只书写一套代码，再通过 Taro 的编译工具，将源代码分别编译出可以在不同端（微信/百度/支付宝/字节跳动/QQ/京东小程序、快应用、H5、React-Native 等）运行的代码
[小程序框架评测](https://aotu.io/notes/2019/03/12/mini-program-framework-full-review/index.html)

- React 语法风格,采用与 React 一致的组件化思想，组件生命周期与 React 保持一致，同时支持使用 JSX 语法
- 支持 `npm/yarn` 安装管理第三方依赖,`ES7/ES8`,`CSS 预编译器`,`Redux/MobX状态管理`,小程序 API 优化,异步 API Promise 化
- 支持多端开发转化到 微信/百度/支付宝/字节跳动/QQ/京东小程序 、快应用、 H5 端 以及 移动端（React Native）

## 规范架构

```js
├── config                 配置目录
|   ├── dev.js             开发时配置
|   ├── index.js           默认配置
|   └── prod.js            打包时配置
├── src                    源码目录
|   ├── components         公共组件目录
|   ├── pages              页面文件目录
|   |   ├── index          index 页面目录
|   |   |   ├── banner     页面 index 私有组件
|   |   |   ├── index.js   index 页面逻辑
|   |   |   └── index.css  index 页面样式
|   ├── utils              公共方法库
|   ├── app.css            项目总通用样式
|   └── app.js             项目入口文件
└── package.json
```

## 框架

### 入口文件

入口文件默认是 `src` 目录下的 `app.js`
通常入口文件会包含一个`config`配置项，这个配置是整个应用的全局的配置，配置规范基于微信小程序的全局配置进行制定，所有平台进行统一。入口文件中的[全局配置](https://taro-docs.jd.com/taro/docs/tutorial.html#%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE)，在编译后将生成全局配置文件`app.json`，其中`page`属性必填

#### 生命周期

[微信小程序和 taro - 生命周期的对应关系](https://taro-docs.jd.com/taro/docs/taroize.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)

- `componentWillMount()`_在微信/百度/字节跳动/支付宝小程序中这一生命周期方法对应 app 的 onLaunch,监听程序初始化，初始化完成时触发（全局只触发一次）_
- `componentDidMount()`_视为等同 componentWillMount,在 componentWillMount 后执行_
- `componentDidShow()`_在微信/百度/字节跳动/支付宝小程序中这一生命周期方法对应 onShow，在 H5/RN 中同步实现_
- `componentDidHide()`_在微信/百度/字节跳动/支付宝小程序中这一生命周期方法对应 onHide，在 H5/RN 中同步实现_
- `componentDidCatchError(String error)`_在微信/百度/字节跳动/支付宝小程序中这一生命周期方法对应 onError，H5/RN 中尚未实现_
- `componentDidNotFound(Object)`_在微信/字节跳动小程序中这一生命周期方法对应 onPageNotFound_

#### Taro.getApp(Object)

可以用来获取到程序 App 实例，在各个端均有实现

### 页面

在项目入口文件 app.js 中 config 的 pages 数组中指定所有页面

#### 页面配置

页面的配置只能设置 全局配置 中部分 window 配置项的内容，页面中配置项会覆盖 全局配置 的 window 中相同的配置项

#### 样式

页面的样式文件建议放在与页面 JS 的同级目录下，然后通过 ES6 规范 import 进行引入，支持使用 CSS 预编译处理器，另可能是由于转编译后每个页面分为独立组件，import css 后不需要再考虑作用域的问题。

#### 生命周期

每个生命周期函数中，都可以通过 `this.$router.params` 获取打开当前页面路径中的参数

- `componentWillMount()`_页面加载时触发，一个页面只会调用一次，此时页面 DOM 尚未准备好，还不能和视图层进行交互_
- `componentDidMount()`_页面初次渲染完成时触发，一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互_
- `shouldComponentUpdate(nextProps, nextState)`_页面是否需要更新，返回 false 不继续更新，否则继续走更新流程_
- `componentWillUpdate(nextProps, nextState)`_页面即将更新_
- `componentDidUpdate(prevProps, prevState)`_页面更新完毕_
- `componentWillUnmount()`_页面卸载时触发，如 redirectTo 或 navigateBack 到其他页面时_
- `componentDidShow()`_页面显示/切入前台时触发_
- `componentDidHide()`_页面隐藏/切入后台时触发， 如 navigateTo 或底部 tab 切换到其他页面，小程序切入后台等_

---

- `onPullDownRefresh()`_监听用户下拉刷新事件_
- `onReachBottom()`_监听用户上拉触底事件_
- `onPageScroll(Object)`_监听用户滑动页面事件_
- `onShareAppMessage(Object)`_监听用户点击页面内转发按钮（Button 组件 openType='share'）或右上角菜单“转发”按钮的行为，并自定义转发内容;只有定义了此事件处理函数，右上角菜单才会显示“转发”按钮_
- `onTabItemTap(Object)`_当前是 tab 页时，点击 tab 时触发_
- `onResize(object)`_Only 微信小程序,屏幕旋转时触发_
- `componentWillPreload()`_Only 微信小程序，预加载钩子_
- `onTitleClick()`_Only 支付宝小程序,点击标题触发_
- `onOptionMenuClick()`_only 支付宝小程序,点击导航栏额外图标触发_
- `onPopMenuClick()`_only 支付宝小程序,点击右上角通用菜单中的自定义菜单按钮触发_
- `onPullIntercept()`_only 支付宝小程序,下拉截断时触发_

### 组件

建议放在 src 下的 components 目录中。一个组件通常包含组件 JS 文件以及组件样式文件，组织方式与页面类似。
一般来说，Taro 组件不需要任何配置，但当你在 Taro 组件中引用原生小程序组件代码时，则需要通过配置 `config.usingComponents{组件名: 组件相对路径}` 来实现。
其生命周期如下：

- `componentWillMount()`_组件加载时触发，一个组件只会调用一次，此时组件 DOM 尚未准备好，还不能和视图层进行交互_
- `componentDidMount()`_组件初次渲染完成时触发，一个组件只会调用一次，代表组件已经准备妥当，可以和视图层进行交互_
- `componentWillReceiveProps(nextProps)`_已经装载的组件接收到新属性前调用_
- `shouldComponentUpdate(nextProps, nextState)`_组件是否需要更新，返回 false 不继续更新，否则继续走更新流程_
- `componentWillUpdate(nextProps, nextState)`_组件即将更新_
- `componentDidUpdate(prevProps, prevState)`_组件更新完毕_
- `componentWillUnmount()`_组件卸载时触发_

## 路由

`navigateTo(params)`[打开新页面](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateTo.html)
`redirectTo(params)`[在当前页面打开](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.redirectTo.html)

### 路由传参

```javascript
// A页面：传入参数 id=2&type=test
Taro.navigateTo({
  url: "/pages/page/path/name?id=2&type=test",
});
// B页面：
class B extends Taro.Component {
  componentWillMount() {
    console.log(this.$router.params); // 输出 { id: 2, type: 'test' }
  }
}
```

## 设计稿和尺寸单位

在 Taro 中尺寸单位建议使用 px、 百分比 %，Taro 默认会对所有单位进行转换。当转成微信小程序的时候，尺寸将默认转换为 100rpx，当转成 H5 时将默认转换为以 rem 为单位的值。
Taro 默认以 750px 作为换算尺寸标准，如果设计稿不是以 750px 为标准，则需要在项目配置 config/index.js 中进行设置，例如设计稿尺寸是 640px，则需要修改项目配置 `config/index.js` 中的 `designWidth` 配置为 640
建议使用 Taro 时，设计稿以 iPhone 6 750px 作为设计尺寸标准

## 静态资源引用

- js 中通过`import`引用样式 css/scss、js、图片音频字体文件、JSON 文件
- 样式中引用本地资源：Taro 提供了直接在样式文件中引用本地资源的方式，其原理是通过 PostCSS 的 postcss-url 插件将样式中本地资源引用转换成 Base64 格式， 默认会对 10kb 大小以下的资源进行转换

## 组件引用外部样式

[详情](https://nervjs.github.io/taro/docs/component-style.html)
自定义组件对应的样式文件，只对该组件内的节点生效；
组件可以指定它所在节点的默认样式，使用 :host 选择器；
如果想传递样式给引用的自定义组件，需要利用 externalClasses 定义段定义若干个外部样式类；
如果希望组件外样式类能够完全影响组件内部，可以将组件构造器中的 options.addGlobalClass 字段置为 true；

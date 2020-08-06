---
title: Bootstrap v4
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-19 19:37:00
tags: ["js", "UI库"]
password:
summary:
categories:
---

# 前言

_当前笔记为 v4.1 版本_

主要 css 文件 `bootstrap.css`,自定义更改常量在 **\_variable.scss**中

某些组件效果是基于/依次引用 `jQuery`和`popper.js`和`bootstrap.js` _(bootstrap.bundle.js 和 bootstrap.bundle.min.js 中打包了 popper.js，不包含 jQuery.js)_

### HTML 模板实例

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no"
    />
    <!--响应式meta-->

    <!-- Bootstrap CSS -->
    <link
      rel="stylesheet"
      href="https://cdn.bootcss.com/bootstrap/4.0.0/css/bootstrap.min.css"
      integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm"
      crossorigin="anonymous"
    />

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script
      src="https://cdn.bootcss.com/jquery/3.2.1/jquery.slim.min.js"
      integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN"
      crossorigin="anonymous"
    ></script>
    <script
      src="https://cdn.bootcss.com/popper.js/1.12.9/umd/popper.min.js"
      integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q"
      crossorigin="anonymous"
    ></script>
    <script
      src="https://cdn.bootcss.com/bootstrap/4.0.0/js/bootstrap.min.js"
      integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

# Layout 排版

## 排版的三个主要轴：container/container-fluid, media queries, z-index

> ### 容器 container/container-fluid
>
> 容器是 Bootstrap 最基本的排版元素，且  **当使用我们的网格系统时**  是必须的。从响应式、固定宽度容器（表示其最大宽度限制在每一个中断点）或可变宽度（显示为 100% 宽）中选择
>
> ### 响应式断点 media queries
>
> 由于 Bootstrap 是被开发来作行动优先，我们使用许多  [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)  建立灵敏的中断点用于我们的排版和介面。这些中断点大部分基于最小 viewport，并且允许我们随着 viewport 的变化放大组件。
>
> > 一般是以下几个尺寸
> >
> > ```css
> > @media (min-width: 576px) {
> >   ...;
> > } // Small devices (landscape phones, 576px and up)
> > @media (min-width: 768px) {
> >   ...;
> > } // Medium devices (tablets, 768px and up)
> > @media (min-width: 992px) {
> >   ...;
> > } // Large devices (desktops, 992px and up)
> > @media (min-width: 1200px) {
> >   ...;
> > } // Extra large devices (large desktops, 1200px and up)
> > ```
> >
> > ```css
> > @media (max-width: 575.98px) {
> >   ...;
> > } // Extra small devices (portrait phones, less than 576px)
> > @media (min-width: 576px) and (max-width: 767.98px) {
> >   ...;
> > } // Small devices (landscape phones, 576px and up)
> > @media (min-width: 768px) and (max-width: 991.98px) {
> >   ...;
> > } // Medium devices (tablets, 768px and up)
> > @media (min-width: 992px) and (max-width: 1199.98px) {
> >   ...;
> > } // Large devices (desktops, 992px and up)
> > @media (min-width: 1200px) {
> >   ...;
> > } // Extra large devices (large desktops, 1200px and up)
> > ```
>
> ### Z-index
>
> 一些 Bootstrap 元件使用  `z-index` 校正图层导引、工具提示和 popover、modals  ， 这些偏高的数值，具体的目的是为了避免冲突，需要再不同的分层组建区分层级，如 工具提示、导览列、下拉选单、互动视窗的行为正确，没有理由不使用  `100`+ 或  `500`+。
>
> > 如下
> >
> > ```css
> > $zindex-dropdown: 1000 !default;
> > $zindex-sticky: 1020 !default;
> > $zindex-fixed: 1030 !default;
> > $zindex-modal-backdrop: 1040 !default;
> > $zindex-modal: 1050 !default;
> > $zindex-popover: 1060 !default;
> > $zindex-tooltip: 1070 !default;
> > ```

# Content 内容

## 重置 reboot.css

重置的原因和规范：

- 更新部分浏览器的预设值，在可变动的文字间距上使用 `rem`s 替代 `em`s。
- 避免 `margin-top`。垂直边缘可能会发生重叠，产生无法预料的错误。更重要的是 `margin` 应该是单向、简单的思维。
- 为了在设备之间之间轻松缩放，方块元素应当在 `margin` 上采用 `rem`。
- 尽可能使用 `inherit` 将字体的属性宣告保持在最小化。

### 1.页面预设

> - 在每个元素上设定全域性的  `box-sizing`，包括  `*::before`  和  `*::after`  以及  `border-box`。
> - `<body>` 同时设定一个全域的 `font-family` 和 `line-height` 及 `text-align`，随后某些元素形式会继承这个设定以防止字体不一致。
> - 安全起见在 `<body>` 宣告 `background-color` 预设值为 `#fff`。

### 2.原生字体堆叠

> 放弃了预设网页字体（Helvetica Neue, Helvetica, 和 Arial）并用 “native font stack” 取代了预设字体以在每个设备和作业系统上获得最佳的阅读呈现

### 3.标题和段落

> 所有标题元素像是  `<h1>`  及  `<p>`  已经删除它们的  `margin-top`。标题元素具有  `margin-bottom: .5rem`，段落元素则是  `margin-bottom: 1rem`  使其具有更单纯的间隔。

### 4.列表

> - 删除全部列表  `<ul>`、`<ol>`  和  `<dl>`  中的  `margin-top`，并设定为  `margin-bottom: 1rem`。巢状列表没有  `margin-bottom`。
> - 为了更简单的样式、明确和更好的间隔，说明清单具有更新后的  `margin`、`<dd>`  重设  `margin-left`  为  `0`  并增加  `margin-bottom: .5rem`。`<dt>`  为  **粗体**。

### 5.代码块

> `<pre>`  元素被重设以删除其  `margin-top`  并在  `margin-bottom`  上使用  `rem`。

### 6.表格

> 表格经过轻微调整以将并  `<caption>`  风格化、合并边框并确保整体的  `text-align`。在[the `.table` class](https://bootstrap.hexschool.com/docs/4.1/content/tables/)  中有针对 borders、padding 和更多的额外变化 。

### 7.表单

> - `<fieldset>` 没有 borders、padding 或 margin 以便包覆独立 input 和成组 inputs。
> - `<legend>` 和 fieldsets 一样，`<legend>` 已经被重新定义样式以便显示为类型的标题。
> - `<label>` 被设定为`display: inline-block` 以便让 `margin` 应用。
> - 透过 Normalize 于 `<input>`、`<select>`、`<textarea>`、 和 `<button>`，重置删除了他们的 `margin` 并同样设定 `line-height: inherit`。
> - 将 `<textarea>` 修改为仅可调整垂直尺寸，因为调整水平宽度通常 “破坏” 了页面配置。

### 8.其他

> #### 8.1 地址
>
> > 更新了  `<address>`  元素以便将浏览器的预设  `font-style`  由`italic`  重置为  `normal`。同时现在继承了  `line-height`  并添加了  `margin-bottom: 1rem`。`<address>`  用于提供联系资讯。透过  `<br>`  来换一行。
>
> #### 8.2 长引用 blockquote
>
> > Blockquotes 的预设  `margin`  为  `1em 40px`，因此我们将其重新设定为  `0 0 1rem`  以便更符合其他元素的设定。
>
> #### 8.3 行内元素
>
> > `<abbr>`  元素接受基本样式以便在段落文字之间突出显示。
>
> #### 8.4 摘要
>
> > 预设游标在摘要上鼠标样式是  `text`，所以我们将其重置为  `pointer`，在界面上了解元素可以点击产生互动。
>
> #### 8.5hidden 属性
>
> > HTML 添加了  [一个名为  `[hidden]`  的新全域属性]，这是属性的预设格式是  `display: none`。借鉴了  [PureCSS](https://purecss.io/)  的一个想法，强制加入  `[hidden] { display: none !important; }`  改善了预设设定，以防止该属性的  `display`  被意外覆盖。兼容 IE10 支援原生  `[hidden]`。

# Components 组件/元件

## 滚动监控 [data-spy="scroll"]

- 基本结构：

  > - `.navbar#id`
  >   - `.navbar-brand`
  >   - `.nav>`
  >     - `.nav-item a[href="#id0"]`
  >     - `*`
  >       - `a[href="#id0-0"]` _嵌套 父层 a 也会是.active_
  >       - `a[href="#id0-1"]`
  >     - `.nav-item a[href="#id1"]`
  > - `div[data-spy="scroll"][data-target="#id"][data-offset="默认10(偏移值)"]`
  >   - `*#id0`
  >     - `*#id0-0`
  >     - `*#id0-1`
  >   - `*#id1`

- 自定义选项：

  - 将 `data-spy="scroll"` 加到要侦听的元素，或`$().scrollspy({ target: '#id' })`

- 方法：

  - `$().scrollspy('refresh')` 当从 DOM 加入或删除元素使用滚动监控时，需要调用刷新方式
  - `$().scrollspy('dispose')` 销毁 JS

- 事件：

  - `$('[data-spy="scroll"]').on('activate.bs.scrollspy', function(){每当一个项目被启用时，这个事件会在滚动元素上触发})`

## 导航栏.nav

- 基本结构：

  > _flex 布局_
  >
  > - `.nav` & `.justify-content-{start/center/end}(水平对齐方式) || .flex-column/.felx-column-reverse(垂直布局)` & `.nav-fill>.nav-item(flex:1 1 auto;自动填充满按比例) || .nav-justified>.nav-item(flex-basis:0;flex-grow:1;自动填满且相同宽度)` & `.nav-tabs>.nav-item>[data-target="#xx"] || .nav-pills>[data-target="#xx"](tab组件)`
  >   - `.nav-item` `.nav-link`
  >     - `.nav-link`
  > - `.tab-content>.tab-pane`

- 自定义选项：

  - `.nav`内包含`.nav-item.dropdown>.dropdown-toggle+.dropdown-menu`
  - `.nav>.nav-item/nav-link[data-toggle="tab"/"pill"][href/data-target="#xxx"]` & `.tab-content>.tab-pane#xxx` **同.list-group-item[data-toggle="list"]**

- 方法：

  - `$('.nav-item a/.nav-link').tab("show")`
  - `$('.tab-pane').tab("dispose")` 销毁 tab-panel

- 事件：

  - `$().on('show.bs.tab', function (e) {在分页标签显示之前。 event.target要显示的分页 和 event.relatedTarget上一个启用分页标签（如果可用）})`
  - `$().on('shown.bs.tab', function (e) {在一个分页标签显示之后。 event.target要显示的分页 和 event.relatedTarget上一个启用分页标签（如果可用）})`
  - `$().on('hide.bs.tab', function (e) {显示新分页标签之后。 event.target上一个启用分页标签 和 event.relatedTarget要显示的分页（如果有）})`
  - `$('.tab-pane').on('hidden.bs.tab', function (e) {在显示新分页标签之后。 event.target上一个启用分页标签 和 event.relatedTarget要显示的分页（如果有）})`

## 导航栏条 .navbar

- 基本结构：

  > display:flex;justify-content: space-between
  >
  > - `.navbar` & `navbar-expand{-sm//md/lg/xl}(响应式宽度,即自定义多宽开始是flex-direction:row;)` & `.navbar-light/.navbar-dark .bg-{primary/success/..}颜色` & `.fixed-top/sticky-top/fixed-bottom定位`
  >   - [.container/.container-fluid]
  >     - `.navbar-brand` _display:inline-block;margin-right:1rem;font-size:1.25rem_
  >     - `button.navbar-toggler[data-toggle="collapse"][data-target="#xx"]>span.navbar-toggler-icon` _折叠菜单按钮_
  >     - `.collapse.navbar-collapse#xx` 折叠菜单 (_navbar-collapse{flex-basis:100%;flex-grow:1;} min-width: ?px:{display: flex !important;} .collapse:not(.show){display:none;} .show{display:block}_)
  >       - `ul.navbar-nav` (_flex-direction:column; min-width:?px:{flex-direction:row}_)
  >         - `li.navbar-item` & `.dropdown`
  >           - `.dropdown-toggle`
  >           - `.dropdown-menu>.dropdown-item+.dropdown-divider`
  >       - `form.form-inline`
  >       - `.navbar-text` _inline-block_

## 面包屑 .breadcrumb > .breadcrumb-item.active

在导航结构中透过 CSS 自动添加分隔符号指示当前页面的位置[_面包屑 jsfiddle 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275859/)

- 基本结构：
  - `ol.breadcrumb`
    - `li.breadcrumb-item`
- 自定义选项：
  - 改变分隔符在`_variable_scss`中：例`$breadcrumb-divider: quote(">");` 或 `$breadcrumb-divider: url(data:image/svg+xml;base64,某base64url);` 或不要分隔符`$breadcrumb-divider: none;`

## 分页 .pagination

- 基本结构：

  > - `.pagination` & `.pagination-sm/lg 大小` & `.justify-content-{center/start/end/between/around} 对齐` _display:flex;list-style:none;_
  >   - `.page-item` `.page-link` & `[disabled/active]`
  >     - `.page-link`
  >       - `<span>左箭头&laquo;/右箭头&raquo;</span>`

## ---

## 轮播 .carousel

- 基本结构：

  > - `.carousel.slide#crsl`
  >   - `ol.carousel-indicators`
  >     - `li.active[data-target='#crsl' data-slide-to='0']`
  >     - `li[data-target='#crsl' data-slide-to='1']`
  >     - `li[data-target='#crsl' data-slide-to='2']`
  >   - `.carousel-inner`
  >     - `.carousel-item.active > img+.carousel-caption`
  >     - `.carousel-item > img+.carousel-caption`
  >     - `.carousel-item > img+.carousel-caption`
  >   - `a.carousel-control-prev[href='#crsl' data-slide='prev'] > span.carousel-control-prev-icon`
  >   - `a.carousel-control-next[href='#crsl' data-slide='next'] > span.carousel-control-next-icon`

- 交叉淡入淡出 切换效果：`.carousel.carousel-fade`

- 自定义选项：

  - `data-interval=5000!default` ：number | 自动回圈之间延迟的时间 ;如果是 false 不会自动重播
  - `data-keyboard=true!default` ：boolean | 是否对键盘事件有反应
  - `data-pause='hover!default'` ： string / boolean | 鼠标 Mouseover 时暂停滚动；false 不会暂停
  - `data-ride=false!default` ： string | 默认用户手动播放第一次后自动播放；='carousel'自动播放
  - `data-wrap=true!default` ： boolean | 轮播是否连续循环或停止

- 方法：

  - `$().carousel({键值对})` ： 透过 boject 设定并开始执行轮播
  - `$().carousel('cycle')` ： 从左到右循环播放
  - `$().carousel('pause')`： 将物件的循环从轮播中停止
  - `$().carousel('number')`：将轮播指向到特定的影格（基于 0，类似于阵列）。 **在目标项目被显示之前回传给调用者**（在发生 `slid.bs.carousel` 事件之前）
  - `$().carousel('prev')`：将轮播指向前一个物件。**在前一个物件显示前回传给调用者** （在发生 `slid.bs.carousel` 事件之前）
  - `$().carousel('next')`：将轮播指向下一个物件。**在前一个物件显示前回传给调用者** （在发生 `slid.bs.carousel` 事件之前）
  - `$().carousel('dispose')`：销毁一个元素的轮播实例 carousel object instance

- 事件：

  - `$().on('slide.bs.carousel', function () {当调用 slide方法时})`

  - `$().on('slid.bs.carousel', function () {轮播完成切换后})`

    两个事件都具有此 4 个附加属性：

    - `direction`：轮播滑动的方向（`"left"` 或 `"right"`）。
    - `relatedTarget`：被作为启用的物件滑动到指定 DOM 元素。
    - `from`：当前物件的索引
    - `to`：下一个物件的索引

- sass 改变速率等：`$carousel-transition:transform .6s ease !default;`

## 超大屏幕 .jumbotron

- 基本结构：

  > - `div.jumbotron` _圆角 padding:4rem 2rem;margin-bottom:2rem;_
  >   - `*`

  > - `div.jumbotron.jumbotron-fluid` _没有圆角 没有左右 padding_
  >   - `div.container/.container-fluid`
  >     - `*`

## 卡片 .card

- 基本结构：

  > - `.card-group`卡片组-无间距 / `.card-deck`卡片叠-有间距 / `.card-colums`卡片栏-竖直(左上到右下)排列*column-count: 3*
  >   - `.card`
  >     - `.card-img` : `.card-img-top`顶图-上圆角 / `.card-img-bottom`底图-下圆角 `div:not(.card-img).card-img-overlay`图片覆盖为背景
  >     - `.card-header` : `card-header>.card-header-tabs` `card-header>.card-header-pills`
  >     - `*`
  >     - `.card-body`
  >       - `.card-title`
  >       - `.card-subtitle`
  >       - `.card-text`
  >       - `.card-link`
  >       - `*`
  >     - `.card-footer`

## 媒体物件 .media > \* + .media-body

- 基本结构：

  > - `.media`
  >   - `*`
  >   - `.media-body`
  >
  > ```css
  > .media {
  >   display: -ms-flexbox;
  >   display: flex;
  >   -ms-flex-align: start;
  >   align-items: flex-start;
  > }
  > .media-body {
  >   -ms-flex: 1;
  >   flex: 1;
  > }
  > ```

- 自定义选项：

  - `.media>*+.media-body>*+.media>*+.media-body` ：巢状嵌套
  - `.align-self-end` `.justify-content-center` ：对齐
  - `.media>*.order-{breakpoint}-{n}` ：自定义排序
  - `ul.list-unstyled>li.media` ：对于 ul>li 结构 增加  ul.list-unstyled，以移除任何浏览器预设清单样式

## ---

## 折叠 [data-toggle="collapse"]

- 基本结构：

  > - `*[data-toggle="collapse" data-target="#ID/.multi-Class"]` / `a[data-toggle="collapse" href="#ID/.multi-Class"]` _会有.collapsed 第一次收起时被 js 调用添加 class_mark_
  > - `*#ID.collapse(/collapsing/collapse+show) > *` _默认给个 collapse 初始收起状态_

- 兼容 Screenreaders： `[aira-expanded="false/true"]`

- 自定义选项：

  - `data-parent=false!default` css 选择器/DOM 元素 | 如果提供了父层，则当显示此可折叠物件时，指定父项下的所有可折叠元素将被关闭。在 collapseContent 设置。
  - `data-toggle=true!default` boolean | 切换可折叠元素

- 方法：

  - `$().collapse({键值对})`
  - `$().collapse('toggle')` ：将可折叠元素切换为显示或隐藏。 **在可折叠元素实际显示或隐藏之前**（即发生 `shown.bs.collapse` 或 `hidden.bs.collapse` 事件之前）返回到调用者。
  - `$().collapse('show')`：显示可折叠的元素。 **在可折叠元素实际显示之前**（即在 `shown.bs.collapse` 事件发生之前）返回到调用者。
  - `$().collapse('hide')`：隐藏可折叠的元素。 **在可折叠元素实际上被隐藏之前返回给调用者**（即在 `hidden.bs.collapse` 事件发生之前）。
  - `$().collapse('dispose')`：销毁一个元素的折叠。

- 事件：

  - `$().on('show.bs.collapse',function(){当调用 show 方法时}`
  - `$().on('shown.bs.collapse',function(){当使用者可见折叠元素时,等待CSS转换完成后}`
  - `$().on('hide.bs.collapse',function(){当调用 hide 方式时}`
  - `$().on('hidden.bs.collapse',function(){对使用者隐藏了一个折叠元素时,等待CSS 转换完成后}`

## 下拉选单 .dropdown>.dropdown-toggle[data-toggle='dropdown'] + .dropdown-meau>.dropdown-item

- 基本结构：

  > - `.dropdown/.dropup/.dropleft/.dropright` //方向非固定，不是此 class 的默认 dropdown，重点是 position：relative;
  >   - `.dropdown-toggle[data-toggle='dropdown']` 或 `* + .dropdown-toggle.dropdown-toggle-split[data-toggle="dropdown"]`
  >   - `div.dropdown-meau` _默认下拉选单自动位于其父级的上方的 100% 及贴齐左边缘_ 增加`.dropdown-mean-right`可居右对齐
  >     - `a.dropdown-item` , 增加`.active`使样式启用，增加`.disabled`使样式不可点击 ，
  >     - `button.dropdown-item`
  >     - `*.dropdown-item-text` 非交互式

- 特殊选单内容 `.dropdown-meau >`

  [_特殊下拉选单内容 jsfiddle 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275895/)

- 自定义选项(在.dropdown-toggle 上)： // todo

  - `data-offset=0!default` ：dropdown-meau 相对于其目标的偏移 例：data-offset='10,20'
  - `data-flip:true!default` ： 允许下拉选单重叠到其相关的元素上
  - `data-boundary='scrollParent'!default`
  - `data-reference='toggle'!default` 在.dropdown-toggle 上
  - `data-display='dynamic!default'`

- 方法：

  - `$().dropdown('toggle')` ： 给予导览列或分页导览使用切换下拉选单功能。
  - `$().dropdown('update')` ： 更新下拉选单元素的定位。
  - `$().dropdown('dispose')`：销毁一个元素的下拉选单 js 实例。

- 事件：

  - `$().on('show.bs.dropdown',function(){调用显示时立即触发})`
  - `$().on('shown.bs.dropdown',function(){这个物件可被看见时会触发此事件(当完成 CSS 转换后)})`
  - `$().on('hide.bs.dropdown',function(){调用隐藏时被立即触发})`
  - `$().on('hidden.bs.dropdown',function(){这个物件隐藏后会触发此事件(当完成 CSS 转换后)})`

## 按钮 .btn

- 基本结构：
  - 9 个样式 class`.btn`：`.btn-primary` `.btn-secondary` `.btn-success` `.btn-danger` `.btn-waring` `.btn-info` `.btn-light` `.btn-dark` `.btn-link`
  - 8 个外框 hover 按钮： `.btn-outline-primary` `.btn-outline-secondary` `.btn-outline-success` `.btn-outline-danger` `.btn-outline-waring` `.btn-outline-info` `.btn-outline-light` `.btn-outline-dark`
  - 大小：`.btn-sm` `.btn` `.btn-lg` `.btn-group-sm>.btn` `.btn-group>.btn` `.btn-group-lg>.btn`
  - 块级：`.btn-blcok`
  - 启用/停用效果： `.active` `[disabled]`/`a.disabled`
  - 其他`input`效果： `div.btn-group.btn-group-toggle[data-toggle='buttons'] > label.btn > input` [_input radio 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275872/)
- 自定义选项 data-api：
  - js - toggle 切换：`[data-toggle='button']`
- js 方法：
  - `$().button('toggle')` ：切换.active 状态
  - `$().button('toggle')` ： 销毁 button instance 示例

## 按钮组 .btn-toolbar > .btn-group > .btn

- 按钮工具列`.btn-toolbar >.btn-group`
- 大小：`.btn-group-sm` `.btn-group` `.btn-group-lg`
- 巢状嵌套：`.btn-group > .btn + .btn-group > .btn`
- 竖直排列：`.btn-group-vertical` [_按钮组 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275874/)

## 输入群组 .input-group

- 基本结构：

  > - `div.input-group-{sm/lg}` _flex_
  >
  >   - `div.input-group-prepend` _flex_
  >     - `.input-group-text` > `input[type='checkbox']`/`input[type='radio']` _灰底文字+单选多选_
  >     - `button.btn` _按钮_
  >     - `.dropdown-toggle/dropdown-toggle-split` & `.dropdown-menu>.dropdown-item` _下拉菜单_
  >   - `input.form-control / .custom-select>option / .custom-file>input.custom-file-input` _relative_
  >   - `div.input-group-append` _flex_
  >     - `.input-group-text` > `input[type='checkbox']`/`input[type='radio']`
  >     - `button.btn`
  >     - `.dropdown-toggle/dropdown-toggle-split` & `.dropdown-menu>.dropdown-item`
  >
  >   都可多个一起出现

## 列表群组 .list-group>.list-group-item & [data-toggle="tab"]

- 基本结构：

  > _flex 布局 column 排列_
  >
  > - `.list-group` & `.list-group-flush`(移除边界边框和圆角)
  >   - `.list-group-item` & `.active` & `.disabled/button[disabled]` & `.list-group-item-action`(有 hover,active,focus 效果) & `list-group-item-{primary/secondary/success/danger/warning/info/light/dark}`
  >     - `span.badge`(带角标)

- 自定义选项：

  - `.list-group-item[data-toggle="list"][href/data-target="#xxx"]`+`.tab-pane#xxx` tab 分页标签

- 方法：

  > `$().tab('show')`：选择给定的列表项目显示其关联的分页，在 `shown.bs.tab` 事件发生之前隐藏所有其他的 panel
  >
  > [_tab 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275829/)

- 事件：事件触发顺序 如 列表顺序

  - `$().on('hide.bs.tab',function(){隐藏target[上一个元素]})`
  - `$().on('show.bs.tab',function(){显示target[当前元素]})`
  - `$().on('hidden.bs.tab',function(){隐藏target[上一个元素]完成。使用 event.target 和 event.relatedTarget 分别定位上一个启用选项和新启用的选项})`
  - `$().on('shown.bs.tab',function(){显示target[当前元素]完成。使用 event.target 和 event.relatedTarget 来分别定位启用中和上一个启用的选项})`

## 表单 form.form-inline > div.form-group/.form-row

- 基本结构：

  > - `form>` _行内表单`&.form-inline`_
  >
  >   - 表单群组：`div.form-group`
  >
  >     - 块级文字：`*.form-text`
  >     - 文本形式控制元件使用 `.form-control` 进行样式化：`input[type='text/email..'].form-control` （如 `<input>`、`<select>` 和 `<textarea>`）
  >     - 档案类型的 input，改用 `.form-control-file` 取代 `.form-control`：`input[type='file'].form-control-file`
  >     - 设置水平可滚动的`input[range]`： `input[type='range'].form-control-range`
  >     - 只读元素：`input[readonly].form-control-plaintext` _将`input[readonly]`设置灰色背景；`.form-control-plaintext`设置为纯文本的样式去边框和背景_
  >     - 单选多选元素：`div.form-check` _.form-check 有个 padding-left，为 checkbox 和 radio 服务_ `&.form-check-inline` 将内部元素放到同一水平行上水平样式 _.form-check-inline 是 inline-flex；.form-check-inline .form-check-input 是 static_
  >       - `&>label[for=''].form-check-label`
  >       - `&>input[type='checkbox/radio' value=''].form-check-input` _.form-check-input 是 absolute 有负值 margin-left_
  >       - `&>input[type='checkbox/radio' value=''].form-check-input.position-static` 将 `.form-check` 没有任何文字内容 label 的 input 加上 `.position-static`
  >
  >   - 表单行：`div.row/.form-row>.col` _有表单格线，可与.form-group 同级 或 子集_
  >
  >     - `label/legend.col-form-label` _.col-form-label-{sm/lg} 可以垂直居中行内 label_
  >     - `.col>input.form-control`

- 其他 class：

  - 设置高度`.form-control-lg` 和 `form-control-sm`

  - 验证`form[novalidate].was-validated *[required]:invalid/:valid + div.valid-feedback`：

    > [_表单验证 jsfiddle 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275841/)
    >
    > 需要将`[novalidate]`属性(禁用浏览器预设的回馈提示，但仍提供 JavaScript 中表单验证 API 有效)添加到 form
    >
    > 会验证后 在 form 添加 `.was-validated` 对子元素的`:invalid` `:valid`样式。它适用于 `<input>`、`<select>` 和 `<textarea>` 元素
    >
    > 在`input`后面 `div.invalid-feedback` 或 `div.invalid-tooltip`
    >
    > 在 script 内 需要 `form.classList.add('was-validated');``

  - 自定义表单

    _`.custom-control` 有个`padding-left:1.5rem`_

    [自定义选择框 jsfiddle 示例](https://jsfiddle.net/hojondo/aq9Laaew/275855/)

    - 自定义单选复选框`div.custom-checkbox/custom-radio > input[].custom-control-input + label.custom-control-label`

      > _隐藏`input`利用`:checked`伪类对应`.custom-control-label::before/::after`显示假框,after 是个白色对号 before 是点选与否的背景_
      >
      > 行内：`div.custom-control.custom-control-inline{display: inline-flex;margin-right: 1rem;}`
      >
      > 禁用：给 input 加`[disabled]`。lable 会 muted 文字，input 会 color 灰色 background 透明

    - 自定义 select `select.custom-select`

      > 大小：`custom-select-lg/sm`

    - 自定义 input[range] `input[type='range'].custom-range`

    - 自定义 input[file] `div.custom-file>input[type='file'].custom-file-input + label.cunstom-file-label`

## ---

## 小标签 .badge

- 基本结构：
  - 8 个颜色样式`.badge`：`&.badge-primary` `&.badge-secondary` `&.badge-success` `&.badge-danger` `&.badge-waring` `&.badge-info` `&.badge-light` `&.badge-dark`
  - 添加胶囊样式 ：`&.badge-pill` _padding:0 .6em;border-radius:10rem_
  - 字体大小 ：`font-size:75%` 和 `padding:.25em .4em` 单位使标签继承相对父元素的尺寸
  - 预设空内容 ：`.badge:empty{display:none}`
  - `span.sr-only{unread message nums}` ：考虑支持 screenreaders

## 进度条 .progress>.progress-bar

- 基本结构：

  > - `.progress` _控制 height_ _可以包含 n 个.progress-bar 并列_
  >   - `.progress-bar` & `.w-75` & `bg-{success/info/warning/...} 背景`&`.progress-bar-striped .progress-bar-animated 条纹/动态条纹`&`style:width:20%`
  >     - `textElement`

## 警告 .alert

- 基本结构：
  - 8 个颜色样式`.alert`：`&.alert-primary` `&.alert-secondary` `&.alert-success` `.&alert-danger` `.&alert-waring` `&.alert-info` `&.alert-light` `&.alert-dark`
  - 8 个样式下的 `.alert .alert-link/.alert-heading` 有各自相对应的颜色
  - 关闭按钮 ：`div.alert.alert-dismissible > .close[data-dismiss='alert']>span{X}`
- 触发方式：
  - `$('.alert').alert()` ：_好像压根没用_
  - `$().alert('close')` ：从 DOM 删除
  - `$().alert('dispose')` ：取消一个元素的警报 （销毁 jQuery 的 alert 示例）
- 事件：
  - `$().on('close.bs.alert',function(){})`：调用 close 时会触发
  - `$().on('closed.bs.alert',function(){})` ：警报关闭后（css 转换完成时）

## 工具提示 `$().tooltip()`

- 基本结构：

  > - `*[data-toggle="tooltip"]`&`[data-placement="top/right/.."]`&`[data-html="true"]` _这里的 data-toggle 不是 data-api，单纯的人为记号_
  > - 页面上初始化所有工具提示框 js: `$('[data-toggle="tooltip"]').tooltip()`

- 自定义选项：

  > | 名称              | 类型                          | 预设值                                                                                                   | 描述                                                                                                                                                                                                                                                                                                                                                                        |
  > | ----------------- | ----------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  > | animation         | boolean                       | true                                                                                                     | 将 CSS 渐变套用到工具提示框                                                                                                                                                                                                                                                                                                                                                 |
  > | container         | string \| element \| false    | false                                                                                                    | 将工具提示框加到特定元素。例如：`container: 'body'`。 该选项特别有用，因为它允许您将触摸屏定位在触发元素附近的文字内容 - 这将防止在画面调整大小期间弹出的提示远离触发元素。                                                                                                                                                                                                 |
  > | delay             | number \| object              | 0                                                                                                        | 显示和隐藏弹出提示框的延迟（ms） - 不适用于手动触发类型如果提供了一个数字，则将延迟应用于隐藏/显示物件结构是：`delay: { "show": 500, "hide": 100 }`                                                                                                                                                                                                                         |
  > | html              | boolean                       | false                                                                                                    | 在工具提示框中允许 HTML 如果为 true，工具提示框的 `title` 中的 HTML 标签将在工具提示框中呈现。 如果为 false，则将使用 jQuery 的 `text`方法将内容插入到 DOM 中。如果您担心 XSS 攻击，请使用文字。                                                                                                                                                                            |
  > | placement         | string \| function            | 'top'                                                                                                    | 如何定位工具提示框 - auto \| top \| bottom \| left \| right。 指定 `auto` 时，将动态重新定位工具提示框。当函数用于确定位置时，将使用工具提示框 DOM 节点作为其第一个参数并将触发元素 DOM 节点作为其第二个参数来调用。 `this` 将被设为弹出提示框实例。                                                                                                                        |
  > | selector          | string \| false               | false                                                                                                    | 如果提供了选择器，工具提示框将被委派给指定的目标。实际上，这用于动态 HTML 来扩增工具提示物件。 请参阅[此 ](https://github.com/twbs/bootstrap/issues/4215)和 [一个讯息范例](https://jsbin.com/zopod/1/edit)。                                                                                                                                                                |
  > | template          | string                        | `'<div class="tooltip" role="tooltip"><div class="arrow"></div><div class="tooltip-inner"></div></div>'` | 创建工具提示框时使用的基本 HTML 工具提示框的 `title` 将被注入到 `.tooltip-inner` 中。`.arrow` 将成为工具提示框的箭头。最外层的包装元素应该有 `.tooltip` 及 `role="tooltip"`。                                                                                                                                                                                               |
  > | title             | string \| element \| function | ''                                                                                                       | 如果 `title` 属性不存在，则为预设标题值。如果给出一个函数，它将被调用，其 `this` 引用设置为工具提示框附加到的元素。                                                                                                                                                                                                                                                         |
  > | trigger           | string                        | 'hover focus'                                                                                            | 如何触发工具提示框 - click \| hover \| focus \| manual。 您可以传递多个触发器；将它们与空格分开。`manual` 不能与任何其他触发器组合。`'manual'` 表示工具提示框将透过 `.tooltip('show')`、`.tooltip('hide')` 及 `.tooltip('toggle')` 的方法触发，这个值不能与其它的触发器做组合。`'hover'` 将导致键盘无法触发工具提示框，只能做为使用键盘用户传递讯息的替代方法。 </td> </tr> |
  > | offset            | number \| string              | 0                                                                                                        | 工具提示框相对于其目标的偏移。更多信息，请参阅 Popper.js 的 [偏移文档](https://popper.js.org/popper-documentation.html#modifiers..offset.offset)。                                                                                                                                                                                                                          |
  > | fallbackPlacement | string \| array               | 'flip'                                                                                                   | 指定工具提示框将在调回时使用哪个位置。 有关更多信息，请参阅 Popper.js 的 [行为文档](https://popper.js.org/popper-documentation.html#modifiers..flip.behavior)                                                                                                                                                                                                               |
  > | boundary          | string \| element             | 'scrollParent'                                                                                           | Overflow constraint boundary of the tooltip. Accepts the values of `'viewport'`, `'window'`, `'scrollParent'`, or an HTMLElement reference (JavaScript only). For more information refer to Popper.js's [preventOverflow docs](https://popper.js.org/popper-documentation.html#modifiers..preventOverflow.boundariesElement).                                               |

- 方法：

  - `$().tooltip({})` ：自定义创建 tooltip
  - `$().tooltip('show')` ： "手动" 显示工具提示框。在 `shown.bs.tooltip` 事件发生之前
  - `$().tooltip('hide')` ： "手动" 隐藏工具提示框。在 `hidden.bs.tooltip` 事件发生之前
  - `$().tooltip('toggle')` ："手动"切换工具提示框。在 `shown.bs.tooltip` 或 `hidden.bs.tooltip` 事件发生之前
  - `$().tooltip('dispose')` ：隐藏和破坏元素的工具提示框。 使用 自定义创建[data-toggle="tooltip"]的工具提示框 不能在后代触发元素上单独销毁
  - `$().tooltip('enable')` ：给一个元素的工具提示框显示的功能，_Tooltips are enabled by default_
  - `$().tooltip('disable')` ：删除元素的工具提示框的显示功能。只有在重新启用后，才能显示工具提示框
  - `$().tooltip('toggleEnabled')` ：切换可以显示或隐藏元素的功能
  - `$().tooltip('update')` ：更新提示框的位置

- 事件：

  - `$().on('show.bs.tooltip', function () {当调用 show 实例方法时，此事件会立即触发})`
  - `$().on('shown.bs.tooltip', function () {当工具提示框显示后，会触发此事件（待CSS转换完成）})`
  - `$().on('hide.bs.tooltip', function () {当调用 hide 实例方法时，会立即触发此事件})`
  - `$().on('hidden.bs.tooltip', function () {当工具提示框对隐藏后，会触发此事件（待CSS转换完成）})`
  - `$().on('inserted.bs.tooltip', function () {将工具提示框范本加到 DOM 后，会在 show.bs.tooltip事件后 shown.bs.tooltip事件前 触发此事件})`

## 弹出框 `$().popover()`

- 基本结构：

  > - `[data-toggle="popover"]`&`[data-content=""]`&`[data-trigger=""]`
  > - `$('[data-toggle="popover"]').popover()`

- 自定义选项：相比于 `tooltip`多的`content`和有差别的`template` `trigger`

  > | 名称     | 类型                          | 预设                                                                                                                                    | 描述                                                                                                                                                                                                             |
  > | -------- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  > | content  | string \| element \| function | ''                                                                                                                                      | 如果 `data-content` 属性不存在，则为预设内容值。如果给出一个函数，它将被调用，其`this` 引用设置为弹出提示框附加到的元素。                                                                                        |
  > | template | string                        | `'<div class="popover" role="tooltip"><div class="arrow"></div><h3 class="popover-header"></h3><div class="popover-body"></div></div>'` | 创建动态提示框时使用的基本 HTML 动态提示框的 `title` 将被注入到 `.popover-header` 中。动态提示框的 `content` 将被注入到 `.popover-body` 中。`.arrow` 将成为动态提示框的箭头。最外层的包装元素应该有 `.popover`。 |
  > | trigger  | string                        | 'click'                                                                                                                                 | 如何触发动态提示框 - click \| hover \| focus \| manual。 您可以传递多个触发器；将它们与空格分开。`manual` 不能与任何其他触发器组合。                                                                             |

- 方法：同 [tooltip](#工具提示 `$().tooltip()`)

- 事件：同 [tooltip](#工具提示 `$().tooltip()`)

## 模态框 .modal

- 基本结构：

> - `*[data-toggle="modal"][href/data-target="#xxx"]`
> - `div#xxx.modal[tabindex="-1"]` & `.fade`(加淡入淡出动画)
>   - `div.modal-dialog` & `.modal-dialog-centered`(垂直居中) & `.modal-lg/sm`(大小框)
>     - `div.modal-content`
>       - `div.modal-header`
>         - `.modal-title`
>         - `.close[data-dismiss="modal"]>span{&times;}` _关闭按钮 data-dismiss="modal"_
>       - `div.modal-body`
>       - `div.modal-footer`

> 显示 Modal 时，给 body 加`modal-open{overflow:hidden}`禁止滚动，同时`body.append('div.modal-backdrop')` _阴影背景的 z-index 是 1040,modal 的 z-index 是 1050_
>
> `[data-toggle="popover" ]` `[data-toggle=tooltip]`也可以放在`.modal`里面，modal 关闭状态时也会自动把前者关闭；
>
> 通过事件`show.bs.modal`和`event.relatedTarget`可以拿到动态显示`modal`内容；
>
> _tabindex 负一使键盘 tab 操作无效_

- 方法：

  - `$('.modal').modal({})`
    - `backdrop : 默认true` _包括动态视窗背景元素。或者指定 `static[string]` 在点击背景时不关闭动态视窗_
    - `keyboard : 默认true` _按下 ESC 时关闭动态视窗_
    - `focus : 默认true` _初始化时 focus 动态视窗_
    - `show : 默认true` _初始化时就显示 modal_
  - `$().modal("show")`
  - `$().modal("hide")`
  - `$().modal("toggle")` ： 切换 modal 显示隐藏
  - `$().modal("handleUpdate")` ： 如果动态视窗在打开状态（比如在出现滚动条的情况下）时重新改变高度，则重新调整动态视窗的左右位置*因为滚动条出现与否会影响文档的居中判定使其自动根据滚动条重新居中，此方法可以让 modal 回到一打开时的左右位置*。
  - `$().modal("dispose")` ： 销毁一个元素的 modal _对所有的 dispose 来说，都只对 DOM 中当前存在的元素进行操作，即：modal.display=block 时可以销毁，none 时不识别_

- 事件：

  - `$().on('show.bs.modal', function (e) {当调用 show 实例方法时，此事件会立即触发。如果是因点击，点击的元素作为事件的 relatedTarget 属性可用})`
  - `$().on('shown.bs.modal', function (e) {当动态视窗显示时会触发此事件（等待 CSS 转换完成）。如果是因点击导致，点击的元素作为事件的 relatedTarget 属性可用})`
  - `$().on('hide.bs.modal', function (e) {当调用 hide 实例方法时，会立即触发此事件})`
  - `$('.modal').on('hidden.bs.modal', function (e) {当动态视窗隐藏后会触发此事件（等 CSS 转换完成）})`

# Utilities 通用类别

## 网格 .container > .row > .col-{breakpoint}-{1~12}

用 flexbox 网格来建立符合各种尺寸的网页排版，包含*十二栏*系统、预设的*五个响应式断点*、*Sass 变量*和 _mixins_、以及很多预定义的 _class_

- 十二栏系统：col-12 , col-6 , col-4...
- 五个断点：col-（<576px）, col-sm-（≥576px）, col-md-（≥768px） , col-lg-（≥992px） , col-xl-（≥1200px） **基于最小高度 col 向上 col-xl 适用**
- 预定义的 class：
  - `col-{breakpoint}` ：从  `xs`  到  `xl` 对于无单位的 class 如 col-md，每一栏将具有相同的宽度
  - `col-{breakpoint}-5` ：设置一栏宽度，其它栏都将重新调整大小平分剩余（的 12-5=7）
  - `col-{breakpoint}-auto` ：基于栏内容的**自然宽度** _比如文字长度_，可使用   自动调整栏的大小
  - `.col.offset-{breakpoint}-{n}` ：向右移动列，通过{n}增加左边距
  - `.col.ml/mr-{breakpoint}-auto` || `.col.ml/mr-{breakpoint}-{0~5}` ：向左右添加 margin*0, 0.25rem, 0.5rem, 1rem, 1.5rem, 3rem*
  - `.row>.col+.row` ：可以嵌套并且每个 row 内都是 12 单位
  - `.row>*+div.w-100+*` ：可在 row 内中间加入，强制一 row 占多行，同时可以设置 `d-{breakpoint}-{value}`控制显示隐藏 [_jsfiddle 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275786/)
  - 垂直对齐： row：以最上对齐`align-items-start` 以 垂直居中`align-items-center` 以最下对齐`align-items-end`；col：`align-self-start` `align-self-center` `align-self-end`
  - 水平对齐： row：以最左对齐`justify-content-start` 以水平居中`justify-content-center` 以最右对齐`justify-content-end` 项目之间的间隔都相等`justify-content-center` 项目两侧的间隔都相等`justify-content-end`；[_jsfiddle 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275780/)
  - `.row.no-gutters`移除 row 的负 margin，移除 col 之间的 padding
  - 排序 `.col.order-{breakpoint}-{n}` ：数字越大 越靠后，未设置的默认`0` ，`order-{breakpoint}-first`等价于`-1`最前 `order-{breakpoint}-last`等价于`13`最后。
- 重定义 sass 变量：略
- 重定义 mixins 函数：略

## 表格.table

- table 选项 ：统一样式 `table.table`
  - `table.table-bordered` 给 table 四周添加边框
  - `table.table-borderless` table 无边框
  - `table.table-hover` 给 tbody 下的 row 添加 hover 效果
  - `table.table-sm` 将 padding 缩小
  - 语义化颜色区别 _在 table，tr 或 td/th 都可_ [_table 颜色 jsfiddle 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275809/)
    - `table-active`
    - `table-primary`
    - `table-secondary`
    - `table-success`
    - `table-danger`
    - `table-warning`
    - `table-info`
    - `table-light`
    - `table-dark`
  - 在父级响应式： `div.table-responsive>table` `div.table-responsive-sm/md/lg/xl>table`
- thead 选项：`thead.thead-dark` 或 `thead.thead-light`
- tbody 选项：`table.table-striped` 给 tbody 斑马纹
- caption 样式：`css{caption-side: bottom;}`

## ---

## 定位 .position-static/relative/absolute/fixed/sticky, .fixed-top/bottom, .sticky-top

`.position-static` `.position-relative` `.position-absolute` `.position-fixed` `.position-sticky`

`.fixed-top` `.fixed-bottom`

`.sticky-top` [_粘性布局 jsfiddle 示例_](https://jsfiddle.net/hojondo/aq9Laaew/275806/)

## 浮动 .float-{breakpoint}-left/right/none

`.float-{breakpoint}-left` `.float-{breakpoint}-right` `.float-{breakpoint}-none`

## 清除浮动 .clearfix

```css
.clearfix::after {
  display: block;
  clear: both;
  content: "";
}
```

## 垂直对齐方式 .align-

> 垂直对齐仅影响 inline、inline-block、inline-table、和 table 元素
>
> [_table 对齐示例_](https://jsfiddle.net/hojondo/aq9Laaew/275815/)
>
> `默认.align-baseline` `.align-top` `.align-middle` `.align-bottom` `.align-text-bottom` `.align-text-top`

## ---

## 可见性 .visible, .invisible

`.visible` = `visibility: visible`;

`.invisible` = `visibility: hidden`

## display .d-{breakpoint}-{value}

- breakpoint 取值：xs sm md lg xl 都可，`.d-{value}` 代表 xs
- value 取值：`none` `inline` `inline-block` `block` `table` `table-cell` `table-row` `flex` `inline-flex`
- _补充_ print 打印设置：`.d-print-none` `.d-print-inline` `.d-print-inline-block` `.d-print-block` `.d-print-table` `.d-print-table-row` `.d-print-table-cell` `.d-print-flex` `.d-print-inline-flex`

## 尺寸 .w-{} .h-{}

`.w-25` `.w-50` `.w-75` `.w-100` `.w-auto` `.mw-100`/max-width:100%;

`.h-25` `.h-50` `.h-75` `.h-100` `.h-auto` `.mh-100`/max-height:100%;

## 间隔 .m-{} .p-{}

- margin ：`.m / mx/my / mt/mb/ml/mr-{breakpoint}-{0-5/auto}`
- padding：`.p / px/py / pt/pb/pl/pr-{breakpoint}-{0-5/auto}`

## 边框 .border

- 加边：`.border` `.border-top` `.border-right` `.border-bottom` `.border-left`
- 去边：`.border-0` `.border-top-0` `.border-right-0` `.border-bottom-0` `.border-left-0`
- 颜色：`.border-primary` `.border-secondary` `.border-success` `.border-danger` `.border-warning` `.border-info` `.border-light` `.border-dark` `.border-white`
- 圆角：`.rounded` `.rounded-top` `.rounded-right` `.rounded-bottom` `.rounded-left` `.rounded-circle` `.rounded-0`

## 弹性布局 Flex .d-{value}-flex, .d-{value}-inline-flex (属于 display 分类)

内容较多 见 [flex - 通用类别 - 六角学院](https://bootstrap.hexschool.com/docs/4.1/utilities/flex/)

- 方向性：
  - 水平 - 默认左到右`.flex-{breakpoint}-row` 或者 右到左`.flex-{breakpoint}-row-reverse`
  - 竖直 - 上到下`.flex-{breakpoint}-colum` 或者 下到上`flex-{breakpoint}-colum-reverse`
- 对齐方式：
  - 主轴上`.justify-content` - (row 是横 x)：`.justify-content-{breakpoint}-start` `.justify-content-{breakpoint}-end` `.justify-content-{breakpoint}-center` `.justify-content-{breakpoint}-between` `.justify-content-{breakpoint}-around`
  - 侧轴上`.align-items` - (row 是竖 y)：`.align-items-{breakpoint}-start` `.align-items-{breakpoint}-end` `.align-items-{breakpoint}-center` `.align-items-{breakpoint}-baseline` `.align-items-{breakpoint}-stretch`
- wrap：`.flex-{breakpoint}-nowrap`、`.flex-{breakpoint}-wrap`、`.flex-{breakpoint}-wrap-reverse`
- 多行对齐：`.align-content-start` `.align-content-end` `.align-content-center` `.align-content-around` `.align-content-stretch`
- 自身对齐 _在.flex-item 上 覆盖.felx 的 align-items 属性_：`.align-self-{breakpoint}-start` `.align-self-{breakpoint}-end` `.align-self-{breakpoint}-center` `.align-self-{breakpoint}-baseline` `.align-self-{breakpoint}-stretch` _对.flex-item_
- 填满：`.flex-fill` `.flex-sm-fill` `.flex-md-fill` `.flex-lg-fill` `.flex-xl-fill` _对.flex-item 强制 flex: 1 1 auto_
- 伸缩值：`.flex-{breakpoint}-{grow|shrink}-0` `.flex-{breakpoint}-{grow|shrink}-1` _对.flex-item_
- margins：也支持 margin 类别的 `ml/mr/mx/my-{breakpoint}-auto` `ml/mr/mx/my-{breakpoint}-{0~5}` _对.flex-item_
- 排序 ：`.order-{breakpoint}-{0~12}` `order-{breakpoint}-first`等价于`-1`， `order-{breakpoint}-last`等价于`13` _对.flex-item_

## ---

## 代码块 code, pre, kbd

- `code` _word-break:break-word_
- `pre` _font-size:87.5%;color:#212529_
- `kbd` _font-size:87.5%;background-color:#212529;border-radius:0.2rem_

## 文字 .text-

- 对齐 ：`.text-{breakpoint}-left` `.text-{breakpoint}-center` `.text-{breakpoint}-right` `.text-justify`
- 换行：`.text-nowrap`
- 省略：`.d-[inline-]block.text-truncate` _源码：overflow:hidden;text-overflow:ellipsis;white-space:nowrap_
- 大小写：`.text-uppercase` `.text-lowercase` `.text-capitalize`(仅改变 每个词首字母)
- 字体粗细斜体：`.font-weight-light` `.font-weight-normal` `.font-weight-bold` `.font-italic`
- 等宽字体：`.text-monospace` _font-family: SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;_
- 文字排版
  - `h1-h6` 和 `.h1-.h6` ：reset 重定义样式
  - `h{1-6}>small.text-mute` ：二级小标题
  - `.display-{1-4}` ：突出一个标题，在`font-size,font-weight,line-height`3 个属性上不同程度放大
  - `.lead` ：字体放大`font-size:1.5rem,font-weight:300`
  - `.initialism` ：字体缩小 `font-size:90%;text-transform:uppercase`
  - `media-query{ html {font-size:xx}}` ：响应式对全局文字大小设置
  - `.blockquote` `cite`引用块
  - `ul.list-unstyle` 对 ul 和 ul>li 继承`list-style: none`
  - `ul.list-inline>li.list-inline-item` 横排导航栏
  - `dl>dt+dd` 的并列对齐 可以用`.row>col`实现

## 图片.img-

- 响应式图片 `.img-fluid` : `max-width:100%;height:auto;`
- 缩略图 `.img-thumbnail` : 有`padding border border-radius background-color`设置
- 对齐图片 `.float-left/right` 居中：`.text-center>img , .mx-auto`

## 图片区 .figure

使用内建的 `figure.figure`、 `figure>img.figure-img` 和 `figure>caption.figure-caption` 类别，应用内建样式。其中.figure{display:inline-block;}

## ---

## 颜色 .text-{}, .bg-{}, .bg-gradient-{}

- 13 个`.text-`：`.text-primary` `.text-secondary` `.text-success` `.text-danger` `.text-waring` `.text-info` `.text-light` `.text-dark` `.text-white` `.text-body` `.text-muted` `.text-black-50` `.text-white-50`
- 10 个`.bg-`： `.bg-primary` `.bg-secondary` `.bg-success` `.bg-danger` `.bg-waring` `.bg-info` `.bg-light` `.bg-dark` `.bg-white` `.bg-transparent`
- 8 个`.bg-gradient-`： `.bg-gradient-primary` `.bg-gradient-secondary` `.bg-gradient-success` `.bg-gradient-danger` `.bg-gradient-waring` `.bg-gradient-info` `.bg-gradient-light` `.bg-gradient-dark` ……_渐变背景注意 bootstrap：默认不启用,需要在自己的 scss import bootstrap 之前`$enable-gradients: true;`_

## 阴影 .shadow

_对组件 component 来说 默认不启用预设阴影 ,需要在 import bootstrap 之前 \$enable-shadows:true;_

`.shadow-none` .`shadow-sm` `.shadow` `.shadow-lg` 阴影逐步变大变明显

```css
.shadow-none {
  box-shadow: none !important;
}
.shadow-sm {
  box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075) !important;
}
.shadow {
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15) !important;
}
.shadow-lg {
  box-shadow: 0 1rem 3rem rgba(0, 0, 0, 0.175) !important;
}
```

## 关闭图标 .close>span{&times;}

```html
<button type="button" class="close" aria-label="Close">
  <span aria-hidden="true">&times;</span>
</button>
```

## 响应式嵌入内容 div.embed-responsive-{value} > .embed-responsive-item

用于 <iframe>, <embed>, <video>, 和 <object>

长宽比 4 种选项 `.embed-responsive.embed-responsive-21by9/16by9/4by3/1by1`

```html
<!-- 21:9 aspect ratio -->
<div class="embed-responsive embed-responsive-21by9">
  <iframe
    class="embed-responsive-item"
    src="https://www.youtube.com/embed/zpOULjyy-n8?rel=0"
  ></iframe>
</div>
```

## 屏幕阅读器 screenreaders .sr-only, .sr-only-focusable

> 是指 对阅读障碍者友好的 设备适配 （一种可将文字、图形以及电脑接口的其他部分转换成语音及/或点字的软件）

`.sr-only` 正常隐藏 只在荧幕阅读器上显示

`.sr-only-focusable` 正常隐藏 只在萤幕屏幕上显示 或者 被 focus/active 的时候显示

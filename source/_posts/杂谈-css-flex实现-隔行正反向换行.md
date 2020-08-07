---
title: 杂谈-css-flex实现-隔行正反向换行
top: false
cover: false
toc: true
mathjax: false
date: 2020-05-03 00:59:35
tags: ["css"]
password:
summary:
categories: ["杂谈"]
---

# css 实现 flex 布局下，奇数行正向排序，偶数行反向排序

目标效果：
[![Jx4v0P.md.png](https://s1.ax1x.com/2020/05/03/Jx4v0P.md.png)](https://imgchr.com/i/Jx4v0P)
[![Jx4jmt.md.png](https://s1.ax1x.com/2020/05/03/Jx4jmt.md.png)](https://imgchr.com/i/Jx4jmt)

```html
<ul>
  <li v-for="(item,index) in listArr" :key="index">
    {{ index }}
  </li>
</ul>
```

### 第一种解决方案 - scss/flex-order

```scss
// scss
ul {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  li {
    position: relative; // 为::before做absolute position 准备
    flex-grow: 1; // 掩护单行没到达5个item的情况，配合 偶数行第一个不要grow
    width: 20%; // 宽度是5个item平分
    height: 160px;
    padding-top: 12rpx; // 为::before腾出空间
    border: 0 solid #b3b3b3;
    border-top-width: 2rpx; // 预留border-top
    text-align: center; // 偶数行最后（左）一个item的left:0会在边缘，要避免
    &.active {
      border-top-color: #3f4c8c;
      &::before {
        border: 4rpx solid #3f4c8c;
      }
    }
    &::before {
      content: "";
      position: absolute;
      top: 0;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 24rpx;
      height: 24rpx;
      background: #ffffff;
      border: 4rpx solid #999999;
      border-radius: 50%;
    }
    &:nth-child(10n + 6):last-child {
      border-top-width: 2rpx;
      width: 80%; // 宽度是4个item的总长
    }
    @for $i from 0 through 150 {
      &:nth-child(#{$i + 1}) {
        // @if ($i+1)/5 % 2==0 or $i/5 % 2==0{
        // 	box-sizing: content-box;
        // 	padding-left: 80rpx;
        // }

        @if ($i + 1)/5 % 2==1 {
          border-right-width: 2rpx;
          border-bottom-width: 2rpx;
          border-radius: 0 50% 50% 0;
        } @else if ($i + 1)/5 % 2==0 {
          border-left-width: 2rpx;
          border-bottom-width: 2rpx;
          border-radius: 50% 0 0 50%;
        } @else if $i%5==0 and $i!=0 {
          border-top-width: 0;
          flex-grow: 0; // 偶数行第一个 不要grow，以便后续兄弟的border-top-width能撑满这行
        }
        @if floor($i / 5) % 2 == 0 {
          order: #{$i};
        } @else {
          order: #{$i + (2 - ($i % 5)) * 2};
        }
        @if floor($i/5) >0 {
          margin-top: -2rpx; // 除第一行以为 统一向上缩进一个border-top-width的高度，以便这行线无缝连接
        }
      }
    }
  }
}
```

### 第二种解决方案 float/nth-child()

```css
ul > li:nth-child(10n + 6),
ul > li:nth-child(10n + 7),
ul > li:nth-child(10n + 8),
ul > li:nth-child(10n + 9),
ul > li:nth-child(10n) {
  float: right;
}
```

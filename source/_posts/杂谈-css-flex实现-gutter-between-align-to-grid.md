---
title: 杂谈-css-flex实现 gutter between align to grid
top: false
cover: false
toc: true
mathjax: false
date: 2020-01-08 16:33:04
tags: ['杂谈', 'css']
password:
summary:
categories:
---
# css实现flex布局下，带间隔between最后一行自动对齐表格

目标效果：
[![l2M7zn.md.png](https://s2.ax1x.com/2020/01/08/l2M7zn.md.png)](https://imgchr.com/i/l2M7zn)
```html
<div class="commentBrands">
    <img src="" alt=""/>
</div>
```

```css
    .commentBrands{
        display: flex;
        flex-flow: row wrap;
        justify-content: space-between;
        margin: 0 -.08rem;
        &:after{
            content: '';
            flex: auto;
        }
        >img{
            width: calc(100% / 6);
            padding: 0 .08rem;
        }
    }
```
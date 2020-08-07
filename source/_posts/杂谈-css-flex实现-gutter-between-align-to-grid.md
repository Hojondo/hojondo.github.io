---
title: css-flex实现 gutter between align to grid
top: false
cover: false
toc: true
mathjax: false
date: 2020-01-08 16:33:04
tags: ["css"]
password:
summary:
categories: ["杂谈"]
---

# css 实现 flex 布局下，带间隔 between 最后一行自动对齐表格

目标效果：
[![Jx524S.md.png](https://s1.ax1x.com/2020/05/03/Jx524S.md.png)](https://imgchr.com/i/Jx524S)

```html
<div class="commentBrands">
  <img src="" alt="" />
</div>
```

```css
.commentBrands {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
  margin: 0 -0.08rem;
  &:after {
    content: "";
    flex: auto;
  }
  > img {
    width: calc(100% / 6);
    padding: 0 0.08rem;
  }
}
```

---
title: uniapp 小程序开发踩坑
top: false
cover: false
toc: true
mathjax: false
date: 2020-04-21 00:26:04
tags: ["小程序开发"]
password:
summary:
categories: ["笔记"]
---

## 学习要点

1. `uni`扩展 API，同微信小程序 API，前缀`uni.`代替`wx.`

## 关于 SCSS

1. 识别`&`父选择器占位符，但不识别`&`处于非行首的情况。如*.first-row &{}*
2. font-size 设置 fonticon 时，align-items :center 无效

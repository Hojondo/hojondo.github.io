---
title: 笔记-js核心基础
date: 2020-08-11T15:12:31+08:00
top: false
cover: false
password:
toc: true
tags: ["js"]
mathjax: false
summary:
categories: ["笔记"]
---

# 术语索引

- 可枚举性： 可枚举是集合的属性，隐式要求每个元素都是唯一的，`for ... in`
- 迭代： 可迭代 迭代是将输出做为输入,再次进行相同动作处理。`for ...of`
- 递归： 自己调用自己，自己包含自己。从计算机角度讲，递归是迭代的特例

> [遍历、枚举、迭代](https://www.jianshu.com/p/dec28ac0175f)
> 递归是重复调用函数自身实现循环。迭代是函数内某段代码实现循环，而迭代与普通循环的区别是：循环代码中参与运算的变量同时是保存结果的变量，当前保存的结果作为下一次循环计算的初始值。

# 骚操作

- 实现 sleep 阻塞目的:
  1. while()限定时间条件达到
  ```js
  const expire = Date.now() + 1000
  while(Date.now()<expire)
  // after that, do something
  ```
  2. promise 异步
  ```js
  new promise((reslove, reject) = > {
      setTimeout  (()=>{
          reslove()
      }, 2000)
  })
  ```

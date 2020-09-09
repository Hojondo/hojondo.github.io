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

  # 冷知识

  [赋值详详详详解](https://www.cnblogs.com/52cik/p/js-assignment-operators.html)
  [神奇的JS连续赋值语句](http://xiaoyuze88.github.io/blog/2013/09/20/%E7%A5%9E%E5%A5%87%E7%9A%84JS%E8%BF%9E%E7%BB%AD%E8%B5%8B%E5%80%BC%E8%AF%AD%E5%8F%A5)
  JS 赋值语句 永远返回值，声明语句 永远返回 undefined
  JavaScript 中赋值语句的返回值就是等号右边的值，例子中 obj.getThis 的值是一个匿名函数，非严格模式下 this 会指向 window

  ```js
  var obj = {
    name: "My Object",
    getThis: function () {
      console.log(this);
    },
  };
  (obj.getThis = obj.getThis)(); // window
  //
  if (x = 10) // 会判断为10，隐式转换为true
  ```

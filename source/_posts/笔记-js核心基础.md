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

# 解构赋值
1. 解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象，字符串被转为类数组对象，数值和布尔型则转为包装对象。由于 undefined 和 null 无法转为对象，所以对它们进行解构赋值，都会报错。
  ```js
  const [a, b, c, d, e] = 'hello';
  a // "h"
  b // "e"
  c // "l"
  d // "l"
  e // "o"

  //字符串转为的对象有length属性
  let {length : len} = 'hello';
  len // 5

  let {toString: s} = 123;
  s === Number.prototype.toString // true

  let {toString: s} = true;
  s === Boolean.prototype.toString // true

  let { prop: x } = undefined; // TypeError
  let { prop: y } = null; // TypeError
  ```
2. 数组是特殊的对象，所以可以对数组进行对象属性的解构
  ```js
  let arr = [1, 2, 3];
  let {0 : first, [arr.length - 1] : last} = arr;
  first // 1
  last // 3
  ```
3. 数组的解构赋值也可以这样使用 `let [,m] = [2,3]`
4. 设置解构赋值的默认值
  ```js
  function move({x = 0, y = 0} = {}) {
    return [x, y];
  }
  ```
5. 如果解构赋值语句不是变量声明语句（前面没有var，let，const），即对已经声明的变量进行进行解构赋值需要注意加上括号 *注：数组的解构赋值[x,y]=[y,x]好像不用加括号*
```js
let x;
;({x} = {x: 1}); // 和立即执行函数一样，该语句的前面一行最好加上分号，否则可能会被当做函数调用
```
6. 解构赋值 可以直接提取 JSON 数据，当作object处理
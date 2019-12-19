---
title: babel中polyfill和plugin-transform-runtime的区别
top: false
cover: false
toc: true
mathjax: false
date: 2019-12-18 11:29:05
tags: ['babel'']
password:
summary:
categories:
---
# 简答

Babel 默认只是转换了 箭头函数 let ,Promise 和 includes 都没有转换 ,这是为什么

Babel 把 Javascript 语法 分为 `syntax` 和 `api`

先说 api , api 指那些我们可以通过 函数重新覆盖的语法 ，类似 includes,map,includes,Promise,凡是我们能想到重写的都可以归属到 api

啥子是 syntax ,像 箭头函数，let,const,class, 依赖注入 Decorators,等等这些，我们在 Javascript 在运行是无法重写的，想象下，在不支持的浏览器里不管怎么样，你都用不了 let 这个关键字

千万要get到上面这2个点，非常重要,很多人以为只要 引用了 Babel 就不会出现兼容性问题了，这个是大错特错的

syntax 这个关键字 Babel 的官网只是一笔带过，直译又不准确，网上很多文章在说 polyfill 和 transform-runtime 的差别都没说到点上，还互相瞎鸡儿抄，这个点上小编还是很自信的，按照自己的理解,说出二者的差别(默默的给自己加个鸡腿)

那 Babel 只负责 转换 syntax , includes,map,includes 这些 API 层面的 怎么办, Babel 把这个放在了 单独放在了 polyfill 这个模块处理

Babel 这个设计非常好, 把 Javascript 语法抽象成2个方面的, syntax 和 polyfill 独立开来，分而治理，6to5 一开始设计是把二者放在一起的，大家想想 polyfill 随着浏览器的不同，差异是非常大的,2个要是在一起 代码的耦合性就太大了，到处都是if else

# babel-polyfill 使用场景

Babel 默认只转换新的 JavaScript 语法，而不转换新的 API。例如，Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象，以及一些定义在全局对象上的方法（比如 Object.assign）都不会转译。如果想使用这些新的对象和方法，必须使用 babel-polyfill，为当前环境提供一个垫片。

# babel-runtime 使用场景

Babel 转译后的代码要实现源代码同样的功能需要借助一些帮助函数，例如，{ [name]: 'JavaScript' } 转译后的代码如下所示：

```js
'use strict';
function _defineProperty(obj, key, value) {
  if (key in obj) {
    Object.defineProperty(obj, key, {
      value: value,
      enumerable: true,
      configurable: true,
      writable: true
    });
  } else {
    obj[key] = value;
  }
  return obj;
}
var obj = _defineProperty({}, 'name', 'JavaScript');
```

类似上面的帮助函数 _defineProperty 可能会重复出现在一些模块里，导致编译后的代码体积变大。Babel 为了解决这个问题，提供了单独的包 `babel-runtime` 供编译模块复用工具函数。

启用插件`babel-plugin-transform-runtime` 后，Babel 就会使用 `babel-runtime` 下的工具函数，转译代码如下：

```js
'use strict';
// 之前的 _defineProperty 函数已经作为公共模块 `babel-runtime/helpers/defineProperty` 使用
var _defineProperty2 = require('babel-runtime/helpers/defineProperty');
var _defineProperty3 = _interopRequireDefault(_defineProperty2);
function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }
var obj = (0, _defineProperty3.default)({}, 'name', 'JavaScript');
```

除此之外，babel 还为源代码的非实例方法（`Object.assign`，实例方法是类似这样的 `"foobar".includes("foo")`）和 babel-runtime/helps 下的工具函数自动引用了 polyfill。这样可以避免污染全局命名空间，非常适合于 JavaScript 库和工具包的实现。例如 `const obj = {}, Object.assign(obj, { age: 30 });` 转译后的代码如下所示：

```js
'use strict';
// 使用了 core-js 提供的 assign
var _assign = require('babel-runtime/core-js/object/assign');
var _assign2 = _interopRequireDefault(_assign);
function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }
var obj = {};
(0, _assign2.default)(obj, {
  age: 30
});
```



思考：babel-runtime 为什么适合 JavaScript 库和工具包的实现？

1. 避免 babel 编译的工具函数在每个模块里重复出现，减小库和工具包的体积；
2. 在没有使用 babel-runtime 之前，库和工具包一般不会直接引入 polyfill。否则像 Promise 这样的全局对象会污染全局命名空间，这就要求库的使用者自己提供 polyfill。这些 polyfill 一般在库和工具的使用说明中会提到，比如很多库都会有要求提供 es5 的 polyfill。在使用 babel-runtime 后，库和工具只要在 package.json 中增加依赖 babel-runtime，交给 babel-runtime 去引入 polyfill 就行了；

总结：

具体项目还是需要使用 babel-polyfill，只使用 babel-runtime 的话，实例方法不能正常工作（例如 `"foobar".includes("foo")`）；

JavaScript 库和工具可以使用 babel-runtime，在实际项目中使用这些库和工具，需要该项目本身提供 polyfill；

疑问：像 antd@2.x 这样的库使用了 babel-runtime，在实际项目中使用 antd@2.x，我们需要引入 babel-polyfill。但全部 polyfill 打包压缩下来也有 80kb 左右，其中很多 polyfill 是没有用到的，如何减少体积呢？手工一个个引入使用到的 polyfill，似乎维护成本太高！

参考文献

[知乎专栏：Babel学习系列4-polyfill和runtime差别(必看)](https://zhuanlan.zhihu.com/p/58624930)
[Segmentfault问题：babel的polyfill和runtime的区别](https://segmentfault.com/q/1010000005596587)


---
title: 代码检查 eslint & tslint
top: true
cover: false
toc: true
mathjax: false
date: 2019-09-18 10:50:16
tags: ['js', '代码规范']
password:
summary:
categories:
---
# 代码检查 eslint & tslint

目前 TypeScript 的代码检查主要有两个方案：使用 [TSLint](https://palantir.github.io/tslint/)或使用 [ESLint](https://eslint.org/)+ [`typescript-eslint-parser`](https://github.com/eslint/typescript-eslint-parser)

## 代码规范

如果你写自己的项目怎么折腾都没关系，但是在公司中老板希望每个人写出的代码都要符合一个统一的规则，这样别人看源码就能够看得懂，因为源码是符合统一的编码规范制定的。

那么问题来了，总不能每个人写的代码老板都要一行行代码去检查吧，这是一件很蠢的事情。凡是重复性的工作，都应该被制作成工具来节约成本。这个工具应该做两件事情：

- 提供编码规范；
- 提供自动检验代码的程序，并打印检验结果：告诉你哪一个文件哪一行代码不符合哪一条编码规范，方便你去修改代码。

Lint 因此而诞生。

## 使用Eslint

1. 确保node npm环境下

2. 安装npm包 `$ npm install eslint --save-dev`

3. `package.json`添加`script`

   ```json
   "scripts": {
       "test": "react-scripts test --env=jsdom",
       "lint": "eslint src", // 让 Lint 自动检验 src 目录下所有的 .js 文件
       "lint:create": "eslint --init"
   }
   ```

4. 创建`.eslintrc.js/json` 和 `.eslintignore` 文件；或通过`eslint --init`命令初始化，在 [eslint规则表](http://eslint.cn/docs/rules/) 

5. 编译器工具配置： [webstorm配置](https://segmentfault.com/q/1010000013857167/a-1020000013857503)， [vscode配置]()

示例：

```json
{
  "root": true,
  "parserOptions": {
    "parser": "babel-eslint",
    "ecmaVersion": 6,
    "sourceType": "module"
  },
  "env": {
    "browser": true,
    "commonjs": true,
    "es6": true,
    "node": true
  },
  "extends": [
     "eslint:recommended",
    "google",
    "plugin:vue/recommended"
  ],
  "plugins": [
    "vue"
  ],
  "rules": {
    "comma-dangle": [
      "error",
      "never"
    ],
    "quote-props": [
      "error",
      "as-needed"
    ],
    "max-len": [
      0
    ],
    "vue/max-attributes-per-line": ["error", {
      "singleline": 3,
      "multiline": {
        "max": 1,
        "allowFirstLine": true
      }
    }],
    "valid-jsdoc": [0],
    "linebreak-style":[0]
  }
}
```



- eslint-loader ，用于webpack 配置 preLoaders

- 结合pre-commit，用于git hooks，[教程](https://juejin.im/entry/58a65e6f61ff4b006c481016#%E7%BB%93%E5%90%88pre-commit%E4%BD%BF%E7%94%A8)

...to do loading

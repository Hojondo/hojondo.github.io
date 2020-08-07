---
title: MockJS
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-19 19:39:39
tags: ["接口"]
password:
summary:
categories: ["笔记"]
---

# MockJS

## 数据模板定义规范

```json
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```

## 生成规则 有 7 种格式

生成规则是*可选的*；有 _7 种格式(String, Number, Boolean, Object, Array, Function, Regexp)_： 其含义*依赖* 属性值的类型 确定

- **`'name|min-max':val`**

  1.  _String_(生成重复次数在 min-max 的字符串);
  2.  _Number_(生成一个大于等于 min、小于等于 max 的整数，属性值 number 只是用来确定类型);
  3.  _Boolean_(随机生成一个布尔值，值为 value 的概率是 min/(min + max)，值为!value 的概率是 max/(min + max));
  4.  _Object_(从属性值 object 中随机选取 min 到 max 个属性)
  5.  _Array_ (通过重复属性值 `array` 生成一个新数组，重复次数大于等于 min，小于等于 max)

- **`'name|count':val`**

  1.  _String_(重复 count 次生成字符串);
  2.  _Boolean_(count=1，随机生成一个布尔值，值为 true 的概率是 1/2，值为 false 的概率同样是 1/2);
  3.  _Object_(从属性值 val 中随机选取 count 个属性);
  4.  _Array_
      1. count!=1：(通过重复属性值 `array` 生成一个新数组，重复次数为 `count`)
      2. count=1：(从属性值 `array` 中随机选取 1 个元素，作为最终值)

- **`'name|+step':val`**

  1.  _Number_(属性值自动加 1，初始值为 step);
  2.  _Array_(从属性值 `array` 中间隔 step 个数顺序选取 1 个元素，作为最终值)

- 浮点数

  - **`'name|min-max.dmin-dmax':val`**
    1.  _Number_(生成一个浮点数，整数部分大于等于 min、小于等于 max，小数部分保留 dmin 到 dmax 位);
  - **`'name|min-max.dcount':val`**
    1.  _Number_(生成一个浮点数,小数点前 min-max 数值范围随机整数，小数点后 dcount 位随机小数);
  - **`'name|count.dmin-dmax':val`**
    1.  _Number_(生成一个浮点数,小数点前 count，小数点后 dmin-dmax 位随机);
  - **`'name|count.dcount':val`**
    1.  _Number_(生成一个浮点数,小数点前 count，小数点后 dcount 位随机小数);

- **`'name': function`**

  执行函数 `function`，取其返回值作为最终的属性值，函数的上下文 this 为该属性 `'name'` 所在的对象。

- **`'name': regexp`**

  根据正则表达式 `regexp` 反向生成可以匹配它的字符串。用于生成自定义格式的字符串。

## 方法

### `Mock.mock()`

4 种参数：都可选

- `rurl` 表示需要拦截的 URL，可以是 URL 字符串或 URL 正则；例：/\/domain\/list\.json/`、`'/domian/list.json'

- `rtype` 表示需要拦截的 Ajax 请求类型；例： `GET`、`POST`、`PUT`、`DELETE` 等

- `template` 表示数据模板，可以是对象或字符串；例： `{ 'data|1-10':[{}] }`、`'@EMAIL'`

- `function(options)` 表示用于生成响应数据的函数；`options`指向本次请求的 Ajax 选项集，含有 `url`、`type` 和 `body` 三个属性。

### `Mock.setup(settings)`

配置拦截 Ajax 请求时的行为。参数 settings 支持的配置项目前只有一个：`timeout`

```js
Mock.setup({
  timeout: 400, // 400 毫秒 后才会返回响应内容
});
Mock.setup({
  timeout: "200-600", // 响应时间介于 200 和 600 毫秒之间
});
// 默认值是'10-100'
```

### `Mock.Random`

是一个工具类，用于生成各种随机数据。

Mock.Random 的方法在数据模板中称为『占位符』，书写格式为 @占位符(参数 [, 参数])

- 方法

  ```js
  Mock.mock({ email: "@email" });
  // => { email: "v.lewis@hall.gov" }
  Random.increment();
  // => 1
  Random.increment(100);
  // => 101
  Random.increment(1000);
  // => 1101
  ```

  | Type 大类     | Method（详见[Mock 文档](https://github.com/nuysoft/Mock/wiki/Basic)）                                                                                                                                              |
  | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
  | Basic         | boolean, natural*(自然数,>=0 的整数)*, integer*(整数)*, float, character*(字符)*, string*(字符串)*, range*(整型数组)*                                                                                              |
  | Time          | date*(日期字符串)*, time*(时间字符串)*, datetime*(日期和时间字符串)*, now*(当前的日期和时间字符串)*                                                                                                                |
  | Image         | image*(图片地址)*, dataImage*(Base64 图片编码)*                                                                                                                                                                    |
  | Color         | color*(格式为 '#RRGGBB')*, hex*(格式为 '#RRGGBB')*, rgb*(格式为 'rgb(r, g, b)')*, rgba*(格式为 'rgba(r, g, b, a)')*, hsl*(格式为 'hsl(h, s, l)')*                                                                  |
  | Text          | paragraph*(一段文本)*, cparagraph*(一段中文文本)*, sentence*(一个首字母大写的句子)*, csentence*(一个中文句子)*, word*(一个单词)*, cword*(一个汉字)*, title*(一句每个单词首字母大写的标题)*, ctitle*(一句中文标题)* |
  | Name          | first*(英文名)*, last*(英文姓)*, name*(英文姓名)*, cfirst*(中文姓)*, clast*(中文名)*, cname*(中文姓名)*                                                                                                            |
  | Web           | url*(一个 URL)*, protocol*(一个 URL 协议:http/ftp/telnet/...)*, domain*(域名)*, tld*(顶级域名:net/com/...)*email*(邮件地址)*, ip,                                                                                  |
  | Address       | region*(中国大区:华北/…)*, province*(中国省)*, city*(中国市)*, county*(中国县)*, zip*(六位邮政编码)*                                                                                                               |
  | Helper        | capitalize*(首字母转为大写)*, upper*(字符串转为大写)*, lower*(字符串转为小写)*, pick*(数组中随机选取一个元素返回)*, shuffle*(原数组乱序返回)*                                                                      |
  | Miscellaneous | guid*(随机生成一个 GUID)*, id*(18 位身份证)*, increment*(全局的自增整数)*                                                                                                                                          |

- 扩展

  ```js
  Random.extend({
    constellation: function (date) {
      var constellations = [
        "白羊座",
        "金牛座",
        "双子座",
        "巨蟹座",
        "狮子座",
        "处女座",
        "天秤座",
        "天蝎座",
        "射手座",
        "摩羯座",
        "水瓶座",
        "双鱼座",
      ];
      return this.pick(constellations);
    },
  });
  Random.constellation();
  // => "水瓶座"
  Mock.mock("@CONSTELLATION");
  // => "天蝎座"
  Mock.mock({
    constellation: "@CONSTELLATION",
  });
  // => { constellation: "射手座" }
  ```

### `Mock.valid(template, data)`

校验真实数据 `data` 是否与数据模板 `template` 匹配。2 个参数，必选

```js
const tempObj = { "user|1-3": [{ name: "@cname", "id|18-28": 88 }] };
const realData = { user: [{ name: "张三", id: 90 }] };
console.log(Mock.valid(tempObj, realData));
// 如果都匹配，则会返回空数组[],否则返回不匹配的项组成的数组,元素是包含action, actual, expected, message, path, type 等属性
```

### `Mock.toJSONSchema()`

把 Mock.js 风格的数据模板 `template` 转换成 [JSON Schema](https://json-schema.org/)。

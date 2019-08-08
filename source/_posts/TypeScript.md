---
title: TypeScript
tags: ["ts"]
---
# TypeScript

TypeScript 是 JavaScript 的一个超集，主要提供了**类型系统**和**对 ES6 的支持**，它由 Microsoft 开发，代码开源[GitHub](https://github.com/Microsoft/TypeScript) 上。

## 特点

可以在编译阶段就发现大部分错误，这总比在运行时候出错好

## 基础

```typescript
function alertName(): void {
    alert('My name is Tom');
}
```



- 原始数据类型

  JavaScript 的类型分为两种：原始数据类型（Primitive data types）和对象类型（Object types）。
  原始数据类型包括：布尔值、数值、字符串、null、undefined 以及 ES6 中的新类型 Symbol。

  1. 布尔值`boolean`

      `let isDone: boolean = false;`

  2. 数值`number`

     `let decLiteral: number = 6;`

  3. 字符串`string`

     `let myName: string = 'Tom';`

  4. 空值`void`

     JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数

  
  
  1. `null` 和 `undefined`
  
     > 与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量：

     `let u: undefined = undefined;`

     `let n: null = null;`

- 任意值`any`

  `let myFavoriteNumber: any = 'seven';`

  1. 如果是一个普通类型，在赋值过程中改变类型是不被允许的，但如果是 `any` 类型，则允许被赋值为任意类型。
  2. 声明一个变量为**任意值**之后，对它的任何操作，返回的内容的类型都是任意值
  3. 任意值上访问任何属性都是允许，也允许调用任何方法
  4. 变量如果在声明的时候，未指定其类型，那么它会被识别为**任意值**类型

- 类型推论

  > 1. 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。
  >
  >       例：`let myFavoriteNumber = 'seven';` 等价于 `let myFavoriteNumber: string = 'seven';`
  >
  > 2. 如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 `any` 类型而完全不被类型检查

- 联合类型`xx|oo`

  > 1. 取值可以为多种类型中的一种
  >
  >    例：`let myFavoriteNumber: string | number;`
  >
  > 2. 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**
  >
  > 3. 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型

  ```typescript
  function getString(something: string | number): string {
    	return something.length;// 报错，因为length不是共有属性
      return something.toString();
  }
  
  let myFavoriteNumber: string | number;
  myFavoriteNumber = 'seven';
  console.log(myFavoriteNumber.length); // 5，被推断成了 string 类型
  myFavoriteNumber = 7;
  console.log(myFavoriteNumber.length); // 编译时报错，被推断成了 number 类型
  ```

- 对象的类型 - 接口`interfaces`

  在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型。

  ```typescript
  interface Person { // 定义了一个接口 Person，接口一般首字母大写
      name: string; // 【确定属性】
      age: number;
  }
  let tom: Person = { // 定义了一个变量 tom，它的类型是 Person，约束tom的形状(内部属性方法)必须和接口 Person 一致，不多不少
      name: 'Tom',
      age: 25
  };
  ```

  ```typescript
  interface Person {
  		readonly id: number; //【只读属性】对象中的该字段 约束：只能在第一次赋值对象的时候被赋值该属性
      name: string;
      age?: number; // 【可选属性】 含义是该属性可以不存在。但仍不允许添加未定义的属性
    	[propName: string]: any; // 【任意属性】任意取 string 类型的值。并且强调：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集
  }
  let tom: Person = {
      name: 'Tom'
  };
  ```

- 数组的类型

  多种表示方法：

  1. 「类型 + 方括号」`let fibonacci: number[] = [1, 1, 2, 3, 5];` 不允许出现其他类型的项

  2. 数组范型（Array Generic） `let fibonacci: Array<number> = [1, 1, 2, 3, 5];`

  3. 用接口`interface`表示数组 

     ```typescript
     interface NumberArray {
         [index: number]: number; // 只要 index 的类型是 number，那么值的类型必须是 number
     }
     let fibonacci: NumberArray = [1, 1, 2, 3, 5];
     ```

  **类数组不是数组类型**

  事实上常见的类数组都有自己的接口定义，如 `IArguments`, `NodeList`, `HTMLCollection` 等

  ```typescript
  function sum() {
      let args: IArguments = arguments;
  }
  ```

- 函数的类型

  两种定义函数的方式 - 函数声明 和 函数表达式

  ```typescript
  // 函数声明式
  function sum(x: number, y: number): number { // 要把输入和输出都考虑到约束数据类型
      return x + y;
  }
  sum(1, 2, 3); // 报错 入参个数必须不多不少
  
  // 函数表达式
  let mySum = function (x: number, y: number): number {
      return x + y;
  };// 事实上，这样的代码只对等号右侧的匿名函数进行了类型定义，左侧是通过赋值操作进行类型推论而推断出来的
  let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
      return x + y;
  };
  // 注：在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。不同于ES6的箭头函数
  ```

  ```typescript
  // 用接口定义函数的形状
  interface SearchFunc {
      (source: string, subString: string): boolean;
  }
  let mySearch: SearchFunc;
  mySearch = function(source: string, subString: string) {
      return source.search(subString) !== -1;
  }
  ```

  ```typescript
  // 用？表示【可选参数】，可选参数必须接在必需参数后面
  // TypeScript 会将添加了【默认值】的参数识别为【可选参数】，不受「可选参数必须接在必需参数后面」的限制
  function buildName(firstName: string, midName: string = 'cat', lastName?: string) {
      if (lastName) {
          return firstName + ' ' + lastName;
      } else {
          return firstName;
      }
  }
  let tomcat = buildName('Tom', 'Cat');
  let tom = buildName('Tom');
  ```

  ```typescript
  // ES6 中，可以使用 ...rest 的方式获取函数中的剩余参数（rest 参数）,事实上，items 是一个数组。所以我们可以用数组的类型来定义它.
  function push(array: any[], ...items: any[]) { // 注意，rest 参数只能是最后一个参数
      items.forEach(function(item) {
          array.push(item);
      });
  }
  let a = [];
  push(a, 1, 2, 3);
  ```

  ```typescript
  // 【重载】允许一个函数接受不同数量或类型的参数时，作出不同的处理
  function reverse(x: number): number;
  function reverse(x: string): string; // 重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现
  function reverse(x: number | string): number | string {
      if (typeof x === 'number') {
          return Number(x.toString().split('').reverse().join(''));
      } else if (typeof x === 'string') {
          return x.split('').reverse().join('');
      }
  } // TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。
  ```

- 类型断言

- 声明文件

- 内置对象

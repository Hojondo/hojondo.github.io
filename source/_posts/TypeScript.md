---
title: TypeScript
tags: ['语言','js']
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-02 16:02:21
password:
summary:
categories:
---

# TypeScript

TypeScript 是 JavaScript 的一个超集，主要提供了**类型系统**和**对 ES6 的支持**，它由 Microsoft 开发，代码开源[GitHub](https://github.com/Microsoft/TypeScript) 上。

## 特点

可以在编译阶段就发现大部分错误，这总比在运行时候出错好

## 基础

```typescript
// TypeScript 中，使用 : 指定变量的类型，: 的前后有没有空格都可以。
function alertName(): void {
    alert('My name is Tom');
}
```

### 原始数据类型

JavaScript 的类型分为两种：原始数据类型（Primitive data types）和对象类型（Object types）。
原始数据类型包括：布尔值、数值、字符串、null、undefined 以及 ES6 中的新类型 Symbol。

1. 布尔值`boolean`

    `let isDone: boolean = false;`

2. 数值`number`

   `let decLiteral: number = 6;`

3. 字符串`string`

   `let myName: string = 'Tom';`

4. 空值`void`

   > JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数
>
   > ```typescript
   > function alertName(): void {
   >     alert('My name is Tom');
   > }
   > ```

5. `null` 和 `undefined`

   > 与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量：
   >
   > ```typescript
   > let u: undefined = undefined;
   > let n: null = null;
   > 
   > let num: number = undefined;// 这样不会报错
   > let u: void;
   > let num: number = u;// 而 void 类型的变量不能赋值给 number 类型的变量
   > ```

### `Any`任意值

`let myFavoriteNumber: any = 'seven';myFavoriteNumber = 7;`

>   1. 如果是一个普通类型，在赋值过程中改变类型是不被允许的，但如果是 `any` 类型，则允许被**赋值**为任意类型。
>   2. 声明一个变量为任意值之后，对它的任何操作，返回的内容的**类型**都是任意值
>   3. 任意值上访问任何**属性**都是允许，也允许调用任何**方法**
>   4. 变量如果在声明的时候，**未指定**其类型，那么它会被识别为任意值类型

### `Object`对象的类型 - 接口

在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型。

*TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。*

```typescript
interface Person { // 定义了一个接口 Person，接口一般首字母大写
    name: string; // 【确定属性】
    age: number;
}
let tom: Person = { // 定义对象变量tom，约束其形状(内部属性方法和数量)必须和接口 Person 一致，不多不少
    name: 'Tom',
    age: 25
};
```

```typescript
interface Person {
		readonly id: number; //【只读属性】对象中的该字段 约束：只能在第一次赋值对象的时候被赋值该属性
    name: string;
    age?: number; // 【可选属性】 含义是该属性可以不存在。但仍不允许添加未定义的属性
  	[propName: string]: any; // 【任意属性】任意取 string 类型的值。并且强调：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集,比如[propName: string]: string; 会让age 报错
}
let tom: Person = {
    name: 'Tom',
  	gender: 'male'
};
```

### `Array`数组的类型

1. 「类型 + 方括号」`let fibonacci: number[] = [1, 6, 2, 3, 5];` 不允许出现其他类型的项

2. 数组范型（Array Generic） `let fibonacci: Array<number> = [1, 1, 2, 3, 5];`

3. 用接口`interface`表示数组 

   ```typescript
   interface NumberArray {
       [index: number]: number; // 只要 index 的类型是 number，那么值的类型必须是 number
   }
   let fibonacci: NumberArray = [1, 1, 2, 3, 5];
   ```
   
4. 用接口表示类数组

   ```typescript
   function sum() {
       let args: number[] = arguments; // 报错，Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more；arguments 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口
       let args: {
           [index: number]: number;
           length: number;
           callee: Function;
       } = arguments;
       let args: IArguments = arguments; // 事实上常见的类数组都有自己的接口定义，如 `IArguments`, `NodeList`, `HTMLCollection` 等 内置对象
   }
   ```

5. 任意类型`any`

   `let list:any[]=['xcatliu',25,{website:'http://xcatliu.com'}];`

### `Function`函数的类型

两种定义函数的方式 - 函数声明 和 函数表达式

1. 函数声明

   ```typescript
   // 1.函数声明
   function sum(x: number, y: number): number { // 要把输入和输出都考虑到约束数据类型
       return x + y;
   }
   sum(1, 2, 3); // 报错 入参个数必须不多不少
   ```

2. 函数表达式

   ```typescript
   // 2.函数表达式
   let mySum = function (x: number, y: number): number {
       return x + y;
   };// 事实上，这样的代码只对等号右侧的匿名函数进行了类型定义，左侧是通过赋值操作进行类型推论而推断出来的
   let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
       return x + y;
   };// 注：在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。不同于ES6的箭头函数
   ```

3. 接口 定义函数形状

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
// 【可选参数】：用？表示【可选参数】，可选参数必须接在必需参数后面
// 【默认值】：TypeScript 会将添加了【默认值】的参数识别为【可选参数】，不受「可选参数必须接在必需参数后面」的限制
// 【剩余参数】：ES6 中，可以使用 ...rest 的方式获取函数中的【剩余参数】（rest 参数）,事实上，items 是一个数组。所以我们可以用数组的类型来定义它.rest 参数只能是最后一个参数
function buildName(firstName: string, midName: string = 'cat', array: any[], lastName?: string,  ...items: any[]) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
    items.forEach(function(item) {
        array.push(item);
    });
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
buildName('Tom', underfined, [], 1, 2, 3);
```

```typescript
// 【重载】：允许一个函数接受不同数量或类型的参数时，作出不同的处理
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

### 联合类型`xx|oo`

取值可以为多种类型中的一种

`let myFavoriteNumber: string | number;`

```typescript
/*访问联合类型的属性或方法的情况：当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法*/
function getString(something: string | number): string {
  	return something.length;// 报错，因为length不是 string 和 number 的共有属性
    return something.toString();
}
/*联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型*/
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 被推断成了 string 类型
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错，被推断成了 number 类型
```


### 类型推论

1. 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

   `let myFavoriteNumber = 'seven';` 等价于 `let myFavoriteNumber: string = 'seven';`

2. 如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 `any` 类型而完全不被类型检查


### 类型断言

手动指定一个值的类型（通常在[联合类型中不确定哪个类型时只能访问所有类型共有的属性和方法]使用）`<类型>值` 或 `值 as 类型`

```typescript
function getLength(something: string | number): number {
    if ((<string>something).length) { // 或 if ((something as string).length)
        return (<string>something).length;
    } else {
        return something.toString().length;
    }
}
```

### 声明文件

声明语句：当我们想使用第三方库 jQuery时，我们需要使用 `declare var`来定义它的类型，例：`declarevarjQuery:(selector:string)=>any;`

声明文件：通常我们会把声明语句放到一个单独的文件（`jQuery.d.ts`）中，这就是声明文件，必须以 `.d.ts`为后缀；

推荐的是使用 `@types`统一管理第三方库的声明文件，例：`npminstall@types/jquery --save-dev`

手动写声明文件[具体操作](https://ts.xcatliu.com/basics/declaration-files)

> 创建一个 `types`目录，专门用来管理自己写的声明文件，将 `foo`的声明文件放到 `types/foo/index.d.ts`中。这种方式需要配置下 `tsconfig.json`中的 `paths`和 `baseUrl`字段。
>
> ```json
> {
>     "compilerOptions": {
>         "module": "commonjs",
>         "baseUrl": "./",
>         "paths": {
>             "*": ["types/*"]
>         }
>     }
> }
> ```
>
> 

### 内置对象

`Boolean` `Error` `Date` `RegExp`

`Document` `HTMLElement` `Event` `NodeList` 等DOM、BOM对象

```typescript
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```



## 进阶

### `type`类型别名

给类型 定义个变量

```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

### `type`字符串字面量类型

约束取值只能是某几个字符串中的一个。

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}
handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dbclick'); // 报错，event 不能为 'dbclick'
// index.ts(7,47): error TS2345: Argument of type '"dbclick"' is not assignable to parameter of type 'EventNames'.
```

### `Tuple`元组

数组合并相同类型的对象，而元组（Tuple）合并不同类型的对象，并且当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型

`let tom:[string,number]=['Tom',25];`

### `enum`枚举

用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。枚举成员会被赋值为从 `0`开始递增的数字，同时也会对枚举值到枚举名进行反向映射

`enumDays {Sun,Mon,Tue,Wed,Thu,Fri,Sat};`

枚举项有两种类型：**常数项**（constant member）和**计算所得项**（computed member）。[判断条件](https://ts.xcatliu.com/advanced/enum#chang-shu-xiang-he-ji-suan-suo-de-xiang)

```typescript
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat}; // 给枚举项手动赋值； Days["Sat"] === 6
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"}; // 手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 tsc 无视类型检查 (编译出的 js 仍然是可用的) Days["Fri"] = 12;
enum Days {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat}; // 手动赋值的枚举项也可以为小数或负数，此时后续未手动赋值的项的递增步长仍为1 Days["Tue"] === 2.5

enum Color {Red, Green, Blue = "blue".length}; // 计算所得项
```

- 普通枚举

  即上述例子

- 常数枚举`const enum`

  会在编译阶段被删除，并且不能包含计算成员，如果包含了计算成员会在编译阶段报错

- 外部枚举`declare enum`

  只会用于编译时的检查，编译结果中会被删除。外部枚举与声明语句一样，常出现在声明文件中

- 同时常数和外部也是可以的`declare const enum`

### `class`类


---
title: TypeScript
top: true
cover: false
toc: true
mathjax: false
date: 2019-09-02 16:02:21
tags: ["TS"]
password:
summary:
categories: ["笔记"]
---

# TypeScript

TypeScript 是 JavaScript 的一个超集，主要提供了**类型系统**和**对 ES6 的支持**，它由 Microsoft 开发，代码开源[GitHub](https://github.com/Microsoft/TypeScript) 上。

## 特点

可以在编译阶段就发现大部分错误，这总比在运行时候出错好

## 基础

```typescript
// TypeScript 中，使用 : 指定变量的类型，: 的前后有没有空格都可以。
function alertName(): void {
  alert("My name is Tom");
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
   >   alert("My name is Tom");
   > }
   > ```

5. `null` 和 `undefined`

   > 与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量：
   >
   > ```typescript
   > let u: undefined = undefined;
   > let n: null = null;
   >
   > let num: number = undefined; // 这样不会报错
   > let u: void;
   > let num: number = u; // 而 void 类型的变量不能赋值给 number 类型的变量
   > ```

### `Any`任意值

`let myFavoriteNumber: any = 'seven';myFavoriteNumber = 7;`

> 1. 如果是一个普通类型，在赋值过程中改变类型是不被允许的，但如果是 `any` 类型，则允许被**赋值**为任意类型。
> 2. 声明一个变量为任意值之后，对它的任何操作，返回的内容的**类型**都是任意值
> 3. 任意值上访问任何**属性**都是允许，也允许调用任何**方法**
> 4. 变量如果在声明的时候，**未指定**其类型，那么它会被识别为任意值类型

### `Object`对象的类型 - 接口

在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型。

_TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。_

```typescript
interface Person {
  // 定义了一个接口 Person，接口一般首字母大写
  name: string; // 【确定属性】
  age: number;
}
let tom: Person = {
  // 定义对象变量tom，约束其形状(内部属性方法和数量)必须和接口 Person 一致，不多不少
  name: "Tom",
  age: 25,
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
  name: "Tom",
  gender: "male",
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

   `let list:any[]=['xcatliu',25,{website:'https://xcatliu.com'}];`

### `Function`函数的类型

两种定义函数的方式 - 函数声明 和 函数表达式

1. 函数声明

   ```typescript
   // 1.函数声明
   function sum(x: number, y: number): number {
     // 要把输入和输出都考虑到约束数据类型
     return x + y;
   }
   sum(1, 2, 3); // 报错 入参个数必须不多不少
   ```

2. 函数表达式

   ```typescript
   // 2.函数表达式
   let mySum = function (x: number, y: number): number {
     return x + y;
   }; // 事实上，这样的代码只对等号右侧的匿名函数进行了类型定义，左侧是通过赋值操作进行类型推论而推断出来的
   let mySum: (x: number, y: number) => number = function (
     x: number,
     y: number
   ): number {
     return x + y;
   }; // 注：在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。不同于ES6的箭头函数
   ```

3. 接口 定义函数形状

   ```typescript
   // 用接口定义函数的形状
   interface SearchFunc {
     (source: string, subString: string): boolean;
   }
   let mySearch: SearchFunc;
   mySearch = function (source: string, subString: string) {
     return source.search(subString) !== -1;
   };
   ```

```typescript
// 【可选参数】：用？表示【可选参数】，可选参数必须接在必需参数后面
// 【默认值】：TypeScript 会将添加了【默认值】的参数识别为【可选参数】，不受「可选参数必须接在必需参数后面」的限制
// 【剩余参数】：ES6 中，可以使用 ...rest 的方式获取函数中的【剩余参数】（rest 参数）,事实上，items 是一个数组。所以我们可以用数组的类型来定义它.rest 参数只能是最后一个参数
function buildName(
  firstName: string,
  midName: string = "cat",
  array: any[],
  lastName?: string,
  ...items: any[]
) {
  if (lastName) {
    return firstName + " " + lastName;
  } else {
    return firstName;
  }
  items.forEach(function (item) {
    array.push(item);
  });
}
let tomcat = buildName("Tom", "Cat");
let tom = buildName("Tom");
buildName("Tom", underfined, [], 1, 2, 3);
```

```typescript
// 【重载】：允许一个函数接受不同数量或类型的参数时，作出不同的处理
function reverse(x: number): number;
function reverse(x: string): string; // 重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现
function reverse(x: number | string): number | string {
  if (typeof x === "number") {
    return Number(x.toString().split("").reverse().join(""));
  } else if (typeof x === "string") {
    return x.split("").reverse().join("");
  }
} // TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。
```

### 联合类型`xx|oo`

取值可以为多种类型中的一种

`let myFavoriteNumber: string | number;`

```typescript
/*访问联合类型的属性或方法的情况：当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法*/
function getString(something: string | number): string {
  return something.length; // 报错，因为length不是 string 和 number 的共有属性
  return something.toString();
}
/*联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型*/
let myFavoriteNumber: string | number;
myFavoriteNumber = "seven";
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
  if ((<string>something).length) {
    // 或 if ((something as string).length)
    return (<string>something).length;
  } else {
    return something.toString().length;
  }
}
```

### 声明文件

声明语句：当我们想使用第三方库 jQuery 时，我们需要使用 `declare var`来定义它的类型，例：`declarevarjQuery:(selector:string)=>any;`

声明文件：通常我们会把声明语句放到一个单独的文件（`jQuery.d.ts`）中，这就是声明文件，必须以 `.d.ts`为后缀；

推荐的是使用 `@types`统一管理第三方库的声明文件，例：`npminstall@types/jquery --save-dev`

手动写声明文件[具体操作](https://ts.xcatliu.com/basics/declaration-files)

> 创建一个 `types`目录，专门用来管理自己写的声明文件，将 `foo`的声明文件放到 `types/foo/index.d.ts`中。这种方式需要配置下 `tsconfig.json`中的 `paths`和 `baseUrl`字段。
>
> ```json
> {
>   "compilerOptions": {
>     "module": "commonjs",
>     "baseUrl": "./",
>     "paths": {
>       "*": ["types/*"]
>     }
>   }
> }
> ```

### 内置对象

`Boolean` `Error` `Date` `RegExp`

`Document` `HTMLElement` `Event` `NodeList` 等 DOM、BOM 对象

```typescript
let b: Boolean = new Boolean(1);
let e: Error = new Error("Error occurred");
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
  if (typeof n === "string") {
    return n;
  } else {
    return n();
  }
}
```

### `type`字符串字面量类型

约束取值只能是某几个字符串中的一个。

```typescript
type EventNames = "click" | "scroll" | "mousemove";
function handleEvent(ele: Element, event: EventNames) {
  // do something
}
handleEvent(document.getElementById("hello"), "scroll"); // 没问题
handleEvent(document.getElementById("world"), "dbclick"); // 报错，event 不能为 'dbclick'
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
enum Days {
  Sun = 7,
  Mon = 1,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
} // 给枚举项手动赋值； Days["Sat"] === 6
enum Days {
  Sun = 7,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat = <any>"S",
} // 手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 tsc 无视类型检查 (编译出的 js 仍然是可用的) Days["Fri"] = 12;
enum Days {
  Sun = 7,
  Mon = 1.5,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
} // 手动赋值的枚举项也可以为小数或负数，此时后续未手动赋值的项的递增步长仍为1 Days["Tue"] === 2.5

enum Color {
  Red,
  Green,
  Blue = "blue".length,
} // 计算所得项
```

- 普通枚举

  即上述例子

- 常数枚举`const enum`

  会在编译阶段被删除，并且不能包含计算成员，如果包含了计算成员会在编译阶段报错

- 外部枚举`declare enum`

  只会用于编译时的检查，编译结果中会被删除。外部枚举与声明语句一样，常出现在声明文件中

- 同时常数和外部也是可以的`declare const enum`

### `class`类

- 类(Class)：定义了一件事物的抽象特点，包含它的属性和方法
- 对象（Object）：类的实例，通过 `new`生成
- 面向对象（OOP）的三大特性：封装、继承、多态
- 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据
- 继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性
- 多态（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 `Cat`和 `Dog`都继承自 `Animal`，但是分别实现了自己的 `eat`方法。此时针对某一个实例，我们无需了解它是 `Cat`还是 `Dog`，就可以直接调用 `eat`方法，程序会自动判断出来应该如何执行 `eat`
- 存取器（getter & setter）：用以改变属性的读取和赋值行为
- 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 `public`表示公有属性或方法
- 抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现
- 接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（implements）。一个类只能继承自另一个类，但是可以实现多个接口

```javascript
/**
在原生js中
*/
class Animal {
  // 使用 class 定义类
  constructor(name) {
    // 使用 constructor 定义构造函数
    this.name = name;
  }
  name = "Jack"; // ES7 提案中 直接在类里面定义实例属性
  static num = 42; // ES7 提案中 使用 static 定义一个静态属性
  sayHi() {
    return `My name is ${this.name}`;
  }
  /*存取器*/
  get name() {
    // 使用 getter 和 setter 可以改变属性的赋值和读取行为
    return "Jack";
  }
  set name(value) {
    console.log("setter: " + value);
  }
  /*静态方法*/
  static isAnimal(a) {
    // 它们不需要实例化，而是直接通过类来调用
    return a instanceof Animal;
  }
}
let a = new Animal("Jack"); // setter: Kitty; 构造并且走set方法
console.log(a.sayHi()); // My name is Jack
a.name = "Tom"; // setter: Tom;
console.log(a.name); // Jack; 走get方法
Animal.isAnimal(a); // true; 走静态方法
a.isAnimal(a); // TypeError: a.isAnimal is not a function
/* 继承 */
class Cat extends Animal {
  // 使用 extends 关键字实现继承
  constructor(name) {
    super(name); // 子类中使用 super 关键字 调用父类的构造函数constructor
    console.log(this.name);
  }
  sayHi() {
    return "Meow, " + super.sayHi(); // 使用 super 关键字 调用父类的 sayHi()
  }
}
let c = new Cat("Tom"); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

#### 修饰符

TypeScript 可以使用三种访问修饰符和一个关键字

- `public` 默认，修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public`的
- `private`修饰的属性或方法是私有的，不能在声明它的类的外部访问
- `protected`修饰的属性或方法是受保护的，它和 `private`类似，区别是它在子类中也是允许被访问的
- `readonly` 只读属性关键字，相比 private 可以访问不可赋值，只允许出现在属性声明或索引签名中

```typescript
class Animal {
  public name;
  /* public constructor (public name) { // 修饰符还可以使用在构造函数参数中，等同于类中定义该属性，使代码更简洁。 */
  public constructor(name) {
    // 当构造函数修饰为 private 时，该类不允许被继承或者实例化；当构造函数修饰为 protected 时，该类只允许被继承：
    this.name = name;
  }
}
let a = new Animal("Jack");
console.log(a.name); // Jack
a.name = "Tom";
console.log(a.name); // Tom
/* private */
class Animal {
  private name;
  public constructor(name) {
    this.name = name;
  }
}
class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name); // 报错，在子类中也是不允许访问的；而如果是用 protected 修饰，则允许在子类中访问：
  }
}
let a = new Animal("Jack");
console.log(a.name); // Jack 虽然也报错，但是需要注意的是，TypeScript 编译之后的代码中，并没有限制 private 属性在外部的可访问性
a.name = "Tom"; // 报错，不能在类外赋值

/* readonly */
class Animal {
  // readonly name;
  public constructor(public readonly name) {
    // 当和其他访问修饰符同时存在的话，需要写在其后面
    this.name = name;
  }
}
let a = new Animal("Jack");
console.log(a.name); // Jack
a.name = "Tom"; // 报错
```

#### 抽象类`abstract class`

- 抽象类 不允许被实例化
- 抽象类中的抽象方法必须被子类实现

```typescript
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}
class Cat extends Animal {
  public sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}
let cat = new Cat("Tom");
```

#### 类的类型

类加上 TypeScript 的类型，与接口类似

```typescript
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sayHi(): string {
    return `My name is ${this.name}`;
  }
}
let a: Animal = new Animal("Jack");
console.log(a.sayHi()); // My name is Jack
```

#### 类实现接口

实现（implements）是面向对象中的一个重要概念。

一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些**共有的特性**，这时候就可以把特性提取成接口（interfaces），用 `implements`关键字来实现。这个特性大大提高了面向对象的灵活性。

接口(interfaces)不止用于[定义对象的类型](#toc-heading-6)，和[定义函数形状](#toc-heading-8)，在这里还可以用于实现(implements)。

```typescript
interface Alarm {
  alert();
}
interface Light {
  lightOn();
  lightOff();
}
class SecurityDoor extends Door implements Alarm {
  alert() {
    console.log("SecurityDoor alert");
  }
}
class Car implements Alarm, Light {
  // 一个类可以实现多个接口
  alert() {
    console.log("Car alert");
  }
  lightOn() {
    console.log("Car light on");
  }
  lightOff() {
    console.log("Car light off");
  }
}

/* 接口继承接口 */
interface LightableAlarm extends Alarm {
  lightOn();
  lightOff();
}
/* 接口继承类 */
class Point {
  x: number;
  y: number;
}
interface Point3d extends Point {
  z: number;
}
let point3d: Point3d = { x: 1, y: 2, z: 3 };

/* 接口定义函数时，函数有自己属性方法 */
interface Counter {
  (start: number): string;
  interval: number;
  reset(): void;
}
function getCounter(): Counter {
  let counter = <Counter>function (start: number) {};
  counter.interval = 123;
  counter.reset = function () {};
  return counter;
}
let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

### 泛型

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```typescript
function createArray<T>(length: number, value: T): Array<T> {
  // 在函数名后添加 <T>，其中 T 用来指代任意输入的类型，在后面的输入 value: T 和输出 Array<T> 中即可使用了。
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
createArray<string>(3, "x"); // ['x', 'x', 'x']
/* 定义多个类型参数 */
// function createArray<T = string>(length: number, value: T): Array<T> { // 可以给泛型参数指定默认类型
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}
swap([7, "seven"]); // ['seven', 7]
```

**泛型约束**

在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它(泛型变量)的属性或方法，这时，我们可以对泛型进行约束，只允许这个函数传入那些包含 `length`属性的变量

```typescript
interface Lengthwise {
  length: number;
}
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  // 使用了 extends 约束泛型 T 必须符合接口 Lengthwise 的形状，也就是必须包含 length 属性
  console.log(arg.length);
  return arg;
}
/* 例2 多个类型参数之间也可以互相约束 */
function copyFields<T extends U, U>(target: T, source: U): T {
  // 要求 T 继承 U，保证 U 上不会出现 T 中不存在的字段
  for (let id in source) {
    target[id] = (<T>source)[id];
  }
  return target;
}
let x = { a: 1, b: 2, c: 3, d: 4 };
copyFields(x, { b: 10, d: 20 });
```

#### 泛型接口

含有泛型的接口定义一个函数的形状

```typescript
interface CreateArrayFunc {
  <T>(length: number, value: T): Array<T>;
}
let createArray: CreateArrayFunc;
/* 把泛型参数提前到接口名上
interface CreateArrayFunc<T> {
    (length: number, value: T): Array<T>;
}
let createArray: CreateArrayFunc<any>;
*/
createArray = function <T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
};
createArray(3, "x"); // ['x', 'x', 'x']
```

#### 泛型类

泛型也可以用于类的类型定义中

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}
let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

### 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型

#### 函数合并 - 重载

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if (typeof x === "number") {
    return Number(x.toString().split("").reverse().join(""));
  } else if (typeof x === "string") {
    return x.split("").reverse().join("");
  }
}
```

#### 接口合并

```typescript
/* 接口中的属性在合并时会简单的合并到一个接口中 */
interface Alarm {
  price: number;
  alert(s: string): string;
}
interface Alarm {
  weight: number;
  alert(s: string, n: number): string;
}
// -> 合并为
interface Alarm {
  price: number;
  weight: number;
  alert(s: string): string;
  alert(s: string, n: number): string;
}

// 并且合并的重名属性的类型必须是相同的否则报错
interface Alarm {
  price: number;
}
interface Alarm {
  price: number; // 虽然重复了，但是类型都是 `number`，所以不会报错
  weight: number;
}
```

#### 类合并

_有处矛盾，待更新_

---

ts 的核心之一就是对值/数据所具有的结构进行类型检查

## ts 配置文件 tsconfig.json

例：

```json
{
  "compilerOptions": {
    "target": "es3",
    "outDir": "./dist",
    "strctNullChecks": true
  },
  "includes": ["./src/**/*"]
}
```

ts 和 eslint 的区别：理解成
eslint 是语法规范，包括缩进 变量命名 大小写 单双引号 还有值限定
ts 是值的限定，帮助提示值/数据类型安全，比如某对象的可用属性或方法

## 类型系统

- 类型标注（签名）格式`变量: 标注类型`

  - 基础类型(基元)： `string`, `number`, `boolean`
  - `null` 和 `undefined`
    1. 默认情况下 `null` 和 `undefined` 是所有类型的子类型。即 所有类型(除了`null`和`undefined`)的值都可以被赋值为 null/undefined 值
    2. 默认一个变量声明时未赋值，则该变量值为 undefined，类型为`any`
    3. 在 ts 配置文件中 添加 `strctNullChecks: true`，可以检测值可能为 null 的情况，以智能提示 对值进行容错处理，使程序更加严谨
  - Object 类型,用接口（Interfaces）
    - 对象字面量的类型标注
      ```ts
      let obj: { x: number; y: string } = {};
      ```
    - 内置对象类型
      `Date`, `Set`, `Array`
      ```ts
      let d1: Date = new Date();
      ```
    - 包装对象
      `String`, `Number`, `Boolean`类和基础`string`类不一样
      ```ts
      let Str: String = new String("sss");
      let st: string = "sss";
      Str.split(" ");
      // st.split() 错
      Str = "s2";
      // st = new String('s2') 错
      ```
      可以理解成`string`类型是`String`类型的子类
  - `number[]`或`string[]`... 数组类型 用泛型（Generics）
    解释：在数据结构中数组的定义是：存储**相同类型**的**有序**集合，但是 js 中的 Array 并没有遵守相同类型规则
    泛形：例`let a: Array<number> = []`
  - 元组 类型
    元组概念类似数组，但是允许元素类型不唯一相同，但是保留规则：1. 初始化数据的**个数**和**对应位置标注类型**必须一致，2.越界数据必须是元组标注允许的类型之一
    例： `let d: [string, number] = ['xx', 2]`
    `d.push(3)` 可以
    `d.push(false)` 错
    _注意：如果开启了开启 strctNullChecks 则必须在声明变量的时候就初始化值 到满足第 1 个规则，如果不开启 strctNullChecks，则会默许声明变量时可以是 undefined 并且后续 push 的值放在对应位置可以不满足第 1 条规则：相同位置一一对应，但是仍需满足第 2 条规则？：类型之一_
    _例允许：let d2: [string, number];d2.push(2)_
  - 枚举 类型
    枚举概念： 其作用是组织收集一组相关联数据。一般常用于定义 constants 常量.
    格式： `enum NAME {key1=val1, key2=val2}`
    其中 key 的命名和变量命名规则一致，所有 value 类型必须 number 或 string 二选一，且一致(ts 并不限定两者同时用但是不建议)，不写 val1 的话默认是 0 开始的数字 number 枚举
    如：
    ```ts
    enum Http_code {
      OK = 200,
      NoT_FOund = 404,
    }
    Http_code.OK; // 200
    ```
  - `void` （函数）无返回值类型
    函数的默认标注类型，标注无返回值的函数
  - `never` （函数）永远无返回值类型
    表示一个函数永不可能执行 return，比如 throw new Error('xx')结尾
    `never`类型同 null/undefined 一样也是 其他所有类型的子类；所有其他类型也都不可以赋值给 never 类型，包括 any 类型
  - `any` 类型
    1. 任何值都可以赋值给 any 类型，2.any 类型可以赋值给任意类型， 3. any 类型有任意属性和方法
    ```ts
    let a: any;
    a = 1;
    let b: string;
    b = a; // 不会报错...any类型可以赋值给其他任何类型，
    ```
    可以理解成 any 类型被 ts 编译器放弃了进行类型检测，同时 IDE 的智能提示也忽略
    _建议给 tsconfig.json 配置 noImplicitAny: true 以禁止存在隐式 any 类型_
  - `unknow` 类型
    ts3.0 新增，理解成安全版的 any，与 any 的区别：
    - unkonw 只能赋值给 unknow 或 any，不能赋值给其他任何类型
    - unknow 没有任何属性和方法
  - 函数 类型
    函数是一等公民，ts 中同 js 中一样也是一种数据。
    格式：

    ```ts
    function add(x: number, y: string): string {
      return x + y;
    }
    // 例：
    function foreach(data: string[], cb: (k:number, v: string)=>void): void {
      for(let i:number = 0;i<data.length;i++>){
        cb(i, data[i])
      }
    }
    foreach(['sss','ddd'], function(kk, vvvv){

    })
    ```

    后续学习 函数重载和数组泛形，接口
    todo

- 类型检测
  即 编译器在编译过程中根据标注的类型进行检测，使数据使用更安全，帮住减少错误，IDE 也可以提供智能提示

  ## 接口 类型

  对复杂的对象类型进行标注的方式之一；或者说是给其他代码定义的一种契约（比如 Class）

  ```ts
  interface Point {
    x: number;
    y: string; // 可以是,或;进行分隔
    z: boolean[];
  }
  let p1: Point = {
    x: 1,
    y: "s",
    z: [false, true],
  };
  ```

  - 可选属性`key?: type`, 该属性可有可无
  - 只读属性`readonly key: type`，该属性只能初始化时赋值，后续不能再次被赋值
  - 任意属性`[prop: key-type]: value-type`，
    - key-type 必须是 string 或 number 二选一；
    ```ts
    interface IAny {
      x: number;
      y: string;
      [prop: string]: number;
    }
    let ia: IAny = {
      x: 1,
      y: "ss",
      dfs: 2,
    };
    ia.xxsdfs = 5;
    ia[1] = 1; // 注意这里[]内是数字可以，因为js中对象中属性key可以是number，但是[]中即使是数字也会变成字符串key，原则上说对象的所有键名都是字符串（ES6 又引入了 Symbol 值也可以作为键名），所以加不加引号都可以，只不过遇到number的话会被解释器自动转成'number'； 换句话说 key-type是string的话，对象的属性可以是string也可以是number 即ia['sss']或ia[2]都可，但是key-type是number的话，只能是ia[2]
    ```
    _object 对应的 key 没有限制，只是如果是数字，取值的时候就不能用英文句号(.)，只能用中括号的方式取值。详细规定见[链接](https://www.cnblogs.com/canger/p/6382944.html)和[阮一峰博客](https://javascript.ruanyifeng.com/grammar/object.html)_
    - 当同时存在 key-type 是 string 和 number 时，`[prop: number]: `的`value-type`必须是`[prop: string]:`的`value-type`相同类型或者其**子类型**
    ```ts
    interface IBoth {
      x: number;
      [prop: string]: Object;
      [prop: number]: Date; // 可以
    }
    interface IBoth2 {
      x: number;
      [prop: string]: string;
      [prop: number]: boolean; // 不可以
    }
    ```

## 深入了解 类型

### 联合类型

多选类型，当我们希望标注一个变量为多个类型之一时
`变量: 类型1 ｜ 类型2`

```ts
function fn1(x: number, y: string | boolean): string | boolean {
  return y;
}
```

### 交叉类型

合并类型，把多种类型合并到一起成为一种新的类型，包括各个类型的特点
`变量: 类型1 & 类型2`

```ts
interface o1 {
  x: number;
  y: string;
}
interface o2 {
  z: boolean;
}
let obj1: o1 = { x: 1, y: "s" };
let obj2: o2 = { z: false };
let obj3: o1 & o2 = Object.assign({}.obj1, obj2);
```

### 字面量类型

```ts
function setPosition(els: Element, direction: "left" | "right" | "center") {
  // left. top, right, bottom
}
let box = document.querySelector(".box");
if (box) {
  setPosition(box, "center 这里只能从3个固定值之中选一");
}
```

### 类型别名

对于对象类型，可以用 interface 来提出来 多次多处使用，但是对于非对象类型来说可以用类型别名来复用

```ts
// 如上例子
type dir = "left" | "right" | "center";
function setPosition(ele: Element, direction: dir) {
  // 如上上个例子
  type o3 = o1 & o2;
}
```

### 类型推导

基于使用情况，比如每次声明变量/函数时都要手动写一遍类型，会很累赘，所以有了类型推导机制，即 ts 编译器会自动根据上下文推导出对应的类型标注，但是这种机制只会出现在

- 变量初始化时
- 函数形参 提供默认值时
- 函数 返回值的类型判定 也会根据函数内 return 值来推导

```ts
function fn(x = 2, y = "s") {
  return x + y;
}
// 相当于
function fn(x: number = 2, y: string = "s"): string {
  return x + y;
}
```

### 类型断言

只是一种预判，类似于类型转换，但是并非真正的转换了，强制告诉 ts 编译器说这个变量一定会是就是这个类型。
例：

```ts
// 以下两种格式都可以
let img = <HTMLInageElment>document.querySelector(".box");
let img = document.querySelector(".box") as HTMLInageElment;
// 这样的话 img变量一定会有src属性了
if (img) {
  img.src;
}
```
### 类型操作符
### 类型保护
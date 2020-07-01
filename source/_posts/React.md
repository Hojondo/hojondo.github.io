---
title: React
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-19 18:55:19
tags: ['js', 'react']
password:
summary:
categories:
---
# React
[demo地址](https://github.com/Hojondo/reactDemo)
React 的世界里一切皆是组件，我们使用class语法构建一个最基本的组件，组件的使用方式和HTML相同，组件的render函数返回页面渲染的一个JSX，然后使用ReactDom渲染到页面里

## React 和 Vue的对比
组件化方面
1. 模块化： 从代码的角度进行分析，把一些可复用的代码抽离成单个模块，便于项目维护和开发；如Node
2. 组件化：从UI界面角度进行分析，把一些可复用的UI元素，抽离为单独的组件。
3. Vue如何实现组件化：`.vue`文件
   - template 结构
   - script 行为
   - style 样式
4. React如何实现组件化：一切以js来表现。ES6、ES7（async和await...）

移动APP开发体验方面

- VUE是 结合Week（阿里）
- React, 结合ReactNative

## 几个核心概念

1. **虚拟DOM**

   为了实现DOM元素的高效更新，性能问题，频繁操作DOM

   需要**按需渲染**，

   获取内存中的新旧DOM树，对比，按需更新DOM，浏览器中并无直接提供获取DOM树的API，因此 用js对象模拟新旧DOM树的虚拟DOM出现了。

2. **DIFF算法**

   新旧DOM树的对比

   - tree diff： 新旧两颗DOM树，逐层对比的过程
   - Component diff：在进行tree diff过程中，每一层中 组件级别的对比。
   - Element diff ：在进行component diff过程中，如果两个组件类型相同，则进行元素级别的对比。



实践步骤：

### webpack4.x 和3的简述差别

约定大于配置，如 约定的入口路径是 src -> index.js，不用再写entry字段

新增mode值

## 项目实践流程

1. `npm init -y`

2. 建目录src和dist

3. 安装webpack4及相关

   `npm i webpack webpack-cli -D`

   `npm i webpack-dev-server -D`

4. 新建`webpack.config.js`，配置

   

5. 安装react

   `npm i react react-dom -S`

   react包是专门用来创建组件和js虚拟DOM的，同时组件的生命周期都在这个包里

   React-dom是 专门进行DOM渲染和操作的，最主要的应用场景是`ReactDOM.render()`

6. 在`index.html`中 创建容器以供入口js调用render

   ```html
   <div id="app"></div>
   ```

7. 导入包 在入口文件`index.js`

   ```js
   import React from 'react'
   import ReactDom from 'react-dom'
   
   // 创建虚拟DOM元素
   const myDiv = React.createElement('div', {id: 'xx',style: {display: 'block'}}, '哈哈哈', React.createElement('p',{},'2'));
   
   // const myDiv = <div id="d1">第一个div元素</div>;
   
   // 使用ReactDom 吧虚拟DOM渲染到页面上
   // 参数1 要渲染的那个虚拟DOM元素
   // 参数2 指定页面上的一个DOM容器
   ReactDom.render(myDiv, document.getElementById('app'));
   ```

   

8. 安装`babel`插件

   `npm i babel-core babel-loader babel-plugin-transform-runtime -D`

   语法包 `npm i babel-preset-env babel-preset-stage-0 -D`

   能够识别转换jsx的语法包`npm i babel-preset-react -D`

9. `Webpack.config.js`中配置babel-loader

10. 配置`.babelrc`配置文件

   ```json
   "presets": ["env","stage-0","react"],
   "plugins": []
   ```

   

## JSX语法

- jsx语法的本质：并不是直接把jsx渲染到页面上，而是内部先转换成了createElement形式，再渲染的

- jsx中混合写入js表达式：在jsx语法中把js代码写在`{}`中

  包括：渲染数字、字符串、布尔值、为属性绑定值、渲染jsx元素、渲染jsx元素数组、将普通字符数组转为jsx数组并渲染到页面上

- 注释

- 标签必须成堆出现，如果是单标签，则必须自闭合

- 在jsx中创建DOM的时候，所有的节点，必须有唯一的跟元素进行包裹

- 特殊的属性，使用`className`来代替`class`，`htmlFor`代替label的`for`属性

- 标签上写style的话，要用js对象

- css模块化

  `npm i style-loader css-loader -D`；

  配置webpack.config.js的rules
  ```js
   // webpack.config.js 
   {
        test: /\.scss$/,
        use: ['style-loader', {
            loader: 'css-loader',
            options: {
                modules: {
                    localIdentName: '[path][name]-[local]-[hash:5]'
                },
                sourceMap: true
            }
        }, 'sass-loader']
    },
   ```

  组件中`import cssobj from './demoStyle.css'`

  给css模块化`use['css-loader?modules']`，但是模块化只针对.class和#id选择器，不会将标签选择器模块化

  给css自定义生成类名格式`use['css-loader?localIdentName=[path][name][local][hash:6]']`

   其可选参数是：
   - [path]表示样式表相对于项目根目录所在路径
   - [name]表示样式表文件名称
   - [local]表示样式的类名定义名称
   - [hash:length]表示最多32位的hash值

  css可选`:local()`(开启modules之后默认 不用写)或者全局`:global(.xxclass)`(不会被模块化)

  对于第三方的样式表，规定都以.css结尾，这样的话 我们不会为普通的.css启用模块化 直接import 'bootstrap'就好，自己的样式表都以.scss或.less结尾，于是 只为`.scss`或`.less`文件启用模块化

  安装`npm i sass-loader node-sass -D`
- 绑定事件
   事件绑定机制，相对于原生，是小驼峰命名，事件值必须是函数
   `button onClick={function(){}}>按钮</button>`
   `<button className="Btn" onClick={() => this.myClickFn()}>点击</button>`



## 组件 - 创建组件的两种方式
- 父组件向子组件传递数据
- 使用{...obj}属性扩散传递数据
- 注意：组件的名称必须是大写
- 将组件封装到单独的文件中
- 引入时省略`.jsx`后缀名

1. **函数式组件** - 使用构造函数创建组件
   没有state状态，可以使用Hooks
   如果要接受外间传递的数据，需要在构造函数的参数列表中使用props来接收
   必须向外return一个合法的jsx创建的虚拟dom（React元素）

   ```jsx
   import React, { useState, useEffect } from "react";
   export default function FunctionComponent(props) {
      const [date, setDate] = useState(new Date());
      useEffect(() => {
         //相当于componentDidMount、componentWillUnmount、componentDidUpdate的集合
         const timer = setInterval(() => {
            setDate(new Date());
         }, 1000);
         return () => clearInterval(timer);
      }, []);
      return (
         <div>
            <h3>FunctionComponent</h3>
            <p>{date.toLocaleTimeString()}</p>
         </div>
      );
   }
   ```

2. **CLASS组件** - 使用class关键自关键组件
   有state状态
   ```jsx
   import React, { Component } from "react";
   export default class ClassComponent extends Component {
      constructor(props) {
         super(props);
         //存储状态
         this.state = {
            date: new Date()
         };
      }
      //组件挂载完成之后执行
      componentDidMount() {
         this.timer = setInterval(() => {
            //更新state，不能用this.state
            this.setState({
            date: new Date()
            });
         }, 1000);
      }
      //组件卸载之前执行
      componentWillUnmount() {
         clearInterval(this.timer);
      }
      render() {
         const { date } = this.state;
         return (
            <div>
            <h3>ClassConponent</h3>
            <p>{date.toLocaleTimeString()}</p>
            </div>
         );
      }
   }
   ```

3. 两者的区别

   1. class关键子创建的组件有自己的私有数据和生命周期，但是function创建的组件 只有props，没有自己的私有数据和生命周期
   2. 用构造函数创建的组件：叫做“无状态组件”，无state和生命周期，但是运行效率较高，很少用
   3. 用class关键字创建出来的组件：叫做“有状态组件”


### Class 组件
#### setState

因为React 是单向绑定 单向数据流(js=>界面*例：对于input，需要三步，1手动监听onChange事件，2在事件中拿到最新的e.target.value，3调用this.setState({})*)，如果要为state中的数据重新赋值，要用react提供的`setState(updater, [callback])`
将 setState() 视为请求而不是立即更新组件的命令。React 会延迟调用它，然后通过一次传递更新多个组件。React 并不会保证 state 的变更会立即生效。因为他会是将对组件 state 的更改排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件。**在合成时间和生命周期中是异步的批量更新，在setTimeout和原生事件中和回调中是同步的**
通常第一个参数updater接受对象类型，如需基于之前的 state 来设置当前的 state，请将`updater`作为函数`(state, props) => stateChange`,updater 函数中接收的 state 和 props 都保证为最新。updater 的返回值会与 state 进行浅合并。



#### react的生命周期

**旧的生命周期：**

![image.png](https://i.loli.net/2020/06/29/YmHfXg3ie9hOQvG.png)

1. 组件创建阶段，只执行一次
   - `componentWillMount`
   - `render`
   - `componentDidMount`
2. 组件运行阶段，根据props属性或state状态数据的改变，有选择的执行0到多次
   - `componentWillReceiveProps(nextProps)`：
      1. 该方法只在props引起的组件更新过程中，才会被调用。state引起的组件更新并不会触发。
      2. nextProps是父组件传递给当前组件的新的props
      3. nextProps的值可能与子组件当前props的值相同，因此往往需要比较他俩的值来决定是否执行props发生变化后的逻辑。
      4. 在该方法中调用setState，只有render以及之后的方法中。this.state指向的才是更新后的state。在之前的shouldComponentUpdate、componentWillUpdate中。this.state指向的还是更新前的state
   - `shouldComponentUpdate`
   - `componentWillUpdate` 
   - `render`
   - `componentDidupdate`
      *shouldComponentUpdate 与componentWillUpdate中不能调用this.setState,否则会引起循环调用问题，render永远无法被调用，组件也永远无法渲染*
3. 组件销毁阶段，只执行一次
   - `componentWillUnmount`

**新的v16.4之后生命周期**

> 图示[来源](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
>
> ![image.png](https://i.loli.net/2019/12/27/ApcXH3ybk7KGd9h.png)

[生命周期详解 参考教程](https://juejin.im/post/5b6f1800f265da282d45a79a)

1. 挂载阶段

   - `constructor`

   - `static getDerivedStateFromProps(props, state)`*[新增]* ：
      1. 一个静态方法 基于props的派生state，会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。不能在此函数里面使用this，这个函数有两个参数props和state，分别指接收到的新参数和当前的state对象
      2. 此方法适用于罕见的用例，即 state 的值在任何时候都取决于 props。
      3. 该函数会在挂载时，接收到新的props，调用了`setState`和`forceUpdate`时被调用,这个方法就是为了取代之前的`componentWillMount`、`componentWillReceiveProps`和`componentWillUpdate`
      4. *getDeriveStateFromProps被设计成一个static方法，就是纯粹的获取父组件props，增量更新本组件的state*

   - ~~`componentWillMount/UNSAFE_componentWillMount`*[废弃]*~~

   - `render`

   - `componentDidMount`
      1. 依赖DOM节点的操作可以放到这个方法中。这个方法通常还用于向服务端请求数据

2. 更新阶段
   组件被挂载到DOM后，组件的props或state改变会引起组件的更新。
   props引起的更新，本质上是由渲染该组件的父组件引起的(也就是当父组件的render方法被调用时，组件会发生更新过程)，无论props是否改变，父组件render方法每一次调用，都会导致组件更新。
   state引起的组件更新，是通过this.setState修改组件的state触发的。

   - ~~`componentWillReceiveProps/UNSAFE_componentWillReceiveProps`*[废弃]*~~

   - `static getDerivedStateFromProps(props, state)`*[新增]* ：同上

   - `shouldComponentUpdate(nextProps,nextState)`： 
      1. 该方法决定组件是否继续执行更新过程。当该方法返回true(默认值)时继续执行，返回false时停止执行
      2. 一般通过比较nextProps、nextState与组件当前的props、state来决定返回值。
      3. 该方法可用来减少不必要的渲染，从而优化组件的性能

   - ~~`componentWillUpdate/UNSAFE_componentWillUpdate`*[废弃]*~~

   - `render`

   - `getSnapshotBeforeUpdate(prevProps, prevState)`*[新增]* ：
      1. 这个方法在`render`之后，`componentDidUpdate`之前调用，表示之前的属性和之前的state
      2. 这个函数有一个返回值，会作为第三个参数传给`componentDidUpdate`，如果你不想要返回值，可返回null，不写的话控制台会warning **此方法必须配合`componentDidUpdate`方法一起使用**
      3. *此方法 为了取代之前的`componentWillUpdate`*

   - `componentDidUpdate(prevProps, prevState, snapshot)`
      1. 当组件更新后，可以在此处对 DOM 进行操作。如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。（例如，当 props 未发生变化时，则不会执行网络请求）

3. 卸载阶段

   - `componentWillUnmount`

#### 组件复合
- 组件通过`props`属性，传递给子组件数据。
- 相当于VUE的`slot`，React中用`props.children`表示组件内子内容。

#### PureComponent
纯组件
即 定制了shouldComponentUpdate(nextProps,nextState){~~`return nextState.xx!==this.state.xx || nextProps.xx!==this.props.xx`~~}后的Component。比如如果赋予 React 组件相同的 props 和 state，render() 函数会渲染相同的内容，那么在某些情况下使用 React.PureComponent 可提高性能
- 必须要用**class组件**形式，⽽且要注意是**浅比较**，只比较一层，当xx是对象 重新赋值时会永远判断为相等导致子组件不会更新。*可使用`immutable对象`加速嵌套数据的比较* *或通过解构赋值/Object.assign给state第一层属性重新赋值新obj*
- 其中定制的`shouldComponentUpdate(nextProps,nextState)`将跳过所有子组件树的prop更新。因此，请确保所有子组件也都是纯组件，否则即使有自定义shouldComponentUpdate也会被warning&忽视
```js
export default class PureComponentPage1 extends PureComponent {}
```

### 函数组件
```js
import React, { useState } from "react"
export default function HookPage(props) { // 声明⼀一个叫 “count” 的 state 变量量，初始化为0 
const [count, setCount] = useState(0); return (
    <div>
      <h3>HookPage</h3>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>add</button>
</div>
); }
```
#### Hook使用规则
- 只能在 **函数最外层** 调用 Hook。不要在循环、条件判断或者子函数中调⽤。
- 只能在 **函数组件/自定义Hook** 中调用 Hook。不要在其他普通 JavaScript 函数中调用。

#### [几个Hook方法](https://zh-hans.reactjs.org/docs/hooks-reference.html)
> [Hook方法 & Class组件生命周期关联](https://github.com/sisterAn/blog/issues/34)
> [几个Hook方法详解](https://juejin.im/post/5dbbdbd5f265da4d4b5fe57d)
> [useImperativeHandle & forwardRef用法详解](https://juejin.im/post/5d8f478751882509563a03b3)

- `useState`
   `const [state, setState] = useState(initialState)`
   返回一个 state，以及更新 state 的函数。setState(newstate | prevCount => prevCount - 1) 函数用于更新 state。它接收一个新的 state 值并将组件的一次重新渲染加入队列，如果新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 setState。该函数将接收先前的 state，并返回一个更新后的值
- `useEffect`
   ```js
   useEffect(() => {
      // 在 componentDidMount，以及 []内变量 更改时 componentDidUpdate 执行的内容
      const subscription = props.source.subscribe();
      return () => {
         // 相当于 componentWillUnmount 执行的内容
         subscription.unsubscribe();
      };
   }, [props.source]);
   ```
   赋值给 useEffect 的函数会在组件渲染到屏幕之后执行**与 componentDidMount 和 componentDidUpdate相似**
- `useContext`
   useContext(MyContext) 相当于 class 组件中的 `static contextType = MyContext` 或者 `MyContext.Consumer`
   ```js
   const themes = {
      light: {
         foreground: "#000000",
         background: "#eeeeee"
      },
      dark: {
         foreground: "#ffffff",
         background: "#222222"
      }
   };
   const ThemeContext = React.createContext(themes.light);
   function App() {
      return (
         <ThemeContext.Provider value={themes.dark}>
            <Toolbar />
         </ThemeContext.Provider>
      );
   }
   function Toolbar(props) {
      return (
         <div>
            <ThemedButton />
         </div>
      );
   }
   function ThemedButton() {
      const theme = useContext(ThemeContext);
      return (
         <button style={{ background: theme.background, color: theme.foreground }}>
            I am styled by theme context!
         </button>
      );
   }
   ```
- `useReducer`
   `const [state, dispatch] = useReducer(reducer, initialArg, init)`
   useState 的替代方案。它接收一个形如 `(state, action) => newState` 的 reducer，并返回当前的 state 以及与其配套的 dispatch 方法。例如 state 逻辑较复杂且包含多个子值，或者下一个 state 依赖于之前的 state 等
   ```js
   const initialState = {count: 0};
   function reducer(state, action) {
      switch (action.type) {
         case 'increment':
            return {count: state.count + 1};
         case 'decrement':
            return {count: state.count - 1};
         default:
            throw new Error();
      }
   }

   function Counter() {
   const [state, dispatch] = useReducer(reducer, initialState);
   return (
      <>
         Count: {state.count}
         <button onClick={() => dispatch({type: 'decrement'})}>-</button>
         <button onClick={() => dispatch({type: 'increment'})}>+</button>
      </>
   );
   }
   ```
   惰性创建初始state，将init函数作为useReducer的第三个参数，初始state将被设置为`init(initialArg)`，这么做可以将用于计算 state 的逻辑提取到 reducer 外部，这也为将来对重置 state 的 action 做处理提供了便利
   ```js
   function init(initialCount) {
      return {count: initialCount};
   }
   function reducer(state, action) {
      switch (action.type) {
         case 'increment':
            return {count: state.count + 1};
         case 'decrement':
            return {count: state.count - 1};
         case 'reset':
            return init(action.payload);
         default:
            throw new Error();
      }
   }
   function Counter({initialCount}) {
      const [state, dispatch] = useReducer(reducer, initialCount, init);
      return (
         <>
            Count: {state.count}
            <button
            onClick={() => dispatch({type: 'reset', payload: initialCount})}>
            Reset
            </button>
            <button onClick={() => dispatch({type: 'decrement'})}>-</button>
            <button onClick={() => dispatch({type: 'increment'})}>+</button>
         </>
      );
   }
   ```
- `useMemo`
   `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])`
   缓存计算数据的值
   如下：在a和b的变量值不变的情况下，memoizedValue的值不变。即：useMemo函数的**第一个入参函数不会被执行**，从而达到节省计算量的目的
   > [useMemo & useCallback对比](https://juejin.im/post/5dd64ae6f265da478b00e639)
- `useCallback`
   ```js
   const memoizedCallback = useCallback(
      () => {
         doSomething(a, b);
      },
      [a, b],
   );
   ```
   缓存函数的引用
   如下：在a和b的变量值不变的情况下，memoizedCallback的引用不变。即：useCallback的**第一个入参函数会被缓存**，从而达到渲染性能优化的目的
   *useCallback(fn, deps) 相当于 useMemo(() => fn, deps) 。 注意依赖项数组不不会作为参数传给“创建”函数。虽然从概念上来说它表现为:所有“创建”函数中引⽤用的 值都应该出现在依赖项数组中。未来编译器器会更更加智能，届时⾃自动创建数组将成为可能*
- `useRef`
   `const refContainer = useRef(initialValue)`
   useRef 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期内保持不变。
   ```js
   function TextInputWithFocusButton() {
   const inputEl = useRef(null);
   const onButtonClick = () => {
      // `current` 指向已挂载到 DOM 上的文本输入元素
      inputEl.current.focus();
   };
   return (
      <>
         <input ref={inputEl} type="text" />
         <button onClick={onButtonClick}>Focus the input</button>
      </>
   );
   }
   ```
   请记住，当 ref 对象内容发生变化时，useRef 并不会通知你。变更 .current 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用回调 ref 来实现。
- `useImperativeHandle`
   `useImperativeHandle(ref, createHandle, [deps])`
   > [when to use this Hook](https://stackoverflow.com/questions/57005663/when-to-use-useimperativehandle-uselayouteffect-and-usedebugvalue)
   useImperativeHandle 应当与 [forwardRef](https://zh-hans.reactjs.org/docs/react-api.html#reactforwardref) 一起使用：
   forwardRef理解成把useRef实例的*指向`ref={xxRef}`*向下传递给某个元素实例，目的是让父组件可以操控子组件内元素
   而用了useImperativeHandle之后，useImperativeHandle将拿到forwardRef传递过来的ref，对该ref.current的具体实例值进行操控，

   ```jsx
   import React, { useRef, useImperativeHandle, forwardRef } from 'react'
   function Child(props,parentRef){
      // 子组件内部自己创建 ref 
      let focusRef = useRef();
      let inputRef = useRef();
      // 这个函数会返回一个对象
      // 该对象会作为父组件 current 属性的值
      // 通过这种方式，父组件可以操作子组件中的多个 ref
      useImperativeHandle(parentRef,()=>({
         focusRef,
         inputRef,
         name:'计数器',
         focus(){
               focusRef.current.focus();
         },
         changeText(text){
               inputRef.current.value = text;
         }
      }));
      return (
         <>
               <input ref={focusRef}/>
               <input ref={inputRef}/>
               {/* <input ref={parentRef} type="text"/> */}
         </>
      )
   }
   const FancyInputEle = forwardRef(Child);
   export default function Home(props) {
      const parentRef = useRef('s')
      return (
         <div>
               <button onClick={()=>{
                  parentRef.current.focus();
                  console.log(parentRef.current.focusRef.current.value);
                  // parentRef.current.addNumber(666);// 因为子组件中没有定义这个属性，实现了保护，所以这里的代码无效
                  parentRef.current.changeText('sd');
                  console.log(parentRef.current.name);
               }}>focus</button>
               <button onClick={()=>console.log(parentRef)}>value</button>
               <FancyInputEle ref={parentRef}></FancyInputEle>
         </div>
      )
   }
   ```
   1. 写在`forwardRef`内部，**限定子组件的哪些实例值可以被父组件暴漏获取/哪些属性方法可以被父组件调用,避免使用 ref 这样的命令式代码**&&**重定义事件方法使用** 
   2. 父组件可以操作子组件中的多个 ref
   3. 在大多数情况下，应当避免使用 ref 这样的命令式代码。

- `useLayoutEffect`
   其函数签名与 useEffect 相同，但它会**在所有的 DOM 变更之后同步调用 effect**。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，有点类似`componentWillMount`，useLayoutEffect 内部的更新计划将被同步刷新。
   ![image.png](https://i.loli.net/2020/07/01/fyaixejqVQI3L7E.png)
   [详情参考](https://juejin.im/post/5dbbdbd5f265da4d4b5fe57d#heading-23)
   - useEffect 在全部渲染完毕后才会执行
   - useLayoutEffect 会在 浏览器 layout 之后，painting 之前执行
   - 其函数签名与 useEffect 相同，但它会在所有的 DOM 变更之后同步调用 effect
   - 可以使用它来读取 DOM 布局并同步触发重渲染
   - 在浏览器执行绘制之前 useLayoutEffect 内部的更新计划将被同步刷新
   - 尽可能使用标准的 useEffect 以避免阻塞视图更新
- `useDebugValue`
   `useDebugValue(value, (value) => value.Fn())`
   在`自定义 hook`内部设置，可用于在 React 开发者工具中显示自定义 hook 的标签
   ```js
   function useFriendStatus(friendID) {
      const [isOnline, setIsOnline] = useState(null);
      // ...
      // 在开发者工具中的这个 Hook 旁边显示标签
      // e.g. "FriendStatus: Online"
      useDebugValue(isOnline ? 'Online' : 'Offline');
      return isOnline;
   }
   ```
   接受一个格式化函数作为可选的第二个参数，该函数只有在 Hook 被检查时才会被调用。它接受 debug 值作为参数，并且会返回一个格式化的显示值。
   例`useDebugValue(date, date => date.toDateString())`

#### 自定义Hook
自定义 Hook 是⼀个函数，其名称必须以 “use” 开头，函数内部可以调⽤其他的 Hook。

有时候我们会想要在组件之间重用一些状态逻辑。⽬前为止，有两种主流方案来解决这个问题:⾼阶组件和 render props。⾃定义 Hook 可以让你在不增加组件的情况下达到同样的目的。

```js
function useClock() {
   const [date, setDate] = useState(new Date());
   useEffect(() => {
      console.log("date effect"); //只需要在didMount时候执行就可以了 
      const timer = setInterval(() => {
         setDate(new Date());
      }, 1000); //清除定时器，类似willUnmount
      return () => clearInterval(timer);
   }, []);
   return date;
}
```


# React-router
react-router包含3个库，react-router、react-router-dom和react-router-native。根据应⽤运⾏的环境选择安装 react-router-dom(在浏览器器中使用)或react-router-native(在rn中使用)。react-router-dom和 react-router-native都依赖react-router，所以在安装时，react-router也会自动安装，创建web应⽤
`npm install --save react-router-dom`
## 基本组件
react-router中奉行⼀切皆组件的思想，路由器-Router、链接-Link、路由-Route、独占-Switch、重定向-Redirect都以组件形式存在
```js
import React, { Component } from "react";
import { BrowserRouter as Router, Route, Link,Switch } from "react-router-dom";
export default class RouterPage extends Component {
  render() {
    return (
      <div>
         <h3>RouterPage</h3>
         <Router>
            <Link to="/">⾸首⻚页</Link>
            <Link to="/user">⽤用户中⼼心</Link>
            {/* 根路路由要添加exact，实现精确匹配 */} 
            <Switch>
               <Route
                  exact
                  path="/"
                  component={HomePage}
                  //children={() => <div>children</div>}
                  //render={() => <div>render</div>}
               />
               <Route path="/user" component={UserPage} />
               <Route component={Page404}></Route>
            </Switch>
         </Router>
      </div>
); }
}
class HomePage extends Component {
  render() {
    return (
      <div>
        <h3>HomePage</h3>
      </div>
); }
}
class UserPage extends Component {
  render() {
    return (
      <div>
        <h3>UserPage</h3>
      </div>
 );
class Page404 extends Component {
  render() {
    return (
      <div>
        <h3>404</h3>
      </div>
 );
```
## Route渲染内容的三种方式
Route渲染方式互斥，优先级:`children`>`component`>`render`
  
1. `component={componentName}`
   只在当document.location匹配的时候渲染
2. `render={()=>{}}`
   只在当document.location匹配的时候渲染
3. `children:{()=>{}}`
   不管document.location是否匹配都会被渲染。工作方法与render完全一样
# React-redux
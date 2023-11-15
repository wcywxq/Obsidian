---
title: react hooks 相关问题
url: https://www.yuque.com/wcywxq/mxunh7/gq900o
---

<a name="t4vQD"></a>

## react 解决代码复用的主要方式

1. 高阶组件
2. `render props`
3. `react hooks` <a name="zMjPV"></a>

## 对 react hooks 的理解

<a name="PK1TM"></a>

### 没有提出 hooks 前存在的问题

- 在组件之间复用状态逻辑很难

`hook` 使你在无需修改组件结构的情况下复用状态逻辑

- 复杂组件变得难以理解

`hook` 将组件中相互关联的部分拆分成更小的函数(如：设置订阅或数据请求)

- 难以理解的 `class`

`hook` 使你在非 `class` 的情况下可以使用更多的 `react` 特性 <a name="yKncd"></a>

### react hooks 带来的便利

1. 跨组件复用变得更简单
2. 状态与 `ui` 隔离 <a name="lOzIh"></a>

### hook 使用规则

`hook` 就是 `javaScript` 函数，但是使用它们会有两个额外的规则：

1. 只能在函数最外层调用 `hook`。不要在循环、条件判断或者子函数中调用
   :::info
   因为 react hooks 内部是通过单向链表实现的，其内部的顺序是极其关键的，如果不按照规则使用，就会造成取值出现偏移的问题
   :::

2. 只能在 `react` 的函数组件中调用 `hook`。不要在其它 `javaScript` 函数中调用

3. 类组件不会被替换或废弃，不需要强制改造类组件，两种方式可以并存

<a name="NwYst"></a>

### 没有 hooks 之前，react 团队提出了哪些解决方法

- `render props` 和 高阶组件
  :::danger
  问题：但这种方式会很麻烦，使你的代码难以理解
  :::

- `mixin` 函数是复用类代码的一种途径，本质上可理解为类功能的扩展
  :::danger
  问题：

1. `mixins` 引入了不清晰的依赖关系
2. 会导致命名空间的冲突问题
3. 复杂度过高
   ::: <a name="uS1sI"></a>

### react hooks 如何保存状态

- `react hooks` 和类组件的状态值都被挂载到组件实例对象 `FirberNode` 和 `memorizedState` 属性中

`react hooks` 和类组件的数据结构完全不同，类组件是直接把 `state` 属性中挂载的这个开发者自定义的对象给保存到 `memorizedState` 属性中；而 `react hooks` 是用链表来保存状态的，`memorizedState` 属性保存的实际上是这个链表的头指针

- 所有的 `hooks` 都以**链表**的形式挂载在 `FiberNode.updateQueue` 中 <a name="akGBz"></a>

### HOC 和 hook 的区别

`HOC` 可以复用逻辑和视图，`hook` 只能复用逻辑 <a name="rEXAC"></a>

## react hooks 常用 api

<a name="tn70i"></a>

### 基础 hooks

1. useState：状态钩子，为函数组件提供内部状态
2. useEffect：副作用钩子，可以看作是 componentDidMount、componentDidUpdate 和 componentWillUnmount 的结合
3. useContext：共享钩子，在组件之间共享状态，可以解决 react 逐层通过 props 传递数据 <a name="mmsp9"></a>

### 额外的 hooks

1. useReducer：action 钩子，提供了状态管理，其基本原理是通过用户在页面上发起的 action，从而通过 reducer 方法来改变 state，从而实现页面和状态的通信，使用起来与 redux 类似
2. useCallback：缓存回调函数，避免传入的回调每次都是新的函数实例而导致依赖组件重新渲染，具有性能优化的效果
3. useMemo：把创建函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时重新计算，可以作为性能优化的手段
4. useRef：获取组件的实例，返回一个可变的 ref 对象，返回的 ref 对象在组件的整个生命周期内保持不变
5. useLayoutEffect：它会在所有 dom 变更后同步调用 effect <a name="pWIlT"></a>

## 常见问题

<a name="lrAUQ"></a>

### useState 使用浅析

- 使用方法

```javascript
const [state, setState] = useState(initialValue);
```

**每次渲染具有独立的状态值**（毕竟每次渲染都是完全独立的嘛）。也就是说，每个函数中的 state 变量只是一个简单的**常量**，每次渲染时从钩子中获取到的常量，并没有附着数据绑定之类的神奇魔法。 <a name="NlhjI"></a>

### useEffect 使用浅析

React 会在**每次渲染后都运行 Effect**。而依赖数组就是用来控制是否应该触发 Effect，从而能够减少不必要的计算，从而优化了性能。具体而言，只要依赖数组中的每一项与上一次渲染相比都没有改变，那么就跳过本次 Effect 的执行。 <a name="js4V1"></a>

### useCallback 使用浅析

为了解决函数在多次渲染中的引用相等（Referential Equality）问题，React 引入了一个重要的 Hook—— useCallback。官方文档介绍的使用方法如下：

```javascript
const memoizedCallback = useCallback(callback, deps);
```

第一个参数 callback 就是需要记忆的函数，第二个参数就是大家熟悉的 deps 参数，同样也是一个依赖数组（有时候也被称为输入 inputs）。在 Memoization 的上下文中，这个 deps 的作用相当于缓存中的键（Key），如果键没有改变，那么就直接返回缓存中的函数，并且确保是引用相同的函数。

在大多数情况下，我们都是传入空数组 \[] 作为 deps 参数，这样 useCallback 返回的就**始终是同一个函数，永远不会更新**。
:::warning
**提示：**
也许在刚开始学习 useEffect 的时候就发现：我们并不需要把 useState 返回的第二个 Setter 函数作为 Effect 的依赖。实际上，React 内部已经对 Setter 函数做了 Memoization 处理，因此每次渲染拿到的 Setter 函数都是完全一样的，deps 加不加都是没有影响的。
::: <a name="yzNnF"></a>

### useMemo

我们知道 useCallback 有个好基友叫 useMemo。还记得我们之前总结了 Memoization 的两大场景吗？useCallback 主要是为了解决函数的”**引用相等**“问题，而 useMemo 则是一个”**全能型选手**“，能够同时胜任引用相等和节约计算的任务。

实际上，useMemo 的功能是 useCallback 的**超集**。与 useCallback 只能缓存函数相比，useMemo 可以缓存任何类型的值（当然也包括函数）。useMemo 的使用方法如下：

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

其中第一个参数是一个函数，这个函数返回值的返回值（也就是上面 computeExpensiveValue 的结果）将返回给 memoizedValue 。因此以下两个钩子的使用是完全等价的：

```javascript
useCallback(fn, deps);
useMemo(() => fn, deps);
```

<a name="qWaYi"></a>

### useEffect 和 useLayoutEffect 的区别

- useEffect 是异步执行，保证了不会阻塞浏览器进程
- useLayoutEffect 是同步执行，会阻塞浏览器进程 <a name="NkvL0"></a>

### useEffect dep = \[] 与 componentDidMount 的区别

在 render 执行之后，componentDidMount 会执行，如果在这个生命周期中再一次 setState ，会导致再次 render ，返回了新的值，浏览器只会渲染第二次 render 返回的值，这样可以避免闪屏。
但是 useEffect 是在真实的 DOM 渲染之后才会去执行，这会造成两次 render ，有可能会闪屏。
实际上 useLayoutEffect 会更接近 componentDidMount 的表现，它们都同步执行且会阻碍真实的 DOM 渲染的。 <a name="gcqKN"></a>

### React.memo 和 React.useMemo 的区别

- memo 是一个高阶组件，默认情况下会对 props 进行浅比较，如果相等不会重新渲染。多数情况下我们比较的都是引用类型，浅比较就会失效，所以我们可以传入第二个参数手动控制。
- useMemo 返回的是一个缓存值，只有依赖发生变化时才会去重新执行作为第一个参数的函数，需要记住的是，useMemo 是在 render 阶段执行的，所以不要在这个函数内部执行与渲染无关的操作，诸如副作用这类的操作属于 useEffect 的适用范畴。 <a name="wb0db"></a>

### React.useCallback 和 React.useMemo 的区别

- useCallback 可缓存函数，其实就是避免每次重新渲染后都去重新执行一个新的函数。
- useMemo 可缓存值。

有很多时候，我们在 useEffect 中使用某个定义的外部函数，是要添加到 deps 数组中的，如果不用 useCallback 缓存，这个函数在每次重新渲染时都是一个完全新的函数，也就是引用地址发生了变化，这就会导致 useEffect 总会无意义的执行。 <a name="JzucX"></a>

### React.forwardRef

React.forwardRef 会创建一个React组件，这个组件能够将其接受的 ref 属性转发到其组件树下的另一个组件中。这种技术并不常见，但在以下两种场景中特别有用：

- 转发 refs 到 DOM 组件

在高阶组件中转发 refs

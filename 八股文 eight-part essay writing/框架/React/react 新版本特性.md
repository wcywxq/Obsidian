---
title: react 新版本特性
url: https://www.yuque.com/wcywxq/mxunh7/yr6q8p
---

<a name="fXrAl"></a>

## React 16.x 新特性

- Time Slicing 时间切片

（解决 CPU 速度问题）使得在执行任务的期间可以随时暂停，跑去干别的事情，这个特性使得 react 能在性能极其差的机器跑时，仍然保持有良好的性能

- Suspense

解决网络 IO 问题）和 lazy 配合，实现异步加载组件。能暂停当前组件的渲染, 当完成某件事以后再继续渲染，解决从 react 出生到现在都存在的「异步副作用」的问题，而且解决得非常的优雅，使用的是「异步但是同步的写法」，我个人认为，这是最好的解决异步问题的方式

- componentDidCatch

此外，还提供了一个内置函数 componentDidCatch，当有错误发生时, 我们可以友好地展示 fallback 组件；可以捕捉到它的子元素（包括嵌套子元素）抛出的异常；可以复用错误组件 <a name="F0bwU"></a>

### React 16.8

加入了 hooks

- useState
- useEffect
- useContext
- useReducer
- useCallback
- useMemo
- useRef
- useImperativeMethods
- useMutationEffect
- useLayoutEffect <a name="fN6Ta"></a>

### React 16.9

- 重命名 Unsafe 的生命周期方法。新的 UNSAFE\_前缀将有助于在代码 review 和 debug 期间，使这些有问题的字样更突出
- 废弃 javascrip: 形式的 URL。以 javascript: 开头的 URL 非常容易遭受攻击，造成安全漏洞。
- 废弃"Factory"组件。 工厂组件会导致 React 变大且变慢。
- act() 也支持异步函数，并且你可以在调用它时使用 await。
- 使用 \<React.ProfiLer> 进行性能评估。在较大的应用中追踪性能回归可能会很方便 <a name="uHa3f"></a>

### React 16.13.0

- 支持在渲染期间调用 setState，但仅适用于同一组件
- 可检测冲突的样式规则并记录警告
- 废弃 unstable\_createPortal，使用 CreatePortal
- 将组件堆栈添加到其开发警告中，使开发人员能够隔离 bug 并调试其程序，这可以清楚地说明问题所在，并更快地定位和修复错误。 <a name="t6ijF"></a>

## React 18 新特性

1. 新的客户端渲染 API：ReactDOM.createRoot(用于替换 ReactDOM.render)重要
2. 服务端渲染 API 更新，完全支持了服务端的 Suspense 和流式 SSR重要
3. 自动批处理优化(解决了异步回调中无法批处理的问题)重要
4. 不再存在并发模式(Concurrent Mode)，而是并发特性(Concurrent Feature)，侵入性更低重要
5. 新增 Hooks API
   1. useId: 为组件生成唯一的 ID。
   2. useSyncExternalStore：可以在并行渲染时避免 UI 状态和外部数据源割裂，官方推荐所有三方状态库都切换为这个 API，[react-redux 8.0.0-alpha 已切换为此 API](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Freduxjs%2Freact-redux%2Freleases%2Ftag%2Fv8.0.0-alpha.1)。
   3. useInsertionEffect：用于给 CSS 库在全局插入 DOM 节点，比如 <style> 或 SVG <defs>，相比 useLayoutEffect，这个方法的回调过程中拿不到 DOM 节点的 ref。
   4. 用于并发渲染的 useDeferredValue、startTransition、useTransition。
6. Suspense API 更新
   1. React 16 - 17 的 Suspense API 仅限于客户端，仅和 React.lazy 进行搭配。
   2. 而 React 18 起的 Suspense API 将支持内部组件的异步加载状态，同时支持流式 SSR。
   3. Suspense 不再依赖 fallback 属性来定义边界。
7. 删除了 “setState on unmounted component” 警告。
8. 不再支持任何版本的 IE 浏览器。是因为 React 18 中引入的新特性是使用现代浏览器特性构建的，比如微任务，这些特性在 IE 中是无法充分填充的。而 IE 浏览器也将于 2022 年 6 月 15 日停止支持。 <a name="TvKtw"></a>

### 自动批处理

React 18 通过默认执行更多批处理来增加开箱即用的性能改进，无需在应用程序或库代码中手动批处理更新。

批处理是 React 将多个状态更新分组到单个重新渲染中以获得更好的性能。简单来说，批处理（分组）意味着将多个状态更新组合到一个渲染中。每当您使用 setState 更改任何函数内的变量时，React 会收集所有 setState，然后一起执行它们，而不是在每个 setState 上进行渲染。这称为批处理。

```javascript
function App() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  function handleClick() {
    setCount(c => c + 1); // Does not re-render yet
    setFlag(f => !f); // Does not re-render yet
    // React will only re-render once at the end (that's batching!)
  }

  return (
    <div>
      <button onClick={handleClick}>Next</button>
      <h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
    </div>
  );
}
```

这对性能非常有用，因为它避免了不必要的重新渲染。 <a name="nl4zs"></a>

### 服务端渲染

服务端渲染是一种在服务器端将JS数据渲染成HTML的方式，以节省前端的计算量。在大多数情况下，这会导致更快的初始页面加载。

React 分 4 个连续步骤执行服务端渲染：

1. 在服务器上，为每个组件获取数据。
2. 在服务器上，整个应用程序被渲染为 HTML 并发送到客户端。
3. 在客户端，获取整个应用程序的 JavaScript 代码。
4. 在客户端，JavaScript 将 React 连接到服务器生成的 HTML，即Hydration。在普通版本中（直到 React 17），SSR 必须先加载整个页面，然后才能开始对页面进行保湿。

这在 React18 中发生了变化，现在我们可以使用将 React 组件分解成更小的块 . <a name="adgvC"></a>

### 流式 HTML

通过将组件包装其中：

```jsx
<Suspense fallback={<Spinner />}>{children}</Suspense> 
```

我们告诉 React 它不需要等待为页面的其余部分流式传输 HTML，这时React 将发送占位符（一个spinner）。
当数据在服务器上准备好时，React 会将额外的 HTML 发送到同一个流中，以及一个最小的内联脚本标签，以将该 HTML 放在“正确的位置”。

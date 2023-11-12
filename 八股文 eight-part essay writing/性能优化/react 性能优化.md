---
title: react 性能优化
url: https://www.yuque.com/wcywxq/mxunh7/qf0cik
---

<a name="BANVM"></a>

## 开发过程中

1. 函数式组件使用 React.Memo 来缓存组件避免组件重渲染
2. 使用 useMemo 缓存大量计算
3. 类组件使用 React.PureComponent，shouldComponentUpdate避免组件重渲染
4. 避免使用内联对象
5. 避免使用匿名函数
6. 延迟加载不是立即需要的组件(React.lazy + React.Suspense)
7. 调整 css 而不是强制组件加载和卸载
8. 使用 React.Fragment 避免添加额外的 DOM
9. 不在 render 中处理数据
10. 使用 web worker 做密集型的任务处理
11. 使用唯一的键值迭代 <a name="xa67Q"></a>

## 上线后首屏及运行状态

- 首屏优化一般涉及到几个指标 FP、FCP、FMP；要有一个良好的体验是尽可能的把 FCP 提前，需要做一些工程化的处理，去优化资源的加载
  - FP: 页面在导航后首次呈现出不同于导航前内容的时间点;
  - FCP: 首次绘制任何文本，图像，非空白 canvas 或 SVG 的时间点;
  - FMP: 是由 Google 工程师引入的一种现代性能指标，它告诉我们页面何时 **有用**。其本质上是通过一种算法来猜测某个时间点可能是 FMP
- 方式及分包策略，资源的减少是最有效的加快首屏打开的方式
- 对于 CSR 的应用，FCP 的过程一般是首先加载 js 与 css 资源，js 在本地执行完成，然后加载数据回来，做内容初始化渲染，这中间就有几次的网络反复请求的过程；所以 CSR 可以考虑使用骨架屏及预渲染（部分结构预渲染）、suspence 与 lazy 做懒加载动态组件的方式
- 当然还有另外一种方式就是 SSR 的方式，SSR 对于首屏的优化有一定的优势，但是这种瓶颈一般在 Node 服务端的处理，建议使用 stream 流的方式来处理，对于体验与 node 端的内存管理等，都有优势；
- 不管对于 CSR 或者 SSR，都建议配合使用 Service worker，来控制资源的调配及骨架屏秒开的体验
- react 项目上线之后，首先需要保障的是可用性，所以可以通过 React.Profiler 分析组件的渲染次数及耗时的一些任务，但是 Profile 记录的是 commit 阶段的数据，所以对于 react 的调和阶段就需要结合 performance API 一起分析
- 由于 React 是父级 props 改变之后，所有与 props 不相关子组件在没有添加条件控制的情况之下，也会触发 render 渲染，这是没有必要的，可以结合 React 的 PureComponent 以及 React.memo 等做浅比较处理，这中间有涉及到不可变数据的处理，当然也可以结合使用 ShouldComponentUpdate 做深比较处理
- 所有的运行状态优化，都是减少不必要的 render，React.useMemo 与 React.useCallback 也是可以做很多优化的地方
- 在很多应用中，都会涉及到使用 redux 以及使用 context，这两个都可能造成许多不必要的 render，所以在使用的时候，也需要谨慎的处理一些数据
- 最后就是保证整个应用的可用性，为组件创建错误边界，可以使用 componentDidCatch 来处理

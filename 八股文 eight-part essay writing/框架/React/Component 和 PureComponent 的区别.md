---
title: Component 和 PureComponent 的区别
url: https://www.yuque.com/wcywxq/mxunh7/ypfivt
---

PureComponent 表示一个纯组件，可以用来优化 React 程序，减少 render 函数执行的次数，从而提高组件的性能。

在 React 中，当 prop 或者 state 发生变化时，可以通过在 shouldComponentUpdate 生命周期函数中执行 return false 来阻止页面的更新，从而减少不必要的 render 执行。React.PureComponent 会自动执行 shouldComponentUpdate

不过，pureComponent 中的 shouldComponentUpdate() 进行的是**浅比较**，也就是说如果是引用数据类型的数据，只会比较不是同一个地址，而不会比较这个地址里面的数据是否一直。浅比较会忽略属性和或状态突变的情况，其实也就是数据引用指针没有变化，而数据发生变化的时候 render 是不会执行的。如果需要重新渲染，那么久需要重新开辟空间引用数据。PureComponent 一般会用在一些纯展示组件上。

<a name="OQQnQ"></a>

### 使用 pureComponent 的好处

当组件更新时，如果组件的 props 或者 state 都没有改变，render 函数就不会触发。省去虚拟 DOM 的生成和对比过程，达到提升性能的目的。这是因为 react 自动做了一层浅比较。

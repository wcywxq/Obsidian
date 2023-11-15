---
title: vue 和 react 的对比
url: https://www.yuque.com/wcywxq/mxunh7/nm61qs
---

相似之处：

- 都将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库
- 都有自己的构建工具，能让你得到一个根据最佳实践设置的项目模板。
- 都使用了 `Virtual DOM`（虚拟 `DOM`）提高重绘性能
- 都有 `props` 的概念，允许组件间的数据传递
- 都鼓励组件化应用，将应用分拆成一个个功能明确的模块，提高复用性

不同之处：

- 数据流

`Vue` 默认支持数据双向绑定，而 `React` 一直提倡单向数据流

- 虚拟DOM

`Vue2.x` 开始引入`"Virtual DOM"`，消除了和 `React` 在这方面的差异，但是在具体的细节还是有各自的特点。

- `Vue` 宣称可以更快地计算出 `Virtual DOM` 的差异，这是由于它在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。
- 对于 `React` 而言，每当应用的状态被改变时，全部子组件都会重新渲染。当然，这可以通过 `PureComponent`/`shouldComponentUpdate` 这个生命周期方法来进行控制，但 `Vue` 将此视为默认的优化。
- 组件化

`React` 与 `Vue` 最大的不同是模板的编写。

- `Vue` 鼓励写近似常规 `HTML` 的模板。写起来很接近标准 `HTML`元素，只是多了一些属性。
- `React` 推荐你所有的模板通用 `JavaScript` 的语法扩展——`JSX` 书写。

具体来讲：`React` 中 `render` 函数是支持闭包特性的，所以我们 `import` 的组件在 `render` 中可以直接调用。但是在 `Vue` 中，由于模板中使用的数据都必须挂在 `this` 上进行一次中转，所以 `import` 完组件之后，还需要在 `components` 中再声明下。

- 监听数据变化的实现原理不同
  - `Vue` 通过 `getter`/`setter` 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
  - `React` 默认是**通过比较引用**的方式进行的，如果不优化（`PureComponent`/`shouldComponentUpdate`）可能导致大量不必要的 `vDOM` 的重新渲染。这是因为 `Vue` 使用的是**可变数据**，而 `React` 更**强调数据的不可变**
- 高阶组件

`react` 可以通过高阶组件（`Higher Order Components-- HOC`）来扩展，而 `vue` 需要通过 `mixins` 来扩展。
原因高阶组件就是高阶函数，而 `React` 的组件本身就是纯粹的函数，所以高阶函数对 `React` 来说易如反掌。相反 `Vue.js` 使用 `HTML` 模板创建视图组件，这时模板无法有效的编译，因此 `Vue` 不采用 `HOC` 来实现。

- 构建工具

两者都有自己的构建工具

- `React ==> Create React APP`
- `Vue ==> vue-cli`
- 跨平台
  - `React ==> React Native`
  - `Vue ==> Weex`

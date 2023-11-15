---
title: React 常见面试题
url: https://www.yuque.com/wcywxq/xxble8/qf8r3p
---

<a name="xJ3qc"></a>

## React 中 keys 的作用是什么

- 在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。
- 在 react diff 算法中 react 会借助元素的 key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的重渲染。
- 此外，react 还要借助 key 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 key 的重要性。

<a name="pC5bd"></a>

## 当调用 setState 时，发生了什么

将传递给 setState 的对象合并到组件的当前状态，这将启动一个和解的过程，构建一个新的 react 元素树，与上一个元素树进行对比(diff)，从而进行最小化的重渲染

<a name="nLOYx"></a>

## 为什么 setState 的参数是一个 callback 而不是一个对

因为 this.props 和 this.state 的更新可能是异步的，不能依赖他们的值去计算下一个 state

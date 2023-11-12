---
title: computed 相关问题
url: https://www.yuque.com/wcywxq/mxunh7/zq4s02
---

<a name="H7SKA"></a>

## computed 实现原理

computed watcher, 计算属性的监听器
computed watcher 持有一个 dep 实例，通过 drity 属性标记计算属性是否需要重新求值

当 computed 的依赖值改变后，就会通知订阅的 watcher 进行更新，对于 computed watcher 会将 drity 属性设置为 true，并且进行计算属性方法的调用 <a name="wNthY"></a>

## computed 所谓的缓存是什么

计算属性是基于它的响应式依赖进行缓存的，只有依赖发生改变的时候才会重新求值 <a name="Z32HB"></a>

## 何时会用到 computed 缓存

比如计算属性方法内部操作非常耗时，遍历一个极大的数组，计算一次可能要耗时 1s

<a name="bMLvz"></a>

## computed 和 watch 的差异

- computed: 计算一个新属性，挂载到实例上(当依赖发生变化后才会重新计算)。
- watch: 监听已经被挂载到实例上的数据，数据发生变化了就会调用。 <a name="WtGk1"></a>





---
title: Link 和 a 标签的区别
url: https://www.yuque.com/wcywxq/mxunh7/wm4ok4
---

对比 a 标签，Link 标签避免了不必要的重渲染

react-router 接管了其默认的链接跳转行为，区别于传统的页面跳转，Link 的"跳转"行为只会触发相匹配的对应页面内容更新，而不会刷新整个页面。

Link 跳转做了三件事：

1. 有 onclick 的时候会直接执行
2. click 的时候阻止了 a 标签的默认事件
3. 根据跳转的 href，用 history 跳转，此时只是链接发生了变化，并不会刷新页面

a 标签跳转属于普通的超链接跳转，会跳转到 href 指向的另一个页面(非锚点跳转的情况)

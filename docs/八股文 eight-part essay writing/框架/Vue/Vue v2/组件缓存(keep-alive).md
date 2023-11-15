---
title: 组件缓存(keep-alive)
url: https://www.yuque.com/wcywxq/mxunh7/wqgptk
---

keep-alive 可以实现组件缓存，当组件切换时不会对当前组件进行卸载。

常用的两个属性 include/exclude，允许组件有条件的进行缓存。

两个生命周期 activated(命中缓存时调用)/deactivated(切换时调用)，用来得知当前组件是否处于活跃状态。同时，beforeDestroy 和 destroyed 就不会再被触发了，因为组件不会被真正销毁。

keep-alive 的中还运用了 LRU(Least Recently Used)算法。 <a name="BEZtk"></a>

## LRU(Least Recently Used)缓存策略

LRU 缓存策略：
从内存中找出最久未使用的数据并置换新的数据。 LRU(Least Recently Used)算法根据数据的历史访问记录来进行淘汰数据，其核心思想是 "如果数据最近被访问过，那么将来被访问的几率也更高"。 最常见的实现是使用一个链表保存缓存数据，详细算法实现如下：

- 新数据插入到链表头部
- 每当缓存命中（即缓存数据被访问），则将数据移到链表头部
- 链表满的时候，将链表尾部的数据丢弃。

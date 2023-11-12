---
title: 组件中 data 为什么是一个函数
url: https://www.yuque.com/wcywxq/mxunh7/kwsg4m
---

复用多次的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数。如果 data 是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间 data 不冲突，data 必须是一个函数。

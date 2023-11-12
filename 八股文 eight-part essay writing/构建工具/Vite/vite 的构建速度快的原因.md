---
title: vite 的构建速度快的原因
url: https://www.yuque.com/wcywxq/mxunh7/on0xtf
---

- 部分文件设置了强缓存，每次都会在本地读取
- 采用了 esbuild
  - esbuild 使用 go 语言编写，速度更快，打包会快 10 ～ 100 倍
  - 大量使用了并发算法

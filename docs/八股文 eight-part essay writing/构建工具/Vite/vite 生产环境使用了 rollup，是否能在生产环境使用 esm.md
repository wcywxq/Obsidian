---
title: vite 生产环境使用了 rollup，是否能在生产环境使用 esm
url: https://www.yuque.com/wcywxq/mxunh7/wfsv84
---

- 如果项目不需要兼容 IE11 等低版本的浏览器，自然是可以使用的。
- 但更通用的方案可能还是类似 ployfill.io 的原理实现， 提前构建好 bundle.js 与 es module 两个版本的代码，根据浏览器的实际兼容性去动态选择导入哪个模块。

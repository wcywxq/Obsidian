---
title: rollup 优缺点
url: https://www.yuque.com/wcywxq/mxunh7/gugo43
---

- 优点
  - 输出结果更加扁平，执行效率更高
  - 配置简单，打包速度快
  - 自动移除未引用的代码（内置 `tree shaking`）
  - 打包结果依然完全可读
- 缺点
  - 浏览器环境中，代码拆分功能必须使用 `Require.js` 这样的 `AMD` 库
  - 加载非 `ESM` 的第三方模块比较复杂
  - 因为模块最终都被打包到全局中，所以无法实现 `HMR`

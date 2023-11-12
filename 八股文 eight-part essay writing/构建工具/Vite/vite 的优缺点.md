---
title: vite 的优缺点
url: https://www.yuque.com/wcywxq/mxunh7/xqi8st
---

:::success
**优点：**

- 快速的冷启动: 采用 No Bundle 和 esbuild 预构建，速度远快于 Webpack
- 高效的热更新：基于 ESM 实现，同时利用 HTTP 头来加速整个页面的重新加载，增加缓存策略
- 真正的按需加载: 基于浏览器 ESM 的支持，实现真正的按需加载
  :::

:::danger
**缺点：**

- 生产环境由于 esbuild 对 css 和代码分割不友好使用 Rollup 进行打包
- 目前 Vite 还是使用的 es module 模块不能直接使用生产环境（兼容性问题）。默认情况下，无论是 dev 还是 build 都会直接打出 ESM 版本的代码包，这就要求客户浏览器需要有一个比较新的版本，这放在现在的国情下还是有点难度的。不过 Vite 同时提供了一些弥补的方法，使用 build.polyfillDynamicImport 配置项配合 @vitejs/plugin-legacy 打包出一个看起来兼容性比较好的版本。
- 生产环境使用 rollup 打包会造成开发环境与生产环境的不一致。
- 很多 第三方 sdk 没有产出 ems 格式的的代码，这个需要自己去做一些兼容。
  :::

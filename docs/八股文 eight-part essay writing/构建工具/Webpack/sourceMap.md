---
title: sourceMap
url: https://www.yuque.com/wcywxq/mxunh7/dtf17x
---

`sourceMap` 是将**编译**、**打包**、**压缩**后的代码映射回源代码的过程。打包压缩后的代码不具备良好的可读性，想要调试源码就需要 `sourceMap`。`map` 文件只要不打开开发者工具，浏览器是不会加载的

线上环境一般有三种处理方案：

- `hidden-source-map`：借助第三方错误监控平台 `Sentry` 使用
- `nosources-source-map`：只会现实具体行数以及查看源代码的错误栈。安全性比 `sourceMap` 高
- `sourceMap`：通过 `nginx` 配置将 `.map` 文件只对白名单开放(公司内网)

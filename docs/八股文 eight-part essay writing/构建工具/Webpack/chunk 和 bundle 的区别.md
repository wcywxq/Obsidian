---
title: chunk 和 bundle 的区别
url: https://www.yuque.com/wcywxq/mxunh7/hg8h4r
---

:::info
个人理解：
`chunk` 是过程，`bundle` 是结果，`chunk` 在构建完成时呈现为 `bundle`
::: <a name="vMn5G"></a>

## chunk

`chunk` 是 `webpack` 打包过程中 `modules` 的集合，是**打包过程中**的概念。
`webpack` 的打包是从一个入口模块开始，入口模块引用其他模块，其他模块引用其他模块，这样一直递归下去。
`webpack` 通过引用关系逐个打包模块，这些 `module` 就形成了一个 `chunk`，如果有多个入口模块，可能会产生多个打包路径，每条路径都会形成 `chunk` <a name="AxhEX"></a>

## bundle

是最终输出的一个或多个打包好的文件 <a name="rykJw"></a>

## chunk 和 bundle 的区别(关系)是什么

大多数情况下，一个 `chunk` 会生产一个 `bundle`，但也有例外。如果加了 `sourceMap`，一个 `entry`，一个 `chunk` 对应两个 `bundle` <a name="Jvjn0"></a>

## split chunk

做代码分割，可以通过配置增加

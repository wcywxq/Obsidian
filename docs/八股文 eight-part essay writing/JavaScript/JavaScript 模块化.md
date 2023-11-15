---
title: JavaScript 模块化
url: https://www.yuque.com/wcywxq/mxunh7/cakiwz
---

<a name="HMvRd"></a>

## esmodule 和 commonjs 的区别

- commonjs 默认采用的是非严格模式
- esmodule 默认采用严格模式
- commonjs 模块输出的是一个值的拷贝，esmodule 输出的是值的引用 <a name="qlsks"></a>

## import 和 require 的区别

- 模块的加载时机不同(require: 运行时加载，import: 会提升到整个模块头部，在编译时加载)
- require：模块就是对象，输入时必须查找对象属性；import：esmodule 模块不是对象

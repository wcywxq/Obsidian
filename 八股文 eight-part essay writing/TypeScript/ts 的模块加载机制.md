---
title: ts 的模块加载机制
url: https://www.yuque.com/wcywxq/mxunh7/ug5vit
---

假设有一个导入语句 `import { a } from "moduleA"`

1. 首先，编译器会尝试定位需要导入的模块文件，通过绝对或者相对的路径查找方式
2. 如果上面的解析失败了，没有查找到对应的模块，编译器会尝试定位一个外部模块声明（`.d.ts`）
3. 最后，如果编译器还是不能解析这个模块，则会抛出一个错误 `error TS2307: Cannot find module 'moduleA'.`

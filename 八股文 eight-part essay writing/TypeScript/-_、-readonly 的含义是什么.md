---
title: -?、-readonly 的含义是什么
url: https://www.yuque.com/wcywxq/mxunh7/vwkgu7
---

用于删除修饰符，`-?` 移除可选属性中的 `?`、`-readonly` 移除只读属性

```typescript
type A = {
  a: string
  b: number
}

type B = {
  [K in keyof A]?: A[K]
}

type C = {
  [K in keyof B]-?: B[K]
}

type D = {
  readonly [K in keyof A]: A[K]
}

type E = {
  -readonly [K in keyof A]: A[K]
}
```

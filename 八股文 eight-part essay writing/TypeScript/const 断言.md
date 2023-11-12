---
title: const 断言
url: https://www.yuque.com/wcywxq/mxunh7/xgt0bn
---

`const`断言，`typescript`会为变量添加一个自身的字面量类型

1. 对象字面量的属性，获得`readonly`的属性，成为只读属性
2. 数组字面量成为`readonly``tunple`只读元组
3. 字面量类型不能被扩展(比如从`hello`类型到`string`类型)

```typescript
// type '"Hello"'
let x = "hello" as const
// type 'readonly [10, 20]'
let y = [10, 20] as const
// type '{ readonly text: "hello" }'
let z = { test: "hello" } as const
```

---
title: ts 中 ?.、??、!.、_、** 等符号的含义
url: https://www.yuque.com/wcywxq/mxunh7/lyezty
---

- `?.`：可选链
- `??`：`??` 类似与短路或，`??` 避免了一些意外情况 `0`，`NaN` 以及 `""`，`false` 被视为 `false` 值。只有 `undefind`，`null` 被视为 `false` 值。
- `!.`：在变量名后添加 `!`，可以断言排除 `undefined` 和 `null` 类型
- `_ `：声明该函数将被传递一个参数，但您并不关心它
- `**`：求幂
- `!:`：待会分配这个变量，`ts` 不要担心

```typescript
// ??
let x = foo ?? bar()
// 等价于
let x = foo !== null && foo !== undefined ? foo : bar()
// !.
let a: string | null | undefined
a.length // error
a!.length // ok
```

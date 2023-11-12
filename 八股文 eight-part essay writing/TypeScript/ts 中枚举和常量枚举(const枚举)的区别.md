---
title: ts 中枚举和常量枚举(const枚举)的区别
url: https://www.yuque.com/wcywxq/mxunh7/oznghr
---

1. 枚举会被编译时会编译成一个对象，可以被当作对象使用
2. `const` 枚举会在 `ts` 编译期间被删除，避免额外的性能开销

```typescript
// 普通枚举
enum Witcher {
  Ciri = 'Queen',
  Geralt = 'Geralt of Rivia'
}

type Obj = {
  [key: string]: string
}

function getGeraltMessage(arg: Obj): string {
  return arg.Geralt
}

getGeraltMessage(Witcher) // Geralt of Rivia
```

```typescript
// const 枚举
const enum Witcher {
  Ciri = 'Queue',
  Geralt = 'Geralt of Rivia'
}

const witchers: Witcher[] = [Witcher.Ciri, Witcher.Geralt]
// 编译后
// const witchers = ['Queue', 'Geralt of Rivia']

```

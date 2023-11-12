---
title: ts 是基于结构类型兼容的
url: https://www.yuque.com/wcywxq/mxunh7/hct7os
---

`typeScript`的类型兼容是基于结构的，不是基于名义的。下面的代码在 `ts`中是完全可行的，同样的代码在 `jav`等基于名义的语言则会抛错。

```typescript
interface Named {
  name: string
}

class Person {
  name: string
}

let p: Named
// ok
p = new Person()
```

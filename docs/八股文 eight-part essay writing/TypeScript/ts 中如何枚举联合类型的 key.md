---
title: ts 中如何枚举联合类型的 key
url: https://www.yuque.com/wcywxq/mxunh7/ly5v3x
---

```typescript
type Name = { name: string }
type Age = { age: number }
type Union = Name | Age

type UnionKey<P> = P extends infer P ? keyof P : never
type T = UnionKey<Union>
```

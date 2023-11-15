---
title: ts 中 enum 类型转 type
url: https://www.yuque.com/wcywxq/mxunh7/gehkyv
---

```typescript
enum test {
  A1,
  A2,
  A3
}

type Test = keyof typeof test
```

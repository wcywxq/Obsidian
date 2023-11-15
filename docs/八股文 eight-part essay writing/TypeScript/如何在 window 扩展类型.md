---
title: 如何在 window 扩展类型
url: https://www.yuque.com/wcywxq/mxunh7/lnqu18
---

```typescript
declare global {
  interface Window {
    myCustomFn: () => void
  }
}
```

---
title: Promise 每隔 1s 输出 1、2、3
url: https://www.yuque.com/wcywxq/pgkpvh/yg55el
---

```javascript
const arr = [1, 2, 3]
arr.reduce((prev, cur) => {
  return prev.then(() => {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve(console.log(cur))
      }, 1000)
    })
  })
}, Promise.resolve())
```

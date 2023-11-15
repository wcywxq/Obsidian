---
title: sleep 函数实现
url: https://www.yuque.com/wcywxq/sd8kbc/ngwrak
---

```javascript
const sleep = (func, delay) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(func())
    }, delay)
  })
}

const consoleStr = (str) => {
  return () => {
    console.log(str)
    return str
  }
}

const doFns = async () => {
  const name = await sleep(consoleStr('test content'), 1000)
  const sex = await sleep(consoleStr('boy'), 1000)
  const age = await sleep(consoleStr(100), 1000)

  console.log(name, sex, age)
}

doFns()
// test content 1s later
// boy 2s later
// 100 3s later
// test content boy 100
```

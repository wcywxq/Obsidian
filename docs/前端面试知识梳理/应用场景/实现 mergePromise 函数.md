---
title: 实现 mergePromise 函数
url: https://www.yuque.com/wcywxq/pgkpvh/apqplp
---

实现 mergePromise 函数，把传进去的数组按照顺序先后执行，并且把返回的数据先后放到数组 data 中

```javascript
const time = timer => {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve()
    }, timer)
  })
}

const ajax1 = () => time(2000).then(() => {
  console.log(1)
  return 1
})

const ajax2 = () => time(1000).then(() => {
  console.log(2)
  return 2
})

const ajax3 = () => time(1000).then(() => {
  console.log(3)
  return 3
})

mergePromise([ajax1, ajax2, ajax3]).then(data => {
  console.log("done")
  console.log(data) // data 为 [1, 2, 3]
})

// 需要分别输出
// 1
// 2
// 3
// done
// [1, 2, 3]
```

- 实现

```javascript
function mergePromise(ajaxArray) {
  // 存放每个 ajax 的结果
  const data = []
  let promise = Promise.resolve()
  ajaxArray.forEach(ajax => {
    // 第一次 then 为了用来调用 ajax
    // 第二次 then 为了获取 ajax 结果
    promise = promise.then(ajax).then(res => {
      data.push(res)
      return data // 返回每次的结果
    })
  })
  return promise
}
```

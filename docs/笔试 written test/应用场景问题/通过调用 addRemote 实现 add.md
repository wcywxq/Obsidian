---
title: 通过调用 addRemote 实现 add
url: https://www.yuque.com/wcywxq/sd8kbc/edff3s
---

<a name="W4Vn6"></a>

## 描述

通过调用下面的`addRemote`，实现`add`方法。`addRemote`仅能计算两个数字之和，`add`可以传入任意多个数字，返回的是这些数字之和

```javascript
const addRemote = async (a, b) => new Promise(resolve => {
  setTimeout(() => resolve(a + b), 1000)
})

async function add(...inputs) {}

// 请用示例验证运行结果
add(1, 2).then(result => {
  console.log(result) // 3
})

add(3, 5, 2).then(result => {
  console.log(result) // 10
})
```

<a name="ng1HU"></a>

## 实现

<a name="VjBy9"></a>

### 利用 reduce

```javascript
async function add(...inputs) {
  return inputs.reduce((prev, cur) => prev.then(val => addRemote(val, cur)), Promise.resolve(0))
}
```

<a name="nrIjD"></a>

### 递归

```javascript
async function add(...inputs) {
  let res = 0
  if (inputs.length === 0) return res
  if (inputs.length === 1) return inputs[0]
  const a = inputs.pop()
  const b = inputs.pop()
  inputs.push(await addRemote(a, b))
  return add(...inputs)
}
```

<a name="QQhno"></a>

### Promise.all

```javascript
async function add(...inputs) {
  if (inputs.length === 0) return res
  if (inputs.length === 1) return inputs[0]
  const promiseList = []
  for (let i = 0; i * 2 < inputs.length - 1; i++) {
    const promise = addRemote(inputs[i * 2], inputs[i * 2 + 1])
    promiseList.push(promise)
  }
  if (inputs.length % 2) {
    promiseList.push(Promise.resolve[inputs[inputs.length - 1]])
  }
  return Promise.all(promiseList).then(res => add(...res))
}
```

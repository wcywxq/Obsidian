---
title: Promise 顺序串行输出
url: https://www.yuque.com/wcywxq/sd8kbc/wtr4cf
---

```javascript
let arr = [() => {
    return new Promise(res => {
        console.log("run1", Date.now());
        res()
    })
}, () => {
    return new Promise(res => {
        console.log("run2", Date.now());
        res()
    })
}, () => {
    return new Promise(res => {
        console.log("run3", Date.now());
        res()
    })
}];
```

<a name="fMTro"></a>

## async - await

```javascript
async function serialOutput(arr) {
  for (const val of arr) {
    await val
  }
}
```

<a name="fMru2"></a>

## Promise.resolve

```javascript
function p(arr) {
    let res = Promise.resolve();
    arr.forEach(val => {
        res = res.then(() => val());
    });
}
```

<a name="sTqmJ"></a>

## reduce

```javascript
function p(arr) {
    arr.reduce((pre, cur) => {
        return pre.then(() => cur())
    }, Promise.resolve());
}
```

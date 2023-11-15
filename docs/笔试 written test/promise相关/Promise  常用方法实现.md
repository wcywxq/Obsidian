---
title: Promise  常用方法实现
url: https://www.yuque.com/wcywxq/sd8kbc/vd2h4l
---

<a name="OgAA4"></a>

## Promise.all

`Promise.all` 方法用于将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

- 如果每个实例都成功，会将所有实例的返回值组成一个数组
- 如果其中一个实例运行失败，则返回最先失败的实例结果（在创建实例的时候，`promise.all` 内部的实例就已经执行了，无论哪一个失败）

```javascript
Promise.prototype.all = promises => {
	return new Promise((resolve, reject) => {
  	// 计数器
    let count = 0
    // 存放结果
    let result = []
    let len = promises.length
    if (len === 0) return resolve([])
    promises.forEach((item, index) => {
    	// 注意有的数组项有可能不是 Promise，需要手动转化一下
      Promise.resolve(item).then(res => {
      	count += 1
        // 收集每个 Promise 的返回值
       	result[i] = res
        // 当所有的 Promise 都成功了，那么将返回 Promise 结果
        if(count === len) {
        	resolve(result)
        }
      }).catch(err => reject(err))
    })
  })
}
```

<a name="N2V7r"></a>

## Promise.allSettled

有时候，我们希望等到一组异步操作都结束了，不管一个操作是成功还是失败，再进行下一步操作。而 `Promise.all` (其只要是一个失败了，结果即进入失败状态)不太合适，所以有了 `Promise.allSettled`

只有等到参数数组的所有 `Promise` 对象都发生状态变更(不管是 `fulfilled` 还是 `rejected`)，返回的 `Promise` 对象才会发生状态变更，一旦发生状态变更，状态总是 `fulfilled`，不会变成 `rejected`

```javascript
Promise.prototype.allSettled = promises => {
	return new Promise((resolve, reject) => {
  	let count = 0
    let result = []
    let len = promises.length
    // 数组是空的话，直接返回空数组
    if (len === 0) return resolve([])
    promises.forEach((item, index) => {
    	Promise.resolve(item).then(res => {
      	count += 1
        // 成功属性设置
        result[i] = {
        	status: 'fulfilled',
          value: res
        }
        if (count === len) {
        	resolve(result)
        }
      }).catch(err => {
      	count += 1
        // 失败属性设置
        result[i] = {
        	status: 'rejected',
          reason: err
        }
        if (count === len) {
        	resolve(result)
        }
      })
    })
  })
}
```

<a name="m5Trz"></a>

## Promise.race

传入的所有实例哪一个实例的状态率先改变，那么就会返回哪个实例的返回值。无论成功还是失败

```javascript
Promise.prototype.race = promises => {
	return new Promise((resolve, reject) => {
  	promises.forEach(item => {
  		// 此处需要做二次包装，防止非 Promise 对象
      // 同时进行状态监听
      Promise.resolve(item)
        .then(value => resolve(value))
        .catch(err => reject(err))
  	})
  })
}
```

<a name="jdISu"></a>

## Promise.any

只有全部实例都失败后才会返回失败，如果有一个实例的状态返回成功，则返回第一个成功的实例

```javascript
Promise.prototype.any = promises => {
  return new Promise((resolve, reject) => {
    let count = 0
  	let len = promiseList.length
    if (len === 0) return
    promises.forEach(item => {
    	Promise.resolve(item)
      	.then(value => resolve(value))
      	.catch(err => {
      		count+=1
        	if (count === len) {
          	return reject(new Error('All promises where rejected'))
          }
      	})
    })
  })
}
```

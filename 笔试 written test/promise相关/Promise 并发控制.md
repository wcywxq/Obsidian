---
title: Promise 并发控制
url: https://www.yuque.com/wcywxq/sd8kbc/gd3193
---

<a name="OtLfr"></a>

##

<a name="ULPRh"></a>

## 并发控制函数

```javascript
// 模拟异步请求
const request = url => new Promise(resolve => {
	setTimeout(() => {
  	resolve(`任务 ${url.padEnd(10, ' ')} 完成`)
  }, 1000)
})

// 并发控制函数
async function asyncPool(tasks = [], max = 3, fn = () => Promise.resolve()) {
	// 待执行任务数组
  // 正在执行的任务数组
  let pool = []
  for (const item of tasks) {
    // 生成异步任务	
    const task = fn(item)
    // 添加到正在执行的任务数组
    pool.push(task)
    task.then(data => {
    	// 当任务执行完毕，将其从正在执行任务数组中移除
      console.log(`${data}; 当前并发数: ${pool.length}`)
      pool.splice(pool.indexOf(task), 1)
    })
    
    // 当并发池满了，就先去执行并发池中的任务，有任务执行完成后，再继续循环
    if (pool.length === max) {
    	await Promise.race(pool)
    }
  }
}

const tasks = new Array(10).fill('').map((task, i) => `url - ${i + 1}`)
asyncPool(tasks, 3, request)
```

<a name="Hlqg7"></a>

##

<a name="VJDtr"></a>

## async-pool 源码实现

核心思路：

1. 先初始化 `limit` 个 `promise` 实例，将他们放到 `executing` 数组中
2. 使用 `Promise.race` 等待这 `limit` 个 `promise` 实例的执行结果
3. 一旦某一个 `promise` 的状态发生变更，就将其从 `executing` 中删除，然后再执行循环生成新的 `promise`，放入 `executing` 中
4. 重复 2、3 两个步骤，直到所有的 `promise` 都被执行完
5. 最后使用 `Promise.all` 返回所有 `promise` 实例的执行结果 <a name="Q0xPD"></a>

### es6 实现

```javascript
function asyncPool(poolLimit, array, iteratorFn) {
	let i = 0
  const ret = []
  const executing = []
  const enqueue = function() {
  	if (i === array.length) return Promise.resolve()
    const item = array[i++]
    const p = Promise.resolve().then(() => iteratorFn(item, array))
    ret.push(p)
    let r = Promise.resolve()
    if (poolLimit <= array.length) {
    	const e = p.then(() => executing.splice(executing.indexOf(e), 1))
      executing.push(e)
      if (executing.length >= poolLimit) {
      	r = Promise.race(executing)
      }
    }
    return r.then(() => enqueue())
  }
  return enqueue().then(() => Promise.all(ret))
}
```

<a name="MhPML"></a>

### es7 实现

```javascript
async function asyncPool(poolLimit, array, iteratorFn) {
	const ret = []
  const executing = []
  for (const item of array) {
  	const p = Promise.resolve().then(() => iteratorFn(item, array))
    ret.push(p)
    if (poolLimit <= array.length) {
      // then 回调中，当这个 promise 状态变为 fulfilled 后，将其从正在执行的 promise 列表 executing 中删除
    	const e = p.then(() => executing.splice(executing.indexOf(e), 1))
      executing.push(e)
      if (executing.length >= poolLimit) {
        // 一旦正在执行的 promise 列表数量等于限制数，就使用 Promise.race 等待某一个 promise 状态发生变更
        // 状态变更后，就会执行上面 then 的回调，将该 promise 从 executing 中删除
        // 然后再进入到下一次 for 循环，生成新的 promise 进行补充
      	await Promise.race(executing)
      }
    }
  }
  return Promise.all(ret)
}
```

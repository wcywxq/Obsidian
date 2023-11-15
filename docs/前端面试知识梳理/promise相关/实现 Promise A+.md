---
title: 实现 Promise A+
url: https://www.yuque.com/wcywxq/pgkpvh/fl6cgx
---

```javascript
// A+
const PENDING = "PENDING"
const FULFILLED = "fulfilled"
const REJECTED = "rejected"

class _Promise {
  constructor(executor) {
    this.status = PENDING
    this.value = undefined
    this.reason = undefined
    
    this.onResolvedCallbackList = []
    this.onRejectedCallbackList = []
    
    const resolve = value => {
      if (this.status === PENDING) {
        this.status = FULFILLED
        this.value = value
        this.onResolvedCallbackList.forEach(fn => fn())
      }
    }
    const reject = reason => {
      if (this.status === PENDING) {
        this.status = REJECTED
        this.reason = reason
        this.onRejectedCallbackList.forEach(fn => fn())
      }
    }
    
    try {
      executor(resolve, reject)
    } catch (err) {
      reject(err)
    }
  }
  
  resolvePromise(promise, next, resolve, reject) {
    if (next === promise) {
      return reject(new TypeError("xxx"))
    }
    let called
    if ((typeof next === "object" && next !== null) || typeof next === "function") {
      try {
        if (typeof next.then === "function") {
          const resolveArg = resolveNext => {
            this.resolvePromise(promise, resolveNext, resolve, reject)
          }
          const rejectArg = rejectNext => {
            if (called) return
            called = true
            this.resolvePromise(promise, rejectNext, resolve, reject)
          }
          next.then.call(next, resolveArg, rejectArg)
        } else {
          resolve(next)
        }
      } catch (err) {
        if (called) return
        called = true
        reject(err)
      }
    } else {
      reject(next)
    }
  }
  
  then(onFulfilled, onRejected) {
    onFulfilled =
      typeof onFulfilled === "function"
      ? onFulfilled
    : function (val) {
      return val
    }
    onRejected =
      typeof onRejected === "function"
      ? onRejected
    : function (val) {
      return val
    }
    
    const promise = new Promise((resolve, reject) => {
      const resolveCallback = () => {
        try {
          let next = onFulfilled(this.value)
          this.resolvePromise(promise, next, resolve, reject)
        } catch (err) {
          reject(err)
        }
      }
      const rejectCallback = () => {
        try {
          let next = onRejected(this.reason)
          this.resolvePromise(promise, next, resolve, reject)
        } catch (err) {
          reject(err)
        }
      }
      
      if (this.status === FULFILLED) {
        setTimeout(resolveCallback)
      }
      if (this.status === REJECTED) {
        setTimeout(rejectCallback)
      }
      if (this.status === PENDING) {
        this.onResolvedCallbackList.push(resolveCallback)
        this.onRejectedCallbackList.push(rejectCallback)
      }
    })
    return promise
  }
  
  catch(callback) {
    this.then(null, callback)
  }
  
  static resolve(value) {
    if (value instanceof Promise) {
      return value
    }
    return new Promise((resolve, reject) => resolve(value))
  }
  
  static reject(reason) {
    if (reason instanceof Promise) {
      return reason
    }
    return new Promise((resolve, reject) => reject(reason))
  }
  
  static all(promises) {
    return new Promise((resolve, reject) => {
      let length = promises.length
      let count = 0
      let res = []
      if (length === 0) return resolve([])
      promises.forEach((item, index) => {
        Promise.resolve(item)
          .then(val => {
          count++
          res[i] = val
          if (count === length) {
            resolve(res)
          }
        })
          .catch(err => reject(err))
      })
    })
  }
  
  static race(promises) {
    return new Promise((resolve, reject) => {
      promises.forEach((item, index) => {
        Promise.resolve(item)
          .then(val => resolve(val))
          .catch(err => reject(err))
      })
    })
  }
  
  static any(promises) {
    return new Promise((resolve, reject) => {
      let length = promises.length
      let count = 0
      if (length === 0) return
      promises.forEach((item, index) => {
        Promise.resolve(item)
          .then(val => resolve(val))
          .catch(err => {
          count++
          if (count === length) {
            return reject("All promises where rejected!")
          }
        })
      })
    })
  }
  
  static allSettled(promises) {
    return new Promise((resolve, reject) => {
      let length = promises.length
      let count = 0
      if (length === 0) return resolve([])
      promises.forEach((item, index) => {
        Promise.resolve(item)
          .then(val => {
          count++
          res[i] = {
            status: "fulfilled",
            value: val
          }
          if (count === length) {
            resolve(res)
          }
        })
          .catch(err => {
          count++
          res[i] = {
            status: "rejected",
            reason: err
          }
          if (count === length) {
            resolve(res)
          }
        })
      })
    })
  }
}
```

:::info
`javaScript` 引擎为了让 `microtask` 尽快的输出，做了一些优化，连续的多个 `then``(3` 个)如果没有 `reject` 或者 `resolve` 会**交替执行 then** 而不至于让一个堵太久完成用户无响应，不单单 `v8` 这样其他引擎也是这样，因为其实 `promise` 内部状态已经结束了。这块在 `v8` 源码里有完整的体现
:::

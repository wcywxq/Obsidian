---
title: Promise 缓存
url: https://www.yuque.com/wcywxq/sd8kbc/sgcusf
---

为了解决 promise 调用常量，每个页面都会调用这个常量，以解决多次调用接口对服务器造成请求的浪费。
可以使用 promise 缓存或者全局状态管理。

```javascript
const cacheMap = new Map()
function enableCache(target, name, descriptor) {
	const val = descriptor.value
  descriptor.value = async function(...args) {
  	const cacheKey = name + JSON.stringify(args)
    if (!cacheMap.get(cacheKey)) {
      const cacheValue = Promise.resolve(val.apply(this, args))
      	.catch(err => {
        	cacheMap.set(cacheKey, null)
        })
      cacheMap.set(cacheKey, cacheValue)
    }
    return cacheMap.get(cacheKey)
  }
  return descriptor
}
class PromiseClass {
	@enableCache
  static async getInfo() {}
}
PromiseClass.getInfo()
PromiseClass.getInfo()
```

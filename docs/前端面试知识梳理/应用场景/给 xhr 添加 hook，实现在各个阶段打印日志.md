---
title: 给 xhr 添加 hook，实现在各个阶段打印日志
url: https://www.yuque.com/wcywxq/pgkpvh/dm9n4g
---

> 重写 xhr 的属性和方法

```javascript
class XhrHook {
	constructor(beforeHook = {}, afterHook = {}) {
  	this.XHR = window.XMLHttpRequest
    this.beforeHooks = beforeHook
    this.afterHooks = afterHook
    
    // 初始化
    this.init()
  }
  
  init() {
    const _self = this
  	// 此处不用剪头函数的问题是因为 this 指向问题，因为实例化后需要指向新构造出的实例上
    window.XMLHttpRequest = function() {
    	this._xhr = new _self.XHR()
      _self.overwrite(this)
    }
  }
  
  overwrite(proxyXHR) {
  	for (let key in proxyXHR._xhr) {
    	if (typeof proxyXHR._xhr[key] === 'function') {
      	this.overwriteMethod(key, proxyXHR)
        continue
      }
      this.overwriteAttributes(key, proxyXHR)
    }
  }
  
  // 重写方法
  overwriteMethod(key, proxyXHR) {
  	let beforeHooks = this.beforeHooks // 我们应该可以拦截原有行为
    let afterHooks = this.afterHooks
    
    proxyXHR[key] = (...args) => {
    	// 拦截
      if (beforeHooks[key]) {
      	const res = beforeHooks[key].call(proxyXHR, args)
        if (res === false) return
      }
      const res = proxyXHR._xhr[key].apply(proxyXHR._xhr, args)
      afterHooks[key] && afterHooks[key].call(proxyXHR._xhr, res)
      return res
    }
  }
  
  // 重写属性
  overwriteAttributes(key, proxyXHR) {
  	Object.defineProperties(proxyXHR, key, this.setPropertyDescriptor(key, proxyXHR))
  }
  
  setPropertyDescriptor(key, proxyXHR) {
  	let obj = Object.create(null) // 得到一个非常干净的对象
    let _self = this
    obj.set = function(val) {
    	if (!key.startsWith('on')) {
      	proxyXHR[`__${key}`] = val
        return
      }
      if (_self.beforeHooks[key]) {
      	this._xhr[key] = function(...args) {
        	_self.beforeHooks[key].call(proxyXHR)
          val.apply(proxyXHR, args)
        }
        return
      }
      this._xhr[key] = val
    }
    obj.get = function() {
    	return proxyXHR[`__${key}`] || this._xhr[key]
    }
    return obj
  }
}
```

---
title: new 关键字
url: https://www.yuque.com/wcywxq/sd8kbc/pldu2b
---

在调用 `new` 的过程中会发生以上 `4` 件事情：

1. 首先创建了一个新的空对象
2. 设置原型，将对象的原型设置为函数的 `prototype` 对象。
3. 让函数的 `this` 指向这个对象，执行构造函数的代码（为这个新对象添加属性）
4. 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。

```javascript
function newFactory(fn, ...args) {
  	// method1
  	const obj = new Object()
    obj.__proto__ = newFactory.prototype
  	newFactory.apply(obj, args)
    return obj
  
  	// method2
    const obj = Object.create(fn.prototype)
    fn.apply(obj, args)
  	return obj
}
```

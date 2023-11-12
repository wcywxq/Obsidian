---
title: call & apply & bind
url: https://www.yuque.com/wcywxq/sd8kbc/ybrsyw
---

<a name="PtZZM"></a>

## call

1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 `call` 等方式调用的情况。
2. 判断传入上下文对象是否存在，如果不存在，则设置为 `window` 。
3. 处理传入的参数，截取第一个参数后的所有参数。
4. 将函数作为上下文对象的一个属性。
5. 使用上下文对象来调用这个方法，并保存返回结果。
6. 删除刚才新增的属性。
7. 返回结果。

```javascript
Function.prototype.call = function(ctx) {
	const args = [...arguments].slice(1)
  const res = null
  ctx = ctx || window
  ctx.fn = this
  res =  ctx.fn(...args)
  delete ctx.fn
  return res
}
```

<a name="iJWCa"></a>

## apply

1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 `call` 等方式调用的情况。
2. 判断传入上下文对象是否存在，如果不存在，则设置为 `window` 。
3. 将函数作为上下文对象的一个属性。
4. 判断参数值是否传入
5. 使用上下文对象来调用这个方法，并保存返回结果。
6. 删除刚才新增的属性
7. 返回结果

```javascript
Function.prototype.apply = function(ctx) {
  let res = null
  // 判断 context 是否存在，如果未传入则为 window
  ctx = ctx || window
  ctx.fn = this
  res = arguments[1] ? ctx.fn(...arguments[1]) : ctx.fn()
  // 将属性删除
  delete ctx.fn
  return res
}
```

<a name="G08P5"></a>

## bind

1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 `call` 等方式调用的情况。
2. 保存当前函数的引用，获取其余传入参数值。
3. 创建一个函数返回
4. 函数内部使用 `apply` 来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的 `this` 给 `apply` 调用，其余情况都传入指定的上下文对象。

```javascript
Function.prototype.bind = function(ctx, ...args) {
	const args = [...arguments].slice(1)
  const fn = this
  
  return function Fn() {
  	return fn.apply(this instanceof Fn ? this : ctx, args.concat(...arguments))
  }
}
```

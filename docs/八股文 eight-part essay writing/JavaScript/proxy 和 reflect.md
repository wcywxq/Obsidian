---
title: proxy 和 reflect
url: https://www.yuque.com/wcywxq/mxunh7/in6cx4
---

<a name="FbJdB"></a>

## proxy

Proxy 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义(如属性查找、赋值、枚举、函数调用等) <a name="MYAFu"></a>

### 语法

```javascript
const p = new Proxy(target, handler)
```

<a name="MVVPv"></a>

### 参数

- target：要使用 Proxy 包装的**目标对象**(可以是任何类型的对象，包括**原生数组**、**函数**，甚至**另一个代理**)
- handler：一个通常以函数作为属性的对象，各属性中的函数分别定义了在执行各种操作时代理 p 的行为 <a name="CDOhV"></a>

### 示例

```javascript
const handler = {
	get(obj, prop) {
  	return prop in obj ? obj[prop] : 37
  }
}
const p = new Proxy({}, handler)
p.a = 1
p.b = undefined
console.log(p.a, p.b) // 1，undefined
console.log('c' in p, p.c) // false，37
```

<a name="moaMA"></a>

## reflect

Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法。这些方法与 proxy handlers(en-US) 的方法相同。Reflect 不是一个函数对象，因此它是不可构造的 <a name="xbzLS"></a>

### 描述

与大多数全局对象不同 Reflect 并非一个构造函数，所以不能通过 new运算符对其进行调用。或者将 Reflect 对象作为一个函数来调用。Reflect 的所有属性和方法都是静态的

Reflect 对象提供了与 proxy handler methods 的命名相同。其中的一些方法与 Object 对象相同，可以说它继承了大部分 Object 的方法，也在许多方法做了相应的优化

- return 不返回 obj，而是返回 true 或者 false

这样可以避免 Object.defineProperty 抛出错误

- 更方便的函数操作

如：Reflect.has(obj, name)、Reflect.deleteProperty(obj, name)

- 更加稳定可靠的函数调用

在 es5 中，如果需要调用一个函数 f，执行上下文指向 obj 对象，还有 args 参数列表，通常是这么写：

```javascript
f.apply(obj, args)
```

然而，函数 f 可能会无意中去定义自己的 apply 方法，所以为了避嫌，可以这么写：

```javascript
Function.prototype.apply.call(f, obj, args)
```

而在 es6 中，可以讲代码缩短：

```javascript
Reflect.apply(f, obj, args)
```

- 调用构造函数时可接受不确定长度的参数列表

在 es6 中可以使用扩展运算符

```javascript
var obj = new F(...args)
```

但是在 es5，这样根本行不通，如果有了 Reflect，就可以这么写：

```javascript
var obj = Reflect.constructor(F, args)
```

- 控制访问器或者读取器的 this

在 es5 中，访问或者定义对象属性可以这样写

```javascript
var name = ... // get property name as a string
obj[name] // generic property lookup
obj[name] = value // generic property update
```

Reflect 也能有相应的方法，此外，还增加了一个 receiver 参数，用于绑定 getter 和 setter 的执行上下文(一般指 Proxy 或者继承 Proxy 的对象)

```javascript
var name = ... // get property name as a string
Reflect.get(obj, name, wrapper) // if obj[name] is an accessor, it gets run with `this === wrapper`
Reflect.set(obj, name, value, wrapper)
```

- 避免直接访问 proto

es5 提供了 Object.getPrototypeOf(obj)，去访问对象的原型；而 es6 也提供了 Reflect.getPrototypeOf(obj) 和 Reflect.setPrototypeOf(obj, newProto)，这个是新的方法去访问和设置对象的原型 <a name="FzdAv"></a>

### 示例

- 检测一个对象是否存在特定属性

```javascript
const duck = {
	name: 'Maurice',
  color: 'white',
  greeting() {
  	console.log(`Quaaack! My name is ${this.name}`)
  }
}
Reflect.has(duck, 'color') // true
Reflect.has(duck, 'haircut') // false
```

- 返回这个对象自身的属性

```javascript
Reflect.ownKeys(duck) // ['name', 'color', 'greeting']
```

- 为这个对象添加一个新的属性

```javascript
Reflect.set(duck, 'eyes', 'black')
// returns 'ture' if successful
// 'duck' now contains the property 'eyes: "black"'
```

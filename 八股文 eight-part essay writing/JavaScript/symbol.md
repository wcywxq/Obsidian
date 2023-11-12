---
title: symbol
url: https://www.yuque.com/wcywxq/mxunh7/qi5rpw
---

<a name="T8XSk"></a>

## 创建 symbol

```javascript
let s = Symbol('foo')
```

- Symbol 函数每次调用会创建一个新的唯一值

```javascript
console.log(Symbol() === Symbol()) // false
```

<a name="eFh6j"></a>

## 共享 symbol

要创建一个共享的 symbol，要使用 Symbol.for() 函数，而不是 Symbol()

Symbol.for 会首先在全局中查找是否有已创建的 ssn 和 symbol，如果有就会返回已经创建的 symbol，如果没有就会创建一个新的 symbol

```javascript
let ssn = Symbol.for('ssn')
let nz = Symbol.for('ssn')
console.log(ssn === nz) // true
console.log(Symbol.keyFor(nz)) // 'ssn'

let systemID = Symbol('sys')
console.log(Symbol.keyFor(systemID)) // undefined
```

<a name="KeWHv"></a>

## Symbol 的作用

- 使用 Symbol 作为唯一值(避免命名重复)
- 使用 Symbol 作为对象属性(实现私有变量)
- 在 call 和 apply 中就有用到

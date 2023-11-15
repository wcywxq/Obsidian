---
title: 实现 es6 的 extends
url: https://www.yuque.com/wcywxq/pgkpvh/vtqbgk
---

```javascript
function B(name) {
  this.name = name
}

function A(name, age) {
  // 1. 将 A 的原型指向 B
  Object.setPrototypeOf(A, B)
  // 2. 用 A 的实例作为 this 调用 B，得到继承 B 之后的实例，这一步相当于调用 super
  Object.getPrototypeOf(A).call(this, name)
  // 3. 将 A 原有的属性添加到新实例上
  this.age = age
  // 4. 返回新实例对象
  return this
}

// example
const a = new A('Bob', 22)
console.log(a)
```

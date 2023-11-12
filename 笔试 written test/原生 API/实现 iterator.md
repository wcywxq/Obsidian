---
title: 实现 iterator
url: https://www.yuque.com/wcywxq/sd8kbc/ir252g
---

```javascript
/* 
  这是一个手写的迭代器(Iterator)
  满足迭代器协议的对象。
  迭代器协议: 对象的 next 方法是一个无参函数，它返回一个对象，该对象拥有 done 和 value 两个属性： 
*/
var it = makeIterator(["a", "b"])

it.next() // { value: "a", done: false }
it.next() // { value: "b", done: false }
it.next() // { value: undefined, done: true }

function makeIterator(array) {
    var nextIndex = 0
    return {
        next: function () {
            return nextIndex < array.length ? { value: array[nextIndex++], done: false } : { value: undefined, done: true }
        }
    }
}
```

<a name="OSWtv"></a>

### 可迭代的迭代器

```javascript
/*
    使迭代器可迭代
    makeIterator函数生成的迭代器并没有实现可迭代协议
    所以不能在for...of等语法中使用。
    可以为该对象实现可迭代协议，在[Symbol.iterator]函数中返回该迭代器自身
    从新名了下函数名称createIterator
*/
function createIterator(array) {
    var nextIndex = 0
    return {
        next: function () {
            return nextIndex < array.length ? { value: array[nextIndex++], done: false } : { value: undefined, done: true }
        },
        [Symbol.iterator]: function () {
            console.log("返回的迭代器:", this)
            return this // 注意这里是对象调用模式，this 指向的就是上层的对象，迭代器
        }
    }
}

var iterator = createIterator([1, 2, 3])
console.log(...iterator)
```

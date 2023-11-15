---
title: generator 和 async
url: https://www.yuque.com/wcywxq/mxunh7/ga86yq
---

<a name="kSwZu"></a>

## generator 函数

<a name="RIwE0"></a>

### 概念

Generator 是 ES6 标准引入的新的数据类型，Generator 可以理解为一个状态机，内部封装了很多状态，同时返回一个迭代器 Iterator 对象。可以通过这个迭代器遍历相关的值及状态。Generator 的显著特点是可以多次返回，每次的返回值作为迭代器的一部分保存下来，可以被我们显示调用。

<a name="hrR0T"></a>

### 声明方式

一般的函数使用 function 声明，return 作为回调，只可以回调 1 次。而 Generator 函数使用 function* 定义，除了 return，还可以使用 yield 返回多次

```javascript
function* foo(x) {
	yield x + 1
  yield x + 2
  return x + 3
}
const res = foo(0) // foo {<suspended>}
res.next() // { value: 1, done: false }
res.next() // { value: 2, done: false }
res.next() // { value: 3, done: true }
res.next() // { value: undefined, done: true }
```

:::tips
在上面的例子中，可以看到，在执行 foo 函数后返回了一个 Generator 函数的实例。它具有 suspended 和 closed，suspended 代表暂停，closed 则为结束，但是这个状态是无法捕获的，我们只能通过 Generator 函数提供的方法获取当前的状态。在执行 next 方法后，顺序执行了 yield 的返回值。返回值有 value 和 done 两个状态。value 为返回值，可以是任意类型。done 的状态为 true/false，true 即为执行完毕。在执行完毕后再次调用返回 `{ value: undefined, done: true }`
:::
:::warning
注意：在遇到 return 的时候，所有剩下的 yield 不再执行，直接返回 `{ value: undefined, done: true }`
::: <a name="MfcBU"></a>

### 方法介绍

Generator 函数提供了 3 个方法：next、return、throw

- next：按步执行，每次返回一个值，同时也可以每次传入新的值作为计算
- return：直接跳过所有步骤，直接返回 `{value: undefined, done: true}`
- throw：根据函数中书写 try...catch 返回 catch 中的内容，如果没有写 try，则直接抛出异常 <a name="CiLqY"></a>

### 遍历

Generator 函数的返回值是一个带有状态的 Generator 实例。它可以被 for...of 调用，进行遍历，并且仅仅只能被 for...of 调用

```javascript
function* foo(x) {
	console.log('start')
  yield x + 1
  console.log('state 1')
  yield x + 2
  console.log('end')
}

const res = foo(0) // foo {<suspended>}

for (let i of res) {
	console.log(i)
}
// state -> 1 -> state 1 -> 2 -> end
res.next() // { value: undefined, done: true }
```

:::tips
调用 for...of 后，在后台调用 next()，当 done 属性为 true 的时候，循环退出。因此 Generator 函数的实例将顺序执行一遍，再次调用时，状态为已完成
::: <a name="e0vC2"></a>

### 状态存储和改变

Generator 函数中 yield 返回的值是可以被变量存储和改变的

```javascript
function* foo(x) {
	let a = yield x + 0
  let b = yield a + 2
  yield x
  yield a
  yield b
}
const res = foo(0)
res.next() // { value: 0, done: false }
res.next(2) // { value: 4, done: false }
res.next(3) // { value: 0, done: false }
res.next(4) // { value: 2, done: false }
res.next(5) // { value: 3, done: false }
```

:::tips
以上的执行结果中，我们可以看到，在第二步的时候，我们传入 2 这个参数，foo 函数中的 a 的变量的值 0 被替换为 2，并且在第 4 次迭代的时候，返回的是 2。而第三次迭代的时候，传入的 3 参数，替换了 b 的值 4，并在第 5 次迭代的时候返回了 3。所以传入的参数，是替代上一次迭代的生成值。
::: <a name="fSy0U"></a>

### yield* 委托

在 Generator 函数中，我们有时需要将多个迭代器的值合并，我们可以使用 yield *  的形式，将执行委托给另外一个 Generator 函数

```javascript
function* foo1() {
	yield 1
  yield 2
  return 'foo1 end'
}
function* foo2() {
	yield 3
  yield 4
}
function* foo() {
  yield* foo1()
  yield* foo2()
  yield 5
}
const result = foo()
console.log(iterator.next()) // { value: 1, done: false }
console.log(iterator.next()) // { value: 2, done: false }
console.log(iterator.next()) // { value: 3, done: false }
console.log(iterator.next()) // { value: 4, done: false }
console.log(iterator.next()) // { value: 5, done: false }
console.log(iterator.next()) // { value: undefined, done: true }
```

:::tips
foo 在执行的时候，首先委托给了 foo1，等 foo1 执行完毕，再委托给 foo2。但是我们发现，'foo1 end' 这一句并没有输出。在整个 Generator 中，return 只能有一次，在委托的时候，所有的 yield 都是以函数表达式的形式出现。return 的值是表达式的结果，在委托结束之前其内部都是暂停的，等待到表达式的结果的时候，将结果直接返回给 foo。此时 foo 内部没有接收到变量，所以未打印。如果我们希望捕获这个值，可以使用 yield foo() 的方式进行获取。
::: <a name="N7QiH"></a>

## async 关键字

<a name="D0Rdx"></a>

### 实现原理

async 函数是 Generator 函数的语法糖，使用关键字 async 来表示，在函数内部使用 await 来表示异步 <a name="o0PiC"></a>

### 简单实现

async/await 语法糖就是使用 **Generator 函数 + 自动执行器** 来运作的

```javascript
// 定义一个 promise，用来模拟异步请求，作用是传入参数
function getNum(num) {
	return new Promise((resolve, reject) => {
  	setTimeout(() => {
    	resolve(num + 1)
    }, 1000)
  })
}

// 自动执行器，如果一个 Generator 函数没有执行完，则会递归调用
function asyncFactory(fn) {
	const gen = fn()
  function next(data) {
  	const res = gen.next(data)
    if (res.done) return res.value
    res.value.then(data => {
    	next(data)
    })
  }
  next()
}

// 所需要执行的 Generator 函数，内部的数据在执行完成一步的 promise 之后，再调用下一步
const fn = function* () {
	const f1 = yield getNum(1)
  const f2 = yield getNum(f1)
  console.log(f2)
}

asyncFactory(fn)
```

:::tips
在执行过程中，判断一个函数的 promise 是否完成，如果已经完成，将结果传入下一个函数，继续重复此步骤
:::

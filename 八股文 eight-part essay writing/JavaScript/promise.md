---
title: promise
url: https://www.yuque.com/wcywxq/mxunh7/vc6k8l
---

:::info
**基本过程：**

1. 初始化 Promise 状态（pending）
2. 立即执行 Promise 中传入的 fn 函数，将Promise 内部 resolve、reject 函数作为参数传递给 fn ，按事件机制时机处理
3. 执行 then(..) 注册回调处理数组（then 方法可被同一个 promise 调用多次）
4. Promise里的关键是要保证，then方法传入的参数 onFulfilled 和 onRejected，必须在then方法被调用的那一轮事件循环之后的新执行栈中执行。
   ::: <a name="EUl0K"></a>

### Promise 中的 then、catch、finally

- promise 必须为：等待态 Pending、执行态 Fulfilled 和拒绝态 Rejected 中的一种，一旦 promise 转台发生改变，将不能够再迁移到其他任何状态(即：状态 immutable)
- .then 和 .catch 都会返回一个新的 Promise
- catch 无论被连接到哪里，都能够捕获上层的错误
- 在 promise 中，返回任意一个非 promise 的值都会被包裹成 promise 对象
- promise 的 .then 或 .catch 可以被调用多次，当 promise 内部的状态一经改变，并且有了一个值，那么后续每次调用 .then 或 .catch 时都会直接拿到该值
- .then 或 .catch 中 return 一个 error 对象并不会抛出错误，所以不会被后续的 .catch 捕获
- .then 或 .catch 返回的值不能是 promise 本身，否则会造成死循环
- .then 或 .catch 的参数期望是函数，传入非函数则会发生值穿透
- .then 方法可以接受两个参数，第一个是处理成功的函数，第二个是处理失败的函数，在某些时候可以认为 catch 是 .then 第二个参数的简便写法
- .finally 方法也会返回一个 promise，它在 promise 结束的时候，无论结果为 resolved 还是 rejected，都会执行里面的回调函数

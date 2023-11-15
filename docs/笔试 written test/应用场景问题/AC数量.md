---
title: AC数量
url: https://www.yuque.com/wcywxq/sd8kbc/kyaq59
---

```typescript
/* 【题目】AC 数量

现有 N 个人参加 ACM 比赛，每个人做出的题目可以通过已知的函数 acBy(i) 获取。

acBy 是一个异步函数，需要请求远程服务器上的 API 来获取结果。其参数 i 取值为 0～N-1，对应参赛人员 1～N；其返回值为一个 Promise<number[]>，代表第 i 个人 AC 的题目编号。

给定参赛人数 N，请计算出本次比赛一共有多少题目被做出了～

示例：参赛人数为 2，请求 acBy(0)、acBy(1) 结果为 [1,2]、[3,4]。则被解答出的题目序号为 1、2、3、4，结果为 4

*/

// 假设 acBy 已定义，可直接调用
function acBy(i: number): Promise<number[]> {
  // @ts-ignore
  return Promise.resolve([i])
}

async function totalAC(n: number) {
  // 请补充代码
  // let ans = []
  let arr = Array.from(new Array(n), (_, k) => k)
  
  return Promise.all(arr.map(k => acBy(k))).then(result => {
    return Array.from(new Set(result.flat(Infinity))).length
  })
  
  // for (const key of arr) {
  //     acBy(key).then(res => {
  //         ans[key] = res // 异步
  //     })
  // }
  
  // let result = Array.from(new Set(ans.flat(Infinity))) // 同步
  
  // return result.length
}

totalAC(10).then(res => console.log(res))
```

---
title: ts 中的 interface 可以给 Function/Array/Class 做声明么
url: https://www.yuque.com/wcywxq/mxunh7/ona7hl
---

```typescript
// 函数类型
interface SearchFunc {
  (source: string, subString: string): boolean
}
let mySearch: SearchFunc
mySearch = function(source: string, subString: string) {
  let result = source.search(subString)
  return result > -1
}
```

```javascript
// Array
interface StringArray {
  [index: number]: string
}

let myArray: StringArray
myArray = ['Bob', 'Fred']
```

```javascript
// Class, constructor 存在于类的静态部分，所以不会检查
interface ClockInterface {
  currentTime: Date
  setTime(d: Date)
}

class Clock implements ClockInterface {
  currentTime: Date
  setTime(d: Date) {
    this.currentTime: d
  }
  constructor(h: number, m: number) {}
}
```

---
title: 实现字符串的 repeat 方法
url: https://www.yuque.com/wcywxq/sd8kbc/hxelqc
---

输入字符串 `s`，以及其重复的次数，输出重复的结果，例如输入 `abc`，`2`，输出 `abcabc`。 <a name="CavGF"></a>

## 利用数组

```javascript
function repeat(s, n) {
    return (new Array(n + 1)).join(s);
}
```

<a name="YWbTi"></a>

## 递归实现

```javascript
function repeat(s, n) {
    return n > 0 ? s.concat(repeat(s, --n)) : "";
}
```

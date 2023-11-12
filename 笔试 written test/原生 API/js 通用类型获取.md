---
title: js 通用类型获取
url: https://www.yuque.com/wcywxq/sd8kbc/tll6lm
---

```javascript
function _typeof(s) {
  const r = Object.prototype.toString.call(s)
  return r.replace(/\[object (.*?)\]/, '$1').toLowerCase()
}
console.log(
  _typeof(),
  _typeof(null),
  _typeof(1),
  _typeof('1'),
  _typeof(true),
  _typeof(Symbol('zs')),
  _typeof({}),
  _typeof([]),
  _typeof(() => {}),
  _typeof(1n)
)
```

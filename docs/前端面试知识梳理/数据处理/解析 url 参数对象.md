---
title: 解析 url 参数对象
url: https://www.yuque.com/wcywxq/pgkpvh/rxw8qp
---

<a name="DBZq9"></a>

## 正则

- method 1

```javascript
function parseParam(url) {  
  const reg = /(?:&|\/?)(\w+)=([^&$]+)/g
  const params = {}
  while(result = reg.exec(url)) {
  	params[result[1]] = result[2]
  }
  return params
}
```

- method 2

```javascript

const getQueryByName = (name) => {
  const queryNameRegex = new RegExp(`[?&]${name}=([^&]*)(&|$)`)
  const queryNameMatch = window.location.search.match(queryNameRegex)
  // 一般都会通过decodeURIComponent解码处理
  return queryNameMatch ? decodeURIComponent(queryNameMatch[1]) : ''
}

// https://www.baidu.com/?name=%E5%89%8D%E7%AB%AF%E8%83%96%E5%A4%B4%E9%B1%BC&sex=boy

console.log(getQueryByName('name'), getQueryByName('sex'))
```

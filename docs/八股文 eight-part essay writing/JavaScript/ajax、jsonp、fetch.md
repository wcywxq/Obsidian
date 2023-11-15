---
title: ajax、jsonp、fetch
url: https://www.yuque.com/wcywxq/mxunh7/irz8zx
---

<a name="qogHC"></a>

## ajax

<a name="VOyeY"></a>

### 实现 ajax 请求的五个步骤

1. 创建 `XMLHttpRequest` 对象
2. 使用 open 方法创建 http 请求，并设置请求地址和请求方式
3. 设置发送的数据，用 send 发送请求
4. 注册事件
5. 获取响应并更新页面

```javascript
function request() {
	// 1 创建 XMLHttpRequest 对象
  var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP')
  // 2 发送请求
  xhr.open('get', 'http://localhost:3000') // 第三个参数表示是否异步
  // 3 发送数据
  xhr.send('id=1')
  // 4 服务端响应
  xhr.onreadystatechange = function() {
    if (xhr.readyState === 4 && xhr.status === 200) {
      var obj = JSON.parse(xhr.responseText)
      console.log(obj)
    }
  }
}
```

<a name="dq8az"></a>

## jsonp

jsonp 实际上是一种跨域解决方案。
:::info
由于 javaScript 跨域请求数据是不可以的，但是 javaScript 跨域请求 javaScript 脚本是被允许的，因此可以把要请求的数据封装成一个 javaScript 语句，做一个方法的调用。另外，在得到脚本后会立即执行，同时会把数据作为参数传递到方法中，从而解决跨域问题。
::: <a name="CrBPb"></a>

### 实现原理

浏览器在 javaScript 请求中，是允许通过 script 标签的 src 跨域请求，可以在请求的结果中添加回调方法名，在请求页面中定义方法，就可获取到跨域请求的数据

```javascript
const generateUrl = (obj, cb) => {
	let dataSrc = ''
  for (let key in obj) {
  	if (obj.hasOwnProperty(key)) {
    	dataSrc += `${key}=${obj[key]}&`
    }
  }
  dataSrc += `callback=${cb}`
  return `${url}?${dataSrc}`
}

const jsonp = ({ url, params, callback }) => {
  return new Promise((resolve, reject) => {
  	const createScript = document.createElement('script')
    createScript.src = generateUrl(params, callback)
    document.body.appendChild(createScript)
    window.callback = data => {
    	resolve(data)
      document.removeChild(createScript)
    }
  })
}
```

:::warning
**jsonp 缺点：**只能实现 get 一种请求
::: <a name="EO66e"></a>

## fetch

fetch 是 web api 中新增的 api，是基于 promise 实现的。用来取代 XMLHttpRequest 的网络请求，其提供了更优雅的接口，更灵活强大的功能。

```javascript
fetch('http://localhost:3000')
  	.then(res => res.json())
		.then(data => console.log(data))
		.catch(err => console.log(err))
```

    		<br /> 		<br /> 	 

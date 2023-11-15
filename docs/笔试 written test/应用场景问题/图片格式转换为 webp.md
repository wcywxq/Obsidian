---
title: 图片格式转换为 webp
url: https://www.yuque.com/wcywxq/sd8kbc/cwlftq
---

阿里云 oss 支持通过链接后面拼接参数来做图片的格式转换，尝试实现把任意图片格式转换为 webp 格式
:::warning
需要注意 webp 兼容性
:::

```javascript
function checkWebp() {
	try {
  	const canvas = document.createElement('canvas')
    return canvas.toDataURL('image/webp').indexOf('data:image/webp') === 0
  } catch (err) {
  	return false
  }
}

export function getWebpImageUrl(url) {
	// 边界条件：url 是否为空
  if (!url) throw new Error('url 不能为空')
  // 边界条件：base64
  if (url.startsWith('data:')) return url
  // 边界条件：浏览器是否支持 webp
  const supportWebp = checkWebp()
  if (!supportWebp) return url
  return `${url}?x-oss-processxxx`
}
```

---
title: 小程序尺寸单位 rpx
url: https://www.yuque.com/wcywxq/mxunh7/dt36ib
---

`rpx（responsive pixel）`: 可以根据屏幕宽度进行自适应。规定屏幕宽为 `750rpx`。公式：

```javascript
const dsWidth = 750

export const screenHeightOfRpx = function () {
  return 750 / env.screenWidth * env.screenHeight
}

export const rpxToPx = function (rpx) {
  return env.screenWidth / 750 * rpx
}

export const pxToRpx = function (px) {
  return 750 / env.screenWidth * px
}
```

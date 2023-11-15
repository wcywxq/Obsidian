---
title: vue.use 具体的功能
url: https://www.yuque.com/wcywxq/mxunh7/bvokpn
---

1. 检查插件是否注册，若已注册，则直接跳出
2. 处理入参，将第一个参数之后的参数收集，并将首部塞入 this 上下文
3. 执行注册方法，调用定义好的 install 方法，传入处理的参数，若没有 install 方法并且插件本身为 function 则直接进行注册

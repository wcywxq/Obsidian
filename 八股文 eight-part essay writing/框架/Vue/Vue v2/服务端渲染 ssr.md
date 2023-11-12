---
title: 服务端渲染 ssr
url: https://www.yuque.com/wcywxq/mxunh7/pw3v8v
---

SSR 也就是服务端渲染，也就是将 Vue 在客户端把标签渲染成 HTML 的工作放在服务端完成，然后再把 html 直接返回给客户端。

SSR 有着更好的 SEO、并且首屏加载速度更快等优点。不过它也有一些缺点，比如我们的开发条件会受到限制，服务器端渲染只支持 beforeCreate 和 created 两个钩子，当我们需要一些外部扩展库时需要特殊处理，服务端渲染应用程序也需要处于 Node.js 的运行环境。还有就是服务器会有更大的负载需求。 <a name="NMYh7"></a>

## 如何等待异步请求的数据返回后，再渲染页面；客户端中的 store 如何同步服务端的数据

1. 组件中添加 serverPrefetch 选项，返回 promise
2. 服务端入口 server-entry.js 添加 context.rendered 回调，渲染完成后执行，自动序列化注入 window.**INITIAL\_STATE** 变量
3. 设置 context.state = store.state
4. 客户端入口获取 window.**INITIAL\_STATE**，设置 store 初始状态 <a name="gUUQF"></a>

## meta、title 等标签如何注入

1. 使用 vue-meta 插件
2. 根组件 app.vue 使用 metaInfo 添加 meta 和 title
3. 子组件中可使用 metaInfo 函数，覆盖默认的 metaInfo
4. server-entry 中拿到 meta，并将其传入到 context 中，渲染 server.html 中的 meta 信息

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    {{{ meta.inject().title.text() }}} 
    {{{ meta.inject().meta.text() }}}
  </head>
  <body>
    <!--vue-ssr-outlet-->
  </body>
</html>
```

<a name="xwW4j"></a>

## 服务端渲染会执行的生命周期

只会执行 beforeCreate 和 created，不要在这两个生命周期里添加定时器或者使用 window 等 <a name="hwYQF"></a>

## 服务端本地开发热更新

webpack 配置读取在内存中

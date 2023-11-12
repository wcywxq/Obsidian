---
title: 渐进式 web 应用程序 pwa 简介
url: https://www.yuque.com/wcywxq/xxble8/fm39qi
---

介绍

> 渐进式 Web 应用程序

<a name="a9dfa62b"></a>

## progressive web app

<a name="52b8c184"></a>

### 优点

1. 显著提高应用加载速度
2. `web` 应用可以在离线环境下使用
3. `web` 应用能够像原生应用一样被添加到主屏幕
4. `web` 应用能在未被激活时发起推送通知
5. `web` 应用于操作系统集成能力进一步提高

<a name="a3b03012"></a>

### 关键技术

- w3c web app manifest

> 由早期的定义在 `html` 页面头部转换成了定义在 `json` 文件中

- `scope`：定义了 `web` 应用的浏览作用域，比如作用域外面的 `URL` 就会打开浏览器而不会在当前`PWA`中继续浏览

- `start_url`：定义了一个 `PWA` 的入口页面

- `orientation`：锁定屏幕旋转

- `theme_color`/`background_color`：主题色与背景色，用于配置一些可定制的操作系统 `UI` 以提高用户体验，比如 `Android` 的状态栏、任务栏等

- service worker
  - 让`web`应用离线使用的第三次尝试
  - 可编程的 `web worker`
  - 像一个位于浏览器与网络之间的客户端代理，可与拦截、处理、相应流经的 `HTTP` 请求
  - 配合 `Cache Storage API`，可以自由管理 `HTTP` 请求文件粒度的缓存

- push notification
  - `push api` 的出现让推送服务具备了向`web`应用推送消息的能力
  - `push API` 不依赖`web`应用和浏览器`UI`存活

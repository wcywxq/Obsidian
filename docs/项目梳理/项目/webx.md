---
title: webx
url: https://www.yuque.com/wcywxq/aggvdq/yghis3
---

<a name="61a3ec66"></a>

## 介绍

最初的话这个项目的雏形其实是叫「统一码」，核心理念就是说希望可以通过一个码（一般指二维码）来进行任意投放，具体来讲就是使用普通浏览器扫码能打开 `H5` 营销页面，微信扫码能打开小程序，并在小程序打开对应页面，支付宝扫码能打开支付宝小程序，并在小程序打开具体页面等等。其实本质上二维码投放的就是一个特殊的链接，我们通过解析链接的域名，`path`，动态 `path` 参数等信息来在不同平台做不同的解析，以达到上述效果。里面就涉及到了服务端的重定向、具体的 `H5` 跟 `path` 参数一一对应的处理、小程序的处理等

`webx-mp-sdk`，实际上就是小程序这一块的工作内容

`webx`九章是以 `webx`技术体系为基础，建设一套集团流量分发的数据标准和规范，目的在于持续建设增长数据体系，服务全集团的用户增长拉新。

实现这个能力的话我们首先需要原生平台的支持，微信和支付宝小程序其实都在那时候提供了扫描普通链接二维码打开小程序的能力，这其实就对我们实现这个能力提供了可能性。后续这个能力整体升级成 `webx`，并建立了 `webx` 团队，也给统一码平台起了个新名字叫赤兔平台（赤兔马（码））--- 一站式营销活动平台。

`dchn` 是 `webx` 体系中一个重要的概念，一个 `dchn` 是一个投放 id，对应一个具体投放。每一个投放出去的最小力度也就是一个 “码”，也就是一个 `dchn`。根据不同的投放方式，`dchn`可以体现为：链接、图形码、`Scheme`等多种形式。在 `dchn` 访问时，`webx` 服务会根据 `dchn` 所属活动与产品，结合当前终端环境 `xenv`，进行跳转决策。

从产品维度来看，小程序内页面分为三种类型：

- 公共页面，如目的地选择页面
- 产品页面，有明确产品归属的页面
- 未定义类别页面
- webview 页面，h5 投放页

`xpsid` 是全局唯一的 `id`，标识用户某次产品访问的会话实例(即：用来记录会话生成与信息记录)。`xpsid` 由用户访问产品时系统分配 `xpsid`，并进行信息记录。当用户访问产品时，一个 `xpsid` 作为用户访问某一产品(`prod_key`)的一个实例标识，作为埋点与请求公参进行上报，同时在跳转到下一个产品时需要作为来源进行传递。

赤兔平台了许多功能，如：

- 统一物料二维码
- 自动适配扫码 app、根据投放平台一键生成对应格式链接
- 投放关联标准入口渠道，自动聚合统计分析
- 整合各营销平台与服务，各类产品与工具快速接入中

赤兔平台为营销活动提供策略配置，协同管理，分组实验(ab实验)，物料分享，数据分析以及支持强大的投放场景等全流程的投放支持。

***

这一块本质上就是请求服务端并返回配置信息我们做对应操作

难点其实是在于我们对于统一码这个功能保持继续维护的情况下，需要新增其他各种各样的能力，`webx` 的主要目标是提供统一的集团工具库，计划会**收入什么埋点**呀、**接口请求**啊、**链接跳转**、以及**各种通用工具**、比如`H5`在小程序中的**分享**、**消息订阅**、**跳转原生页面**，以及**统一的 passport 登录接入**，以及对应的跨端功能抹平

***

之前的九章信息全部是页面维度的，改成了一个队列中心，同时也为以后接入这些功能做准备

之前是直接在页面上上报，现在是把所有的信息直接推送进任务队列里面，依次执行，也提供了各式各样的任务接口，可以推送 `omega` 埋点任务、普通函数执行任务等，只要是推送进来的任务，都是会被保证按顺序执行的，也支持异步任务

以前是 `A` 跳 `B`，在 `A` 中上报 `A`，`B` 中上报 `B`，现在是 `A` 在 `A` 中 `push` 任务到队列，`B` 中 `push` 任务到队列，**队列按顺序处理**，自动建立 `A` 到 `B` 的关系。之前是因为两个页面中上报会导致上报时机偏差问题，也不能很好的建立 A 和 B 之间的关系

<a name="a4425dc2"></a>

## 任务队列实现

```javascript
class ImmeExecPromise {
    constructor() {
        this.task = []
        this.executing = false
    }
    push(data) {
        // 可封装多个 push 来实现不同任务的推送
        this.task.push(data)
        this.exec()
    }
    create(data) {
        let cachePromise
        let cacheResolve = new Promise(r => {
            cachePromise = r
        })
        this.task.push(
            new Promise(resolve => {
                cachePromise((rest = {}) => resolve(Object.assign(data, rest)))
                setTimeout(() => {
                    resolve(data)
                }, 2000) // 超时
            })
        )
        this.exec()
        return cacheResolve
    }
    async exec() {
        if (this.executing) return
        this.executing = true
        while (this.task.length) {
            let data = await (this.task.shift() || {})
            console.log(data) // 这里根据 data 的 type 可以调用对应执行函数
        }
        this.executing = false
    }
}

let p = new ImmeExecPromise()

p.push({ key: "a" })
p.push({ key: "b" })
let r = p.create({ key: "c" })
p.push({ key: "d" })
setTimeout(() => {
    r.then(f => f({ sub: "cc" }))
}, 1500)
let r2 = p.create({ key: "e" })
setTimeout(() => {
    r.then(f => f({ sub: "ee" }))
}, 2500) // 超时，自动执行，打印中不存在 sub 属性
```

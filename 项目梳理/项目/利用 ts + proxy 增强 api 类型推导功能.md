---
title: 利用 ts + proxy 增强 api 类型推导功能
url: https://www.yuque.com/wcywxq/aggvdq/wagl2t
---

<a name="YWnaB"></a>

## 接口 response 配置文件

```typescript
interface IUserGetArg {
  id: string
}

interface IUserGetRe {
  name: string
}

export default interface IUserAPI {
  user: {
    get: (get: IUserGetArg) => IUserGetRe
  };
  login: {
    "verify-code": {
      get: (arg: IUserGetArg) => IUserGetRe
    }
  }
}
```

<a name="RXo14"></a>

## 请求函数类型映射文件

```typescript
import { HttpConfig } from "."
import IUserAPI from "./user"

// 后端返回格式
export interface IResponse<T = any> {
  code: number
  message: string
  success: boolean
  data: T
}
// 只对继承改类型的函数进行映射，也就是当函数有0和1个参数时映射
type AnyFn = (arg: any) => any
// 函数返回结果的格式
type FnReturn<T extends AnyFn> = Promise<IResponse<ReturnType<T>>>
// 函数没定义参数时的映射格式
type Fn1<T extends AnyFn> = (config?: HttpConfig) => FnReturn<T>
// 函数定义参数时的映射格式
type Fn2<T extends AnyFn> = (arg: Parameters<T>[0], config?: HttpConfig) => FnReturn<T>
// 根据有无参数映射上面两个函数格式
type HttpMethod<T extends AnyFn> = Parameters<T>[0] extends undefined | null ? Fn1<T> : Fn2<T>

// 对传入的类型深度处理所有为 AnyFn 的子类型为 Fn1 或 Fn2
type ToPromise<T> = {
  [K in keyof T]: T[K] extends AnyFn ? HttpMethod<T[K]> : ToPromise<T[K]>
}

// 把所有的 API 类型合并为一个
interface IMixinAPI extends IUserAPI {}

// 对合并后的类型进行映射处理
export default interface IRootAPI extends ToPromise<IMixinAPI> {
  url: string
}
```

<a name="JGVGN"></a>

## 基于 proxy 的拦截器配置文件

```typescript
import axios, { AxiosRequestConfig, AxiosResponse } from "axios"
import IRootAPI, { IResponse } from "./mixins"

const service = axios.create({
  baseURL: "",
  timeout: 10000,
})

export class HttpConfig {
  public config: AxiosRequestConfig
  
  constructor(config: AxiosRequestConfig) {
    this.config = config
  }
}

const handler: ProxyHandler<IRootAPI> = {
  get: function get(target, prop): any {
    let value: any // 用于为 undefined 的属性赋值
    // 对象取值，效果和 target[prop] 类似
    const item = Reflect.get(target, prop)
    // 这里主要是拿 service.get, service.post, service.put, service.delete，为了方便就写在一起
    const httpMethod = Reflect.get(service, prop)
    const method = {
      params: (arg1?: any, arg2?: HttpConfig) => {
        return arg1 instanceof HttpConfig
          ? httpMethod(target.url, arg1?.config)
          : httpMethod(target.url, {
              params: { ...arg1 },
              ...arg2
            })
      },
      data: (arg1?: any, arg2?: HttpConfig) => {
        return arg1 instanceof HttpConfig
          ? httpMethod(target.url, arg1?.config)
          : httpMethod(target.url, { ...arg1 }, arg2?.config)
      }
    }
    
    switch (prop) {
      case "get" || "delete":
        value = methods.params
        break
      case "post" || "put":
        value = methods.data
        break
      case "url":
        value = item
        break
      default:
        value = {
          ...item,
          url: `${target.url ?? ""}/${prop.toString()}`
        }
        break
    }
    
    if (!item || typeof item !== "function") {
      Reflect.set(target, prop, value)
    }
    
    return new Proxy(Reflect.get(target, prop), { get })
  }
}

export const http = new Proxy({} as IRootAPI, handler)
```

<a name="tsVLW"></a>

## 使用方式

```tsx
import React, { useEffect } from "react"
import logo from "./logo.svg"
import "./App.css"
import { http } from "./api"

function App() {
  useEffect(() => {
    http.login["verify-code"].get({ id: "123" }, {
      config: {
        url: "http://www.baidu.com"
      }
    }).then(val => {
      console.log(val)
    })
  }, [])
  
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>Edit <code>src/App.tsx</code> and save to reload.</p>
        <a className="App-link" href="http://reactjs.org" target="_blank" rel="noopener noreferrer">
          Learn React
        </a>
      </header>
    </div>
  )
}
```

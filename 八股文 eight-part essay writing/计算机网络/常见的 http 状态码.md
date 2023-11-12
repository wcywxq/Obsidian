---
title: 常见的 http 状态码
url: https://www.yuque.com/wcywxq/mxunh7/imp49b
---

<a name="DxoBk"></a>

## 1XX(代表服务器接收到请求)

<a name="WAghT"></a>

## 2XX(Success 成功状态码)

- 200 OK 表明请求已经成功. 默认情况下状态码为200的响应可以被缓存。
- 204 No Content 表示目前请求成功，但客户端不需要更新其现有页面 <a name="jjRA8"></a>

## 3XX(Redirection 重定向状态码)

- 301 Moved Permanently 永久重定向。说明请求的资源已经被移动到了由 Location 头部指定的 url 上，是固定的不会再改变。搜索引擎会根据该响应修正。
- 302 Found 临时重定向。重定向状态码表明请求的资源被暂时的移动到了由 Location 头部指定的 URL 上。浏览器会重定向到这个URL，但是搜索引擎不会对该资源的链接进行更新
- 304 Not Modified 说明无需再次传输请求的内容，也就是说可以使用缓存的内容。 <a name="HJs0M"></a>

## 4XX(Client Error 客户端错误状态码)

- 400 Bad Request 客户端请求的语法错误
- 401 Unauthorized 说明由于缺乏目标资源要求的身份验证凭证，发送的请求未得到满足。
- 403 Forbidden 指的是服务器端有能力处理该请求，但是拒绝授权访问。进入该状态后，不能再继续进行验证。该访问是永久禁止的，并且与应用逻辑密切相关（例如不正确的密码）
- 404 Not Found 说明服务器端无法找到所请求的资源。返回该响应的链接通常称为坏链（broken link）或死链（dead link），它们会导向链接出错处理
- 405 Method Not Allowed 表明服务器禁止了使用当前 HTTP 方法的请求。需要注意的是，GET 与 HEAD 两个方法不得被禁止，当然也不得返回状态码 405。 <a name="NGbg5"></a>

## 5XX(Server Error 服务器错误状态码)

- 500 Internal Server Error 表示所请求的服务器遇到意外的情况并阻止其执行请求。
- 502 Bad Gateway 表示作为网关或代理角色的服务器，从上游服务器（如tomcat、php-fpm）中接收到的响应是无效的。
- 503 Service Unavailable 表示服务器尚未处于可以接受请求的状态。通常造成这种情况的原因是由于服务器停机维护或者已超载。

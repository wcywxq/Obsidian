---
title: http 请求的 method
url: https://www.yuque.com/wcywxq/mxunh7/fpmvpg
---

<a name="Bo66Q"></a>

## 各个 http 版本所支持的 method

|&#x20;
&#x20;| [GET](#SCoN1) | [POST](#fpM3o) | [HEAD](#GKUxY) | [PUT](#QX6KZ) | [DELETE](#QonXS) | [PATCH](#pRanL) | [OPTIONS](#Wby7B) | [CONNECT](#XzBQK) | [TRACE](#qDU4y) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| http/1.0 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| http/1.1 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

<a name="cKrGY"></a>

## http 各个 method 介绍

<a name="SCoN1"></a>

### GET

HTTP GET 方法请求指定的资源。使用 GET 的请求应该只用于获取数据。 <a name="fpM3o"></a>

### POST

HTTP POST 方法 发送数据给服务器。请求主体的类型由 [Content-Type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) 首部指定。
PUT 和[POST](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST)方法的区别是，PUT 方法是幂等的：连续调用一次或者多次的效果相同（无副作用）。连续调用同一个 POST 可能会带来额外的影响，比如多次提交订单。
一个 POST 请求通常是通过 [HTML 表单](https://developer.mozilla.org/en-US/docs/Learn/Forms)发送，并返回服务器的修改结果。在这种情况下，content type 是通过在 [<form>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form) 元素中设置正确的 [enctype](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form#attr-enctype) 属性，或是在 [<input>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input) 和 [<button>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button) 元素中设置 [formenctype](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input#attr-formenctype) 属性来选择的:

- application/x-www-form-urlencoded: 数据被编码成以 '&' 分隔的键 - 值对，同时以 '=' 分隔键和值。非字母或数字的字符会被 [percent-encoding](https://developer.mozilla.org/zh-CN/docs/Glossary/percent-encoding): 这也就是为什么这种类型不支持二进制数据 (应使用 multipart/form-data 代替).
- multipart/form-data
- text/plain

当 POST 请求是通过除 HTML 表单之外的方式发送时，例如使用 [XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest), 那么请求主体可以是任何类型。按 HTTP 1.1 规范中描述，POST 为了以统一的方法来涵盖以下功能：

- 注释已有的资源
- 在公告板，新闻组，邮件列表或类似的文章组中发布消息;
- 通过注册新增用户;
- 向数据处理程序提供一批数据，例如提交一个表单;
- 通过追加操作，扩展数据库数据。 <a name="To9Mk"></a>

#### 例子

使用默认的 application/x-www-form-urlencoded 做为 content type 的简单表单：

```latex
POST / HTTP/1.1
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

say=Hi&to=Mom
```

使用 multipart/form-data 作为 content type 的表单：

    POST /test.html HTTP/1.1
    Host: example.org
    Content-Type: multipart/form-data;boundary="boundary"

    --boundary
    Content-Disposition: form-data; name="field1"

    value1
    --boundary
    Content-Disposition: form-data; name="field2"; filename="example.txt"

    value2

<a name="GKUxY"></a>

### HEAD

HEAD 几乎与 GET 相同，但没有响应正文。
换句话说，如果 GET /users 返回用户列表，那么 HEAD /users 将发出相同的请求，但不会返回用户列表。
HEAD 请求对于在实际发出 GET 请求之前检查 GET 请求将返回的内容很有用——比如在下载大文件或响应正文之前。
:::tips
HTTP HEAD 方法请求资源的头部信息，并且这些头部与 HTTP [GET](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 方法请求时返回的一致。该请求方法的一个使用场景是在下载一个大文件前先获取其大小再决定是否要下载，以此可以节约带宽资源。
HEAD 方法的响应不应包含响应正文。即使包含了正文也必须忽略掉. 虽然描述正文信息的 [entity headers](https://developer.mozilla.org/zh-CN/docs/Glossary/Entity_header), 例如 [Content-Length](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length) 可能会包含在响应中，但它们并不是用来描述 HEAD 响应本身的，而是用来描述同样情况下的 [GET](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 请求应该返回的响应。
如果 HEAD 请求的结果显示在上一次 [GET](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 请求后缓存的资源已经过期了，即使没有发出[GET](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET)请求，缓存也会失效
::: <a name="fmoX9"></a>

#### 例子

```latex
HEAD /index.html
```

<a name="QX6KZ"></a>

### PUT

HTTP PUT 请求方法使用请求中的负载创建或者替换目标资源。
PUT 与 [POST](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 方法的区别在于，PUT 方法是幂等的：调用一次与连续调用多次是等价的（即没有副作用），而连续调用多次 POST 方法可能会有副作用，比如将一个订单重复提交多次。 <a name="FFmPo"></a>

#### 例子

<a name="XNFtl"></a>

##### 请求

```latex
PUT /new.html HTTP/1.1
Host: example.com
Content-type: text/html
Content-length: 16

<p>New File</p>
```

<a name="DT2jH"></a>

##### 响应

如果目标资源不存在，并且 PUT 方法成功创建了一份，那么源头服务器必须返回[201](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/201) (Created) 来通知客户端资源已创建。

```latex
HTTP/1.1 201 Created
Content-Location: /new.html
```

如果目标资源已经存在，并且依照请求中封装的表现形式成功进行了更新，那么，源头服务器必须返回[200](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200) (OK) 或者[204](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/204) (No Content) 来表示请求的成功完成。

```latex
HTTP/1.1 204 No Content
Content-Location: /existing.html
```

<a name="QonXS"></a>

### DELETE

DELETE 方法删除指定的资源。 <a name="aye5L"></a>

#### 例子

<a name="doXt2"></a>

##### 请求

    DELETE /file.html HTTP/1.1

<a name="EnXAy"></a>

##### 响应

- 如果 DELETE 方法成功执行，那么可能会有以下几种状态码：
  - 状态码 [202](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/202) (Accepted) 表示请求的操作可能会成功执行，但是尚未开始执行。
  - 状态码 [204](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/204) (No Content) 表示操作已执行，但是无进一步的相关信息。
  - 状态码 [200](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200) (OK) 表示操作已执行，并且响应中提供了相关状态的描述信息。

```html
HTTP/1.1 200 OK
Date: Wed, 21 Oct 2015 07:28:00 GMT

<html>
  <body>
    <h1>File deleted.</h1>
  </body>
</html>
```

<a name="pRanL"></a>

### PATCH

PATCH 方法用于对资源应用部分修改。
:::tips
在 HTTP 协议中，请求方法 PATCH 用于对资源进行部分修改。
在 HTTP 协议中， [PUT](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/PUT) 方法已经被用来表示对资源进行整体覆盖， 而 [POST](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 方法则没有对标准的补丁格式的提供支持。不同于 PUT 方法，而与 POST 方法类似，PATCH 方法是非幂等的，这就意味着连续多个的相同请求会产生不同的效果。
要判断一台服务器是否支持 PATCH 方法，那么就看它是否将其添加到了响应首部 [Allow](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Allow) 或者 [Access-Control-Allow-Methods](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)（在跨域访问的场合，CORS）的方法列表中 。
另外一个支持 PATCH 方法的隐含迹象是 [Accept-Patch](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Patch) 首部的出现，这个首部明确了服务器端可以接受的补丁文件的格式。
::: <a name="fzDyJ"></a>

#### 例子

<a name="XUjMx"></a>

##### 请求

```latex
PATCH /file.txt HTTP/1.1
Host: www.example.com
Content-Type: application/example
If-Match: "e0023aa4e"
Content-Length: 100

[description of changes]
```

<a name="e4z7T"></a>

##### 响应

[204](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/204) 状态码表示这是一个操作成功的响应，因为响应中不带有消息主体。

```latex
HTTP/1.1 204 No Content
Content-Location: /file.txt
ETag: "e0023aa4f"
```

<a name="Wby7B"></a>

### OPTIONS

OPTIONS 方法描述了目标资源的通信选项。
:::tips
HTTP 的 OPTIONS 方法用于获取目的资源所支持的通信选项。客户端可以对特定的 URL 使用 OPTIONS 方法，也可以对整站（通过将 URL 设置为“*”）使用该方法。
::: <a name="i4K2g"></a>

#### 例子

<a name="z4voM"></a>

##### 检测服务器所支持的请求方法

可以使用 OPTIONS 方法对服务器发起请求，以检测服务器支持哪些 HTTP 方法：

```latex
curl -X OPTIONS http://example.org -i
```

响应报文包含一个 [Allow](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Allow) 首部字段，该字段的值表明了服务器支持的所有 HTTP 方法：

```latex
HTTP/1.1 200 OK
Allow: OPTIONS, GET, HEAD, POST
Cache-Control: max-age=604800
Date: Thu, 13 Oct 2016 11:45:00 GMT
Expires: Thu, 20 Oct 2016 11:45:00 GMT
Server: EOS (lax004/2813)
x-ec-custom-error: 1
Content-Length: 0
```

<a name="xmG9T"></a>

##### CORS 中的预检请求

在 [CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS) 中，可以使用 OPTIONS 方法发起一个预检请求，以检测实际请求是否可以被服务器所接受。预检请求报文中的 [Access-Control-Request-Method](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Method) 首部字段告知服务器实际请求所使用的 HTTP 方法；[Access-Control-Request-Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Headers) 首部字段告知服务器实际请求所携带的自定义首部字段。服务器基于从预检请求获得的信息来判断，是否接受接下来的实际请求。

```latex
OPTIONS /resources/post-here/ HTTP/1.1
Host: bar.other
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Origin: http://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
```

服务器所返回的 [Access-Control-Allow-Methods](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Methods) 首部字段将所有允许的请求方法告知客户端。该首部字段与 [Allow](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Allow) 类似，但只能用于涉及到 CORS 的场景中。

```latex
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```

<a name="XzBQK"></a>

### CONNECT

CONNECT 方法用于启动与请求资源的双向通信（隧道）。
:::tips
在 HTTP 协议中，CONNECT 方法可以开启一个客户端与所请求资源之间的双向沟通的通道。它可以用来创建隧道（tunnel）。
例如，CONNECT 可以用来访问采用了 [SSL](https://developer.mozilla.org/zh-CN/docs/Glossary/SSL) ([HTTPS](https://developer.mozilla.org/zh-CN/docs/Glossary/https)) 协议的站点。客户端要求代理服务器将 TCP 连接作为通往目的主机隧道。之后该服务器会代替客户端与目的主机建立连接。连接建立好之后，代理服务器会面向客户端发送或接收 TCP 消息流。
CONNECT 是一个应用范围为点到点的方法。
::: <a name="tdJZp"></a>

#### 例子

    CONNECT server.example.com:80 HTTP/1.1
    Host: server.example.com:80
    Proxy-Authorization: basic aGVsbG86d29ybGQ=

<a name="qDU4y"></a>

### TRACE

HTTP TRACE 方法 实现沿通向目标资源的路径的消息环回（loop-back）测试 ，提供了一种实用的 debug 机制。
请求的最终接收者应当原样反射（reflect）它接收到的消息，除了以下字段部分，作为一个[Content-Type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) 为 message/http 的 200（OK）响应的消息的主体（body）返回给客户端 。
最终接收者是指初始（origin）服务器，或者第一个接收到 [Max-Forwards](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Max-Forwards) 值为 0 的请求的服务器。

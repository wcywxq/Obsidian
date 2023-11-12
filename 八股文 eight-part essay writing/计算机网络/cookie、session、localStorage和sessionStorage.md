---
title: cookie、session、localStorage和sessionStorage
url: https://www.yuque.com/wcywxq/mxunh7/bk9qnc
---

<a name="vupLO"></a>

## cookie

由于 http 是一种无状态的协议，服务器单从网络连接上是无法知道客户身份的。这时候服务器就需要给客户端颁发一个 cookie，用来确认用户的身份。简单的说，cookie 就是客户端保存用户信息的一种机制，用来记录用户的一些信息。
cookie 用于数据存储，同时 cookie 存放的数据量非常小，它的大小限制在 4kb 之内，可以缓存用户的登陆信息 <a name="kfCAk"></a>

### cookie 的构成字段

服务器端向客户端发送 cookie 是通过 http 响应报文实现的，在 Set-Cookie 中设置需要向客户端发送的 cookie，cookie 格式如下：

```javascript
Set-Cookie: "name=value;domain=.domain.com;path=/;expires=Sat, 11 Jun 2019 11:29:42 GMT;HttpOnly;secure"
```

其中 `name=value` 是必选项，其它都是可选项。Cookie 的主要构成如下：

- `Name`：一个唯一确定的 cookie 名称。通常来讲 cookie 的名称是不区分大小写的
- `Value`：存储在 cookie 中的字符串值。最好为 cookie 的 name 和 value 进行 url 编码
- `Domain`：cookie 对于哪个域是有效的。所有向该域发送的请求中都会包含这个 cookie 信息。这个值可以包含子域(如：[http://e.baidu.com](https://link.zhihu.com/?target=http%3A//e.baidu.com))，也可以不包含它(如：[http://baidu.com](https://link.zhihu.com/?target=http%3A//baidu.com)，则对于 [http://baidu.com](https://link.zhihu.com/?target=http%3A//baidu.com) 的所有子域都有效)
- `Path`: 表示这个 cookie 影响到的路径，浏览器跟会根据这项配置，向指定域中匹配的路径发送 cookie
- `Expires`：失效时间，表示 cookie 何时应该被删除的时间戳(即何时应该停止向服务器发送这个 cookie)。如果不设置这个时间戳，浏览器会在页面关闭时即将删除所有 cookie；不过也可以自己设置删除时间。这个值是 GMT 时间格式。如果客户端和服务器端时间不一致，使用 expires 就会存在偏差。并且如果给 cookie 设置一个过去的时间，浏览器会立即删除该 cookie
- `MaxAge`：与 expires 作用相同，用来告诉浏览器此 cookie 多久过期（单位是s），而不是一个固定的时间点。正常情况下，max-age的优先级高于 expires
  - `MaxAge > 0 `： 表示可以长期存储，比如：设置 `cookie` 可以存在一个小时
  - `MaxAge < 0` : 表示 `cookie` 只存在于浏览器的内存中，随着浏览器的关闭而消失
  - `MaxAge = 0 `： 表示 `cookie` 立刻删除
- `HttpOnly`：告知浏览器不允许通过脚本 document.cookie 去更改这个值，同样这个值在 document.cookie 中也不可见。但在 http 请求中仍然会携带这个 cookie。注意这个值虽然在脚本中不可获取，但仍然在浏览器安装目录中以文件形式存在。这项设置通常在服务器端设置。(如果设置不正确可能会导致 cookie 泄漏)
- `Secure`：安全标志，指定后，只有在使用 `SSL` 链接时候才能发送到服务器，如果是 `http` 链接则不会传递该信息。
- `SameSite`：用于限制第三方 `Cookie` 的发送场景，可以取值：
  - `Strict`：完全禁止第三方 `Cookie`，跨站点时，任何情况下都不会发送 `Cookie`。
  - `Lax`：默认值。除了下面三种情况外，不发送第三方 `Cookie`
    - 链接：`<a href="..."></a>`
    - 预加载请求：`<link rel="prerender" href="..."/>`
    - `GET` 表单：`<form method="GET" action="...">`
  - `None`：跨站都发送 `Cookie`
    :::info
    `SameSite` 属性可以防范 `CSRF` 攻击和用户追踪
    ::: <a name="fuIA8"></a>

### cookie 的作用

- cookie 缓存的数据始终在同源的 http 中携带，在浏览器和服务器之间进行传递
- cookie 缓存可以做数据持久化，cookie 数据在设置的 cookie 过期时间之前都有效
  :::warning
  数据在缓存中，关闭浏览器窗口并不能删除 cookie 数据
  ::: <a name="fgz6l"></a>

### 跨域时如何携带 cookie

1. 前端请求时，设置 `request` 对象的属性 `withCredentials` 为 `true`(携带认证信息)
2. 服务端 `header` 设置 `Access-Control-Allow-Origin`
3. 服务端 `header` 设置 `Access-Control-Allow-Credentials` <a name="kJQ8z"></a>

### cookie 的修改

- 如果要修改某个 `cookie`，只需要新建一个同名的 `cookie`，添加到 `response` 中覆盖原来的 `cookie`

- 如果要删除某个 `cookie`，只需要新建一个同名的 `cookie`，并将 `maxAge` 设置为 `0`，并添加到 `response` 中覆盖原来的 `cookie`
  :::warning
  **注意：**

- `**maxAge**`**设置为 0 而不是负数，负数代表其他含义**

- **修改、删除 **`**cookie**`** 时，新建的 **`**cookie**`** 除 **`**value**`**、**`**maxAge**`** 之外的所有属性，例如 **`**name**`**、**`**path**`**、**`**domain**`** 等，都要与原 **`**cookie**`** 完全一样。否则，浏览器将视为两个不同的 **`**cookie**`** 不予覆盖，导致修改、删除失败。**
  ::: <a name="bQFo6"></a>

## session

`session` 是另一种记录客户状态的机制，不同的是 `cookie` 保存在**客户端浏览器**中，而 `session` 保存在**服务器**上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。
客户端浏览器再次访问时只需要从该 `session` 中查找该客户的状态就可以了 <a name="Lf6dt"></a>

### session 工作步骤

因为 http 协议是无状态的，session 不能依据 http 连接来判断是否为同一个用户。于是乎：服务器向用户浏览器发送了一个名为 sessionID 的 cookie，它的值是 session 的 id 值。其实 session 依据 cookie 来识别是否是同一个用户。该 cookie 是服务器自动颁发给浏览器的，不用我们手工创建的。该 cookie 的 maxAge 值默认是 -1，也就是说仅当前浏览器使用，不将该 cookie 存在硬盘中，并且各浏览器窗口间不共享，关闭浏览器就会失效。

1. 产生 sessionID：session 是基于 cookie 的一种方案，所以，首先要产生 cookie。client 第一次访问 server，server 生成一个随机数，命名为 sessionID，并将其放在响应头里，以 cookie 的形式返回给 client，client 以处理其他 cookie 的方式处理这段 cookie。大概是这样：cookie：sessionID=135165432165
2. 保存 sessionID： server 将要保存的数据保存在相对应的 sessionID 之下，再将 sessionID 保存到服务器端的特定的保存 session 的内存中（如：一个叫 session 的哈希表）
3. 使用 session： client 再次访问 server，会带上首次访问时获得的 值为 sessionID 的 cookie，server 读取 cookie 中的 sessionID，根据 sessionID 到保存 session 的内存寻找与 sessionID 匹配的数据，若寻找成功就将数据返回给 client。 <a name="UUmmF"></a>

### session的有效期

- `session` 保存在服务器端。为了获得更高的存取速度，服务器一般把 `session` 放在内存里。每个用户都会有一个独立的 `session`。如果 `session` 内容过于复杂，当大量客户访问服务器时可能会导致内存溢出。因此，`session` 里的信息应该尽量精简。
- `session` 生成后，只要用户继续访问，服务器就会更新 `session` 的最后访问时间，并维护该 `session`。用户每访问服务器一次，无论是否读写 `session`，服务器都认为该用户的 `session`“活跃（active）”了一次。
- 由于会有越来越多的用户访问服务器，因此 `session` 也会越来越多。为防止内存溢出，服务器会把长时间内没有活跃的 `session` 从内存删除。这个时间就是 `session` 的超时时间。如果超过了超时时间没访问过服务器，`session` 就自动失效了 <a name="kyKGt"></a>

## cookie 和 session 的区别

- `cookie` 数据存放在客户端，`session` 数据放在服务器端
- `cookie` 的安全性一般，他人可通过分析存放在本地的 `cookie` 并进行 `cookie` 欺骗。在安全性第一的前提下，选择 `session` 更优。重要交互信息比如权限等就要放在 `session` 中，一般的信息记录放 `cookie` 中
- 单个 `cookie` 保存的数据不能超过 `4K`，很多浏览器都限制一个站点最多保存 `20` 个 `cookie`，而 `session` 原则上没有限制
- `session` 会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用 `cookie`
- `session` 的运行依赖 `session ID`，而 `session ID` 是存在 `cookie` 中的，也就是说，如果浏览器禁用了 `cookie`，`session` 也会失效（但是可以通过其它方式实现，比如在 url 中传递 `session ID`，也就是地址重写） <a name="JTdBD"></a>

## localStorage

- `localStorage` 与 `HTTP` 没有任何关系，所以在 `HTTP` 请求时不会带上 `localStorage` 的值
- 只有相同域名的页面才能互相读取 `localStorage`，同源策略与 `cookie` 一致
- 不同的浏览器，对每个域名 `localStorage` 的最大存储量的规定不一样，超出存储量会被拒绝。最大存 `5M` 超过 `5M` 的数据就会丢失。而 `Chrome 10MB` 左右
- 常用来记录一些不敏感的信息
- `localStorage` 理论上永久有效，除非用户清理缓存
- `localStorage` 只能存储字符串类型的数据，如果是其他类型数据，将会自动转换为字符串格式进行存储
- `localStorage` 本质上是一个 `hash`（哈希表），是一个存在于浏览器上的 `hash`（哈希表） <a name="jeGkj"></a>

## sessionStorage

- `sessionStorage` 的所有性质基本上与 `localStorage` 一致，唯一的不同区别在于：`sessionStorage` 的有效期是页面会话持续，如果页面会话（`session`）结束（**关闭窗口或标签页**），`sessionStorage` 就会消失。而 `localStorage` 则会一直存在。 <a name="c6nqt"></a>

## 对比

|  | cookie | localStorage | sessionStorage |
| --- | --- | --- | --- |
| 数据保存的期限 | 一般由服务器生成，可设置保存时间。 | 需要手动清除，否则永久保存 | 仅在当前页面有效，关闭页面或浏览器后被清除 |
| 存放数据大小 | `4kb` 以内 | `5MB` 或更大 | `5MB` 或更大 |
| 与服务器关系 | 每次都会携带在 `http`头中,在浏览器和服务器之间传递 | 保存与客户端硬盘中，不参与和服务器的通信 | 保存在客户端内存中，不参与和服务器的通信 |

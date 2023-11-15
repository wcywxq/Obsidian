---
title: get 和 post 的区别
url: https://www.yuque.com/wcywxq/mxunh7/nk4fw8
---

get 和 post 是 http 请求的两种方法

1. 从应用场景上来说，get 请求是一个幂等的请求，一般 get 请求用于对服务器资源不会产生影响的场景，比如说请求一个网页。而 post 不是一个幂等的请求，一般用于对服务器资源产生影响的情景。比如注册用户这一类的操作。
2. 因为不同的应用场景，所以浏览器一般会对 Get 请求缓存，但很少对 Post 请求缓存
3. 从发送的报文格式来说，get 请求的报文中实体部分为空，post 请求的报文中实体部分一般为向服务器发送的数据
4. 但是 get 请求也可以将请求的参数放入 url 中向服务器发送，这样的做法相对于 post 请求来说，一个方面是不太安全，因为请求的 url 会被保留在历史记录中。并且浏览器由于对 url 上有一个长度上的限制，所以会影响 get 请求发送数据时的长度。这个限制是浏览器规定的，并不是 RFC 规定的。还有就是 post 的参数传递支持更多的数据类型。

get

- GET 请求可以被缓存
- GET 请求保留在浏览器历史记录中
- GET 请求可以添加书签
- 处理敏感数据时绝不应使用 GET 请求
- GET 请求有长度限制
- GET 请求仅用于请求数据（不修改）

post

- POST 请求永远不会被缓存
- POST 请求不会保留在浏览器历史记录中
- 不能为 POST 请求添加书签
- POST 请求对数据长度没有限制

| GET | POST |  |
| --- | --- | --- |
| BACK button/Reload | Harmless | Data will be re-submitted (the browser should alert the user that the data are about to be re-submitted) |
| Bookmarked | Can be bookmarked | Cannot be bookmarked |
| Cached | Can be cached | Not cached |
| Encoding type | application/x-www-form-urlencoded | application/x-www-form-urlencoded or multipart/form-data. Use multipart encoding for binary data |
| History | Parameters remain in browser history | Parameters are not saved in browser history |
| Restrictions on data length | Yes, when sending data, the GET method adds the data to the URL; and the length of a URL is limited (maximum URL length is 2048 characters) | No restrictions |
| Restrictions on data type | Only ASCII characters allowed | No restrictions. Binary data is also allowed |
| Security | GET is less secure compared to POST because data sent is part of the URL

Never use GET when sending passwords or other sensitive information! | POST is a little safer than GET because the parameters are not stored in browser history or in web server logs |
| Visibility | Data is visible to everyone in the URL | Data is not displayed in the URL |

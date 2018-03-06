## HTTP Status Codes
HTTP标准（主要是[RFC7231](https://tools.ietf.org/html/rfc7231#section-6)和[RFC6585](https://tools.ietf.org/html/rfc6585)）中提供了大约60个有特定语义的HTTP状态代码（参考[Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)或者https://httpstatuses.com/）。也有新的即将到来（例如：[draft legally-restricted-status](https://tools.ietf.org/html/draft-tbray-http-legally-restricted-status-05)）。还有非官方的，例如Nginx使用的。

- 不能创造（Invent）新的HTTP状态代码。只使用标准状态码并遵从其预期的语义（semantics）
- 应该使用最具体的HTTP状态代码
- 当使用不常用的HTTP状态代码，必须在API定义中提供良好的文档


### 最常使用的HTTP状态代码
- 成功代码（Success Codes）

|代码|含义|方法|
|-------|-------|------|
|200    |OK - 这是标准的成功响应    |所有|
|201    |Created - 成功创建实体。或者返回空响应，或者返回创建的资源并Location。 |POST，PUT |
|202    |Accepted - 请求成功，将异步处理  |POST，PUT，DELETE，PATCH  |
|204    |No content - 没有任何响应体   |PUT, DELETE, PATCH |
|207    |Multi-Status - 响应正文包含用于分批/批量请求的不同部分的多个状态信息。参考[批次或批量请求]() |POST |

- 重定向代码（Redirection Codes）

|代码|含义|方法|
|-------|-------|------|
|301    |Moved Permanently - 当前和未来所有请求应当重定向到给定URI  |所有 |
|302    |See Other - 可以使用GET方法在另一个URI下找到对请求的响应  |PATCH, POST, PUT, DELETE  |
|303    |Not Modified - 自通过请求标头If-Modified-Since或If-None-Match传递日期或版本后，资源尚未修改。  |GET  |

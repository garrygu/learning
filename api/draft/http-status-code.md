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

- 客户端错误代码（4xx - Client Side Error Codes）

|代码|名称|含义|方法|备注|
|-------|-------|------|------|------|
|400  |Bad Request  |一般/未知错误  |All  |可用400代码来标识所有校验错误 |
|401  |Unauthorized |用户无授权（通常是指Unauthenticated），需要登录。如果客户经过认证后依然不允许访问，则返回403（Forbidden） |All| |
|403  |Forbidden  |用户无权访问该资源  |All  | |
|404  |Not found  |资源未找到  |All  | |
|405  |Method Not Allowed |请求使用的HTTP方法不被允许  |All  |可返回一个包含有效方法的Allow头  |
|406  |Not Acceptable |不支持请求的媒体格式（由Accept标头所指定） |All  ||
|408  |Request timeout  |服务端超时等待资源  |All  ||
|409  |Conflict |由于冲突而无法完成请求。例如，当两个客户端尝试创建相同的资源或者存在并发的冲突更新时 |POST, PUT, DELETE, PATCH | |
|410  |Gone |资源不再存在，例如访问删除的资源时  |All  | |
|412  |Precondition Failed  |提供的If-Unmodified-Since或者If-Match和实际修改时间或ETag值不同。用于乐观锁定（optimistic locking） |PUT, DELETE, PATCH | |
|413  |Request Entity Too Large |请求的Body太大  |POST, PUT, PATCH  |在响应中指明允许的值，或者变通方法  |
|[415](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/415)  |Unsupported Media Type |服务端拒绝接受请求，因为不支持Payload格式 |POST, PUT, DELETE, PATCH |检查[Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type), [Content-Encoding](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding)或数据本身 |
|422  |Unprocessable  |请求数据格式正确但语义错误  |POST, PUT, DELETE, PATCH |Use 400 if the query is syntactically incorrect. Use 422 if the query is semantically incorrect.  |
|423  |Locked |悲观锁定 |PUT, DELETE, PATCH | |
|428  |Precondition Required  |服务器要求有条件的请求（例如，以确保避免“丢失更新问题”）。 |All  | |
|429  |Too many requests  |客户端不考虑速率限制并发送太多请求  |All |参考[...]() |


- 服务端错误代码（5xx - Server Side Error Codes）

|代码|名称|含义|方法|备注|
|-------|-------|------|------|------|
|500  |Internal Server Error  |服务器端一般错误 |All  |客户端重试可能是明智的  |
|501  |Not Implemented |服务器无法满足请求（通常意味着将来可用，例如新功能） |All  | |
|509	|Service Unavailable	|服务器暂时不可用。例如server出现故障或者过载  |All  |可能的话返回一个Retry-After头 |

Example:
```
http:localhost:8080/getDetails?user_id=12345
In this case if the details for the user_id 12345 exist then 200 OK status can be sent .
If the details for user_id 12345 are not present .Return a 404 NOT FOUND status
If you do not provide the user_id. That's syntactically wrong. You must return a 400 BAD REQUEST status
If you provide the user_id but it's not valid i.e a -43.67 . The query is syntactically correct but semantically incorrect. Throw a 422 Unprocessable Entity status. This must be used for validation purposes.
```

常用技术包括：

- gzip压缩
- 查询字段过滤器以检索资源属性的子集（请参阅应该：支持下面的资源字段的过滤）
- 数据项分页列表（请参阅下面的分页）
- ETag和If-(None-)Match头部以避免重新获取未改变的资源（请参阅5月：考虑将ETag与If-（无 - ）匹配头部一起使用
- 为更大（结果）列表的增量访问分页


# gzip压缩
除非请求太多以致压缩时间成为瓶颈，使用gzip压缩API响应的payload。这有助于通过网络更快地传输数据（更少的字节数），并使前端更快地响应。

尽管gzip压缩可能是服务器有效负载的默认选择，服务器还应该通过[Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4)请求标头支持无压缩有效负载和客户端控制。服务器应通过Content-Encoding标头指示使用的gzip压缩。

# 资源字段过滤 (Field selection)
可以通过支持对返回实体字段的过滤来显着降低网络带宽需求。在这里，客户端可以通过字段查询参数确定他想要接收的字段子集。 参见[Google AppEngine API’s partial response](https://cloud.google.com/appengine/docs/standard/python/taskqueue/rest/performance#partial-response)

未过滤：
```
GET http://api.example.org/resources/123 HTTP/1.1

HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "cddd5e44-dae0-11e5-8c01-63ed66ab2da5",
  "name": "John Doe",
  "address": "1600 Pennsylvania Avenue Northwest, Washington, DC, United States",
  "birthday": "1984-09-13",
  "partner": {
    "id": "1fb43648-dae1-11e5-aa01-1fbc3abb1cd0",
    "name": "Jane Doe",
    "address": "1600 Pennsylvania Avenue Northwest, Washington, DC, United States",
    "birthday": "1988-04-07"
  }
}
```

过滤：
```
GET http://api.example.org/resources/123?fields=(name,partner(name)) HTTP/1.1

HTTP/1.1 200 OK
Content-Type: application/json

{
  "name": "John Doe",
  "partner": {
    "name": "Jane Doe"
  }
}
```

# 允许可选嵌入子资源（资源扩展）
嵌入相关资源（也称为资源扩展）是减少请求数量的好方法。



# 缓存
## 支持客户端缓存
HTTP 1.1协议支持客户端和中间服务器的缓存，通过它使用Cache-Control头来路由请求。  

HTTP协议还为Cache-Control标头定义了no-cache指令。 相当混淆的是，这个指令并不意味着“不缓存”，而是“在返回之前用服务器重新验证缓存的信息”; 数据仍然可以被缓存，但每次使用时都会检查数据以确保它仍然是最新的。  
如果响应标头包含Cache-Control标头不存储，则不管HTTP状态码如何，都应该从缓存中删除对象。  


出于安全原因，请勿允许敏感数据或经过身份验证（HTTPS）连接返回的数据进行高速缓存。


HTTP协议定义了以下用于优化您在Web API中支持的GET请求的过程：

- 客户端构造一个GET请求，该请求包含用于在If-None-Match HTTP标头中引用的当前资源缓存版本的ETag：
```
GET http://adventure-works.com/orders/2 HTTP/1.1
If-None-Match: "2147483648"
```
- Web API中的GET操作获取所请求数据的当前ETag（上例中的次序为2），并将其与If-None-Match头中的值进行比较。
- 如果所请求数据的当前ETag与请求提供的ETag匹配，则资源没有发生变化，Web API应该返回一个空消息正文和状态码为304（未修改）的HTTP响应。
- 如果所请求数据的当前ETag与请求提供的ETag不匹配，则数据已更改，并且Web API应返回一个HTTP响应，其中包含消息正文中的新数据和状态码200（OK）。
- 如果请求的数据不再存在，则Web API应该返回状态码为404（未找到）的HTTP响应。
- 客户端使用状态码来维护缓存。 如果数据没有改变（状态码304），则对象可以保持高速缓存，并且客户端应用程序应该继续使用该版本的对象。 如果数据已经改变（状态码200），那么应该丢弃缓存的对象并插入新的。 如果数据不再可用（状态码404），则应从缓存中删除该对象。




# 参见Quey的分页操作


# 异步操作（Asynchronous operations）
参阅：  
https://www.adayinthelifeof.nl/2011/06/02/asynchronous-operations-in-rest/  


有时，POST，PUT，PATCH或DELETE操作可能需要处理一段时间才能完成。 如果您在向客户端发送响应之前等待完成，则可能会导致无法接受的延迟。 如果是这样，请考虑使操作异步。 返回HTTP状态码202（接受）以指示请求被接受处理但未完成。

您应该公开一个返回异步请求状态的端点，以便客户端可以通过轮询状态端点来监视状态。 将状态端点的URI包含在202响应的位置标题中。 例如：
```
HTTP/1.1 202 Accepted
Location: /api/status/12345
```

如果客户端向此端点发送GET请求，则响应应包含请求的当前状态。 可选地，它也可以包括估计完成时间或取消操作的链接。
```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "status":"In progress",
    "link": { "rel":"cancel", "method":"delete", "href":"/api/status/12345"
}
```

如果异步操作创建新资源，则操作完成后状态端点应返回状态码303（请参阅其他）。 在303响应中，包含一个Location头部，该头部提供新资源的URI：
```
HTTP/1.1 303 See Other
Location: /api/orders/12345
```

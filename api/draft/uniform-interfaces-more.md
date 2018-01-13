# 什么是统一接口(Uniform Interfaces)？
[HTTP协议](https://tools.ietf.org/html/rfc2616)定义了由下列标准方法组成的统一接口：
  - OPTIONS
  - GET
  - HEAD
  - POST
  - PUT
  - DELETE
  - TRACE
  - PATCH (new)

所谓统一接口，指这些方法的语法和含义不会随着应用或者资源而改变。REST可以利用HTTP content-types, caching, status codes, etc.

REST一般只使用有限的HTTP操作集合，包括HTTP `GET`, `PUT`, `DELETE`和`POST`.



# 安全与幂等
在处理HTTP方法时，[RFC 2616规范](https://www.ietf.org/rfc/rfc2616.txt)定义了两个重要概念：[`Safe`和`Idempotent(幂等)`](https://tools.ietf.org/html/rfc7231#section-4.2)。`Safe`指方法不能修改资源的状态；`Idempotent`指将一个请求发送多次和发送一次的结果是一样的。

|方法   |Safe     |Idempotent |描述|
|------ |:------: |:------:   |------|
|GET    |Y        |Y          |根据URL获取资源的一个呈现。<br>GET请求永远不应导致资源状态的改变。|
|HEAD   |Y        |Y           |返回Response头|
|OPTIONS|Y        |Y           |返回资源的可用方法|
|PUT    |N        |Y          |更新资源（如果指定资源不存在，在允许的条件下创建新资源）|
|DELETE |N        |Y          |删除资源|
|POST   |N        |N          |提交数据到服务器，对指定资源进行处理。用于创建新资源或者向已有资源添加数据。|
|PATCH  |N        |N          |资源局部更新|

## 争议
[Why PATCH method is not idempotent?](https://softwareengineering.stackexchange.com/questions/260818/why-patch-method-is-not-idempotent)  
> Whether PATCH can be idempotent or not depends strongly on how the required changes are communicated. For example, if the patch format is in the form of {change: ‘Name’ from: ‘benjamin franklin’ to: ‘john doe’}, then any PATCH request after the first one would have a different effect (a failure response) than the first request.  


# GET
请求：
请求消息中只能有头，不能有消息体。


# HEAD
除了没有内容（body），HEAD的返回和GET一样. 客户端可以用该方法来检查资源是否存在，或者获取资源的元数据（metadata）。

请求消息中只能有头，不能有消息体。


# POST
使用POST执行controller资源。

如果使用POST方法创建一个新资源，应该返回：
- 201，以及另一个超文本资源呈现（新资源的链接，以及下一步可以做什么）
- 204，以及包含新资源URI的`Location` header.



# DELETE
   A payload within a DELETE request message has no defined semantics;sending a payload body on a DELETE request might cause some existing implementations to reject the request.

   DELETE
   The problem with DELETE, which if successful would normally return a 200 (OK) or 204 (No Content), will often return a 404 (Not Found) on subsequent calls, unless the service is configured to "mark" resources for deletion without actually deleting them. However, when the service actually deletes the resource, the next call will not find the resource to delete it and return a 404. However, the state on the server is the same after each DELETE call, but the response is different.


   We've discussed using Expect and OPTIONS to guard against race conditions as much as possible. Besides these, we can also attach If-Unmodified-Since or If-Match headers to our PUT to convey our intentions to the receiving service. If-Unmodified-Since uses the timestamp and If-Match the ETag1 of the original order.   

   Checking OPTIONS and using the Expect header can't totally shield us from a situation where a change at the service causes subsequent requests to fail.

   General consensus is that OPTIONS metadata isn't that fine-grained in time.

如果资源彻底删除，GET和HEAD请求应返回404("Not Found")状态。
如果不是彻底删除（soft delete），不能使用DELETE（用 POST+controller代替），以符合HTTP语义？


# PUT
Q: PUT请求消息必须和GET请求得到的消息一致吗？
不需要。可以只包含资源状态的可变部分。

conditional PUT requests

# OPTIONS
获取资源的元数据：
`Allow: GET, PUT, DELETE`


# 自定义HTTP方法
除了上述的标准HTTP方法外，还有一些非标准方法，例如WebDAV提供的`PROPFIND, PROPPATCH, MOVE, LOCK, UNLOCK`等, `PATCH`, `MERGE`等。

**使用非标准HTTP方法可能存在的问题：**  
代理、缓存、HTTP库等通常会将非标准HTTP方法按照非幂等、不安全、不能缓存进行对待（和`POST`类似）。

**推荐做法：**   
尽量使用`POST`来代替非标准HTTP方法。

**HTTP头：Allow**  
通过HTTP [Allow](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.7)头可以知道资源支持哪些HTTP方法。例如：  
```
Allow: GET, PUT, DELETE
```

# 自定义HTTP Headers
比较著名的自定义HTTP头有： X-Powered-By, X-Cache, X-Pingback, X-Forwarded-For, X-HTTP-Method-Override等。
- X-HTTP-Method-Override  
最早用于[Google Data Protocol](https://developers.google.com/gdata/docs/2.0/basics). 如果防火墙不支持某个HTTP方法，例如PATCH, 可以用POST代替，并在`X-HTTP-Method-Override`指定应该使用的HTTP方法。 例如：
```
POST /myfeed/1/1/
X-HTTP-Method-Override: PATCH
Content-Type: application/xml
...
```

**推荐做法：**  
与其使用X-HTTP-Method-Override，不如利用一个单独的资源来POST同样的请求。因为任何客户和服务端之间的中间层都有可能忽略自定义头。


**推荐做法：**  
自定义HTTP Headers只用于提供信息目的（客户端可忽略）  
习惯于使用`X-`前缀命名自定义HTTP头。


# References
- [PUT vs PATCH vs JSON-PATCH](https://philsturgeon.uk/api/2016/05/03/put-vs-patch-vs-json-patch/)

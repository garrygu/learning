https://www.safaribooksonline.com/library/view/rest-api-design/9781449317904/ch04.html

# Cache-Control, Expires, and Date response headers should be used to encourage caching
# Cache-Control, Expires, and Pragma response headers may be used to discourage caching

Caching should be encouraged  
Expiration caching headers should be used with 200 (“OK”) responses  
Expiration caching headers may optionally be used with 3xx and 4xx responses  

如果API旨在支持缓存，则必须通过提供Cache-Control和Vary标题（请仔细阅读[RFC-7234](https://tools.ietf.org/html/rfc7234)），通过定义缓存边界（即生存时间和缓存约束）来指定此功能。

缓存必须考虑许多方面，例如响应信息的一般缓存能力，我们使用SSL和OAuth授权保护端点的指导原则，资源更新和失效规则，多个客户实例的存在. 因此，对于RESTful API，应避免客户端和透明HTTP缓存，除非API设计者已经证明知道更好。

默认情况下，API提供者应该设置Cache-Control: no-cache标题。


===============================

“缓存”通常用于指对象缓存（Object Cache）（例如memcached）或者HTTP缓存（例如Squid或者Traffic Server）。它们最大的不同是HTTP缓存不需要调用任何编程API来存储、访问或删除缓存中的对象。

HTTP过期缓存机制（Expiration Caching）可以有效减少对服务端的请求数量。服务只应该缓存成功的GET请求。  

过期缓存机制基于Cache-Control（HTTP 1.1）和Expires（HTTP 1.0）头。这些头信息告诉客户和缓存在一段特定的时间内保留服务端响应结果的一份拷贝。服务应在响应中添加这两个HTTP头，使得客户端可以决定是否可以缓存结果。例如：
```
First Request
GET /order/12345678 HTTP/1.1

First Response
HTTP/1.1 200 OK
Date: Sun, 09 Aug 2009 00:44:14 GMT
Last-Modified:  Sun, 09 Aug 2009 00:40:14 GMT
Expires: Sun, 09 Aug 2009 01:44:14 GMT
Cache-Control: max-age=3600,must-revalidate

Second Request (10分钟后)
GET /order/12345678 HTTP/1.1

Second Response
HTTP/1.1 200 OK
Date: Sun, 09 Aug 2009 00:54:14 GMT
Last-Modified:  Sun, 09 Aug 2009 00:40:14 GMT
Expires: Sun, 09 Aug 2009 01:44:14 GMT
Cache-Control: max-age=3600,must-revalidate
Age: 600 (由缓存添加，显示该拷贝存在于缓存的时间)
```

Cache-Control的主要参数有：

|属性 |	描述  |
|------ |------ |
|public	|默认值。即使请求是需要认证的，也可用之来使用共享缓存（Shared Cache）
|private	|客户端缓存（例如浏览器缓存或者正向代理（Forward Proxy））可以缓存结果，但共享缓存（例如服务端或者网络缓存）不能缓存结果。用于当响应只针对认证客户时。
|no-cache & no-store	|禁止缓存。同时包含Pragma: no-cache头可以支持HTTP 1.0
|max-age	|有效期（freshness lifetime）,以秒为单位
|must-revalidate	|告诉缓存返回过期结果前必须检查服务端数据（条件请求）
|proxy-revalidate	|和must-revalidate类似，但只用于共享缓存

此外，对可缓存的服务端响应，服务应该设置HTTP  Vary头。例如：Vary: Content-Type，表示基于URL+返回的content-type服务响应是可缓存的。  
REST recommends is that resources are meant to explicitly define their cacheability:  

HTTP Caching via Cache-Control or Expires centralizes the expiry time on the server, meaning clients all over the world will respect the decisions made by the server so long as they're coded to follow the HTTP response, instead of hardcoding stuff. Lots of middlewares support this, like the Ruby gem Faraday middleware Faraday HTTP Client.  

* Note that Expires isn't the only mechanism that will keep the order list up-to-date here; POST to the order list will invalidate any intervening caches (as per RFC2616 section 13.10).   

Due to the way GraphQL operates as a query language POSTing against a single endpoint, HTTP is demoted to the role of a dumb tunnel, making network caching tools like Varnish, Squid, Fastly, etc. entirely useless.

Computed Fields Suck

Tools like Varnish or Squid. An endpoint-based API can start off adding Etag and Cache-Control tags in the application itself.   
> Software like Memcache, Redis, etc. These tools can leverage HTTP headers like Etag, Vary, Cache-Control to handle cache validation

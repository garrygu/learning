REST可以利用HTTP content-types, caching, status codes, etc.

JSON API has been properly registered with the IANA. Its media type designation is application/vnd.api+json.




## JSON
对于二进制数据（ Binary Data）或替代内容表示（Alternative Content Representations）使用非JSON媒体类型：
- 传输结构不相关的二进制数据或数据。客户端不解析payload结构并直接消费就是这种情况。例如： 下载JPG, PNG, GIF类型的图片。


## 使用标准日期和时间格式
- JSON： 参见json指南。
- HTTP Headers  https://tools.ietf.org/html/rfc7231#section-7.1.1.1


## 为Number和Integer类型定义格式(format)
http://zalando.github.io/restful-api-guidelines/#171


## 公共数据类型
- 公共Money类型
在特定语言或计算时，不要把Money类型(amount, currency)转换成float / double类型，否则可能丢失精度。参见Stack Overflow [Why not use Double or Float to represent currency?
](https://stackoverflow.com/questions/3730019/why-not-use-double-or-float-to-represent-currency/3730040#3730040)

一些JSON解析器（例如NodeJS）默认将数字转成float。
使用decimal作为money格式。


# 通用字段名和语义
为了实现所有API实现的一致性，只要适用，就必须使用公共字段名称和语义。

## 通用字段

## 地址字段
required:
  - firstname
  - last_name
  - street
  - city
  - zip
  - country_code

## 使用Problem JSON
[RFC 7807](https://tools.ietf.org/html/rfc7807)定义了媒体类型application/problem+json
API可以定义具有扩展属性（extension properties）的自定义问题类型(custom problem types)


## Link & Link Relations


https://restpatterns.mindtouch.us/Articles/Resources_and_Representations
https://knpuniversity.com/screencast/rest/rest#play



6）使用Accept-Language头来指定和业务数据无关的一般语言偏好；使用查询参数languageCode来匹配实际业务数据中的语言类型。  
7）如果服务不支持请求的语言类型，将使用客户或BU（Business Unit）默认使用的语言类型。  
7. 内部客户总是使用Accept-Encoding: gzip （生产环境IIS的 gzip压缩是打开的）。



分页操作参数：
- String page_token: 客户端使用此字段来请求列表结果的特定页面。
- Int32 page_size：客户端使用此字段来指定服务器返回的最大结果数量。 服务器可以进一步限制在单个页面中返回的最大结果数量。 如果page_size为0 ，服务器将决定要返回的结果数量。  
- 响应消息中定义一个string字段next_page_token 。 该字段表示分页标记以检索结果的下一页。 如果值为"" ，则表示请求没有进一步的结果。

当客户端除了页面令牌外还指定查询参数时，如果查询参数与页面令牌不一致，则该服务必须使请求失败。


## 查询数据返回
- 服务需要对返回的记录总数进行限制（用户已指定最大返回记录数除外）。  
请求单个资源，如果资源不存在，返回404
请求集合资源，如果返回列表为空，返回200；如果列表缺少，返回404.
请求集合资源时，应提供足够的过滤（filter)机制以及分页（Pagination）。 响应消息中包含资源List的字段的名称必须是资源名称本身的复数形式。


Information about record limits and total available count should also be included in the response.
```
HTTP/1.1 200 OK
Vary: Accept
Content-Type: text/javascript

{
  "metadata": {
    "resultset": {
      "count": 227,
      "offset": 25,
      "limit": 25
    }
  },
  "results": [...]
}
```

请求和响应应该共享相同的Content-Type，除非请求是GET或具有“ application/x-www-form-urlencoded body”的POST 。




API enumerations  
API slice and map elements    

CORS  Cross Origin Resource Sharing  
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS  

A user agent makes a cross-origin HTTP request when it requests a resource from a different domain, protocol, or port than the one from which the current document originated.  

Resources are not storage items (or, at least, they aren’t always equivalent to some storage item on the back-end).   

The same resource state can be overlayed by multiple resources, just as an XML document can be represented as a sequence of bytes or a tree of individually addressable nodes.
Don’t confuse application state (the state of the user’s application of computing to a given task) with resource state (the state of the world as exposed by a given service). They are not the same thing.



更多阅读:  
 https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-implementation
- http://xfhnever.com/2015/02/27/rws-conditionalrequest/
- https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Conditional_requests

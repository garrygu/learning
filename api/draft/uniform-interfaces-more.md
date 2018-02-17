
# 标准方法

|Standard Method	|HTTP Mapping	|HTTP Request Body	|HTTP Response Body |
|------|------|------|------|
|List	|GET <collection URL>	    |N/A	|Resource* list
|Get	|GET <resource URL>	      |N/A	|Resource*
|Create	|POST <collection URL>	|Resource	|Resource*
|Update	|PUT or PATCH <resource URL>	|Resource	|Resource*
|Delete	|DELETE <resource URL>	|N/A	|Empty**

\*如果方法支持响应字段掩码（response field masks）（指定要返回的字段子集），则从List ， Get ， Create和Update方法返回的资源可能包含部分数据。
\**从不立即删除资源的Delete方法返回的响应（例如更新标志或创建长时间运行的删除操作） 应包含长时间运行的操作或修改后的资源。

对在单个API调用的时间跨度内未完成的请求，标准方法也可能会返回一个长时间运行的操作。

## 推荐返回格式
- GET
单个资源，返回的资源将映射到整个响应主体。
```
get /sales-order/{so#}
{
  _self: ...
  _next: ...
  sales_order: {
    ...
  }

}
```

- LIST
响应主体应该包含资源列表以及可选的元数据。
```
get /sales-order?dateFrom={x}&DateTo={x}
{
  _self: ...
  _next: ...
  sales_orders: [
    sales-order:{}
    sales-order:{}
  ]  
}
```

- 创建 (Create)
它在指定的父项下创建一个新资源，并返回新创建的资源。  
返回的资源将映射到整个响应主体。  

如果Create方法支持客户端分配的资源名称并且该资源已存在，则该请求应该失败并显示错误代码ALREADY_EXISTS  


- 更新（Update）
它更新指定的资源及其属性，并返回更新后的资源。  
任何重命名或移动资源的功能都不得在Update方法中发生，而应由自定义方法处理。  

- 标准Update方法应支持部分资源更新，并使用名为update_mask的FieldMask字段使用HTTP动词PATCH 。
- Update方法需要更高级的修补语义，例如添加到重复字段（repeated field）， 应该通过自定义方法提供 。
- 如果Update方法只支持完整资源更新，则必须使用HTTP动词PUT 。 但是，极度不鼓励完全更新，因为它在添加新资源字段时存在向后兼容性问题。  
- 响应消息必须是已更新的资源本身。

如果API接受客户端分配的资源名称，则服务器可能允许客户端指定不存在的资源名称并创建新资源。 否则， Update方法应该失败并显示不存在的资源名称。 如果它是唯一的错误条件，则应该使用错误代码NOT_FOUND 。

一个支持资源创建的Update方法的API也应该提供一个Create方法。


- 删除（Delete）
- API 不应该依赖任何由Delete方法返回的信息，因为它不能被重复调用。
- 没有请求主体; API配置不能声明body子句。
- 如果Delete方法立即删除资源，它应该返回一个空的响应。
- 如果Delete方法启动一个长时间运行的操作，它应该返回长时间运行的操作。
- 如果Delete方法只标记资源被删除，它应该返回更新的资源。

调用Delete方法应该是幂等的，但不需要产生相同的响应。 任何数量的Delete请求都应该导致（最终）删除资源，但只有第一个请求才会导致成功代码。 随后的请求应该会导致google.rpc.Code.NOT_FOUND 。


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

标准方法降低复杂性并增加一致性。


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

"Get with Body"
API标准中，在GET体必须在服务器端被忽略
请求消息中只能有头，不能有请求主体(request body)。



# POST
使用POST执行controller资源。

如果使用POST方法创建一个新资源，应该返回：
- 201，以及另一个超文本资源呈现（新资源的链接，以及下一步可以做什么）
- 204，以及包含新资源URI的`Location` header.

- 200 (if resources have been updated),
- 202 (if the request was accepted but has not been finished yet)

在不能使用其他方法的场景，使用POST。例如，复杂查询的GET.

和POST请求相关的资源ID是由服务端创建和维护的，并通过响应的Payload返回。执行同样的POST请求多次可能会造成多个资源实例。如果外部URI可用于识别重复的请求，尽量实现幂等（idempotent ）。


# PUT
语义：please put the enclosed representation at the resource mentioned by the URL, replacing any existing resource.

Q: PUT请求消息必须和GET请求得到的消息一致吗？
不需要。可以只包含资源状态的可变部分。

- PUT请求通常应用于单个而不是集合资源（意味着替换整个集合）
- 如果资源不存在，PUT隐含着“先创建再更新”
- 成功的PUT请求，服务端将使用有效载荷（payload）传递的资源表示(representation)替换URL定位的整个资源（后续读取将提供相同的有效载荷）。
- 成功的PUT请求通常会产生200或204（如果资源被更新 - 有或没有实际内容返回），和201（如果资源被创造）

与PUT请求相关的资源ID由客户端维护，并作为URL路径段传递。同样的资源PUT两次要求幂等，即导致同样的单一资源。使用PUT来创建资源时，只有URL允许作为资源ID。如果URI是不可用POST应该是首选。

conditional PUT requests  
为防止同步更新，应使用Etag和If-(None-)Match标头组合指示服务端暴露冲突，防止丢失更新。


# PATCH
PATCH通常用于单个资源（而不是集合资源）的局部更新（ partial update），即只有部分字段被更新。
- 通常返回200 or 204 (if resources have been updated）
- 返回或者不返回更新后的内容

除非为了满足向后兼容性，按顺序选择以下模式之一：
- 只要可行，使用PUT和完整对象更新资源；
- 使用PATCH和部分对象（partial objects）只更新资源的一部分（实际上是[JSON Merge Patch](https://tools.ietf.org/html/rfc7396)）,局部资源表示的特殊媒体类型application/merge-patch+json）
- 使用PATCH和[JSON Patch](https://tools.ietf.org/html/rfc6902)(特殊媒体类型，包括如何改变资源的指令)
- 使用POST，如果请求并不是按照媒体类型定义的方式来改变资源。

为防止同步更新，应使用Etag和If-Match标头组合指示服务端暴露冲突，防止丢失更新。

# DELETE
   A payload within a DELETE request message has no defined semantics;sending a payload body on a DELETE request might cause some existing implementations to reject the request.

   DELETE
   The problem with DELETE, which if successful would normally return a 200 (OK) or 204 (No Content), will often return a 404 (Not Found) on subsequent calls, unless the service is configured to "mark" resources for deletion without actually deleting them. However, when the service actually deletes the resource, the next call will not find the resource to delete it and return a 404. However, the state on the server is the same after each DELETE call, but the response is different.


   We've discussed using Expect and OPTIONS to guard against race conditions as much as possible. Besides these, we can also attach If-Unmodified-Since or If-Match headers to our PUT to convey our intentions to the receiving service. If-Unmodified-Since uses the timestamp and If-Match the ETag1 of the original order.   

   Checking OPTIONS and using the Expect header can't totally shield us from a situation where a change at the service causes subsequent requests to fail.

   General consensus is that OPTIONS metadata isn't that fine-grained in time.

DELTE通常应用于单个资源
成功删除，generate 200 (if the deleted resource is returned) or 204 (if no content is returned)
如果资源彻底删除，GET和HEAD请求应返回404("Not Found")状态。
如果不是彻底删除（soft delete），不能使用DELETE（用 POST+controller代替），以符合HTTP语义？
失败删除， usually generate 404 (if the resource cannot be found) or 410 (if the resource was already deleted before)



# HEAD
和GET具有同样的语义。除了没有内容（body），HEAD的返回和GET一样. 客户端可以用该方法来检查资源是否存在，或者获取资源的元数据（metadata）。

请求消息中只能有头，不能有消息体。



# OPTIONS
获取资源的元数据（用来检查给定端点的可用操作（HTTP方法））。可以被用来自我描述资源的全部功能。
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





# 为批量或批量请求（Batch or Bulk Requests）使用207
出于性能原因，例如通信和处理效率，一些API须使用POST提供批次或批量请求.在这种情况下，服务可能需要为批量或批量请求的每个部分发送多个响应代码。
- 批处理或批量请求必须始终使用HTTP状态代码207进行响应，除非在处理单个部件之前就遇到一般或意外故障。
- 带状态码207的批处理或批量响应始终返回一个多状态对象（multi-status object），其中为批处理或批量请求的每个部分包含足够的状态和/或监视信息。
- 批处理或批量请求可能会导致状态代码400/500，只有在处理各个部件之前服务就遇到故障或发生意外故障时。  

即使在处理所有部件失败或每个部件异步执行的情况下，以前的规则也适用。它们旨在允许客户以一致的方式检查单个结果

注：  
批处理(Batch)定义了触发独立流程的请求集合，但批量(bulk)定义了在一个请求中一起创建或更新的独立资源集合。但关于响应处理，这个区别通常无关紧要。

=================================  
允许客户同时创建、更新或删除多个资源。
在一个Request Body里包含多条记录，一次提交。

API Request methods  
providing a single request constructor for each API call  
This enables you to optionally modify and configure how the request will be sent. This includes, but isn’t limited to, modifications such as setting the Context per request, adding request handlers, and enabling logging.  

API enumerations  
API slice and map elements    

CORS  Cross Origin Resource Sharing  
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS  

A user agent makes a cross-origin HTTP request when it requests a resource from a different domain, protocol, or port than the one from which the current document originated.  

Resources are not storage items (or, at least, they aren’t always equivalent to some storage item on the back-end).   

The same resource state can be overlayed by multiple resources, just as an XML document can be represented as a sequence of bytes or a tree of individually addressable nodes.
Don’t confuse application state (the state of the user’s application of computing to a given task) with resource state (the state of the world as exposed by a given service). They are not the same thing.


# 并发控制
## 条件请求（CONDITIONAL REQUEST）
条件请求用于GET请求时，可以验证缓存是否过期；用于POST、PUT和DELETE时，可以提供并发控制。  
服务端用Last-Modified和ETag（Entity Tag）响应头来驱动条件请求。  
客户端：  
•	If-Modified-Since和If-None-Match用于校验缓存  
•	If-Unmodified-Since和If-Match作为并发控制的前提条件

服务端必须处理客户端提供的If-Match 和 If-None-Match头。客户端在使用If-Match  和If-None-Match头请求资源时需提供ETag值。ETag是由针对同一资源的前一次请求的结果提供的。服务端比较结果返回给用户最新内容，或者用 HTTP 响应码 304 告知用户，内容没有变化。  

Last-Modified的刻度较粗（以秒为单位），因此属于“弱验证”。ETag属于“强验证”，因为每个不同的资源呈现都会不同。不必要同时使用Last-Modified和ETag。  

如何生成Last-Modified和ETag标签？确保每次产生的ETag值都不重复。  
1）	利用数据库表结构的时间戳或者序列号字段  
2）	使用资源数据产生ETag值。将该值和时间戳保存在一个单独的表或数据源中  
3）	如果资源呈现的数据量不大，可以将其进行MD5哈希后得到ETag值  

http code 304


### Best Practices： REST APIs的乐观锁定(Optimistic Locking)
http://zalando.github.io/restful-api-guidelines/#optimistic-locking

- ETag和If-Match标头  
ETag只能通过在更新之前对单个实体资源执行GET请求来获得。
```
< GET /orders
> HTTP/1.1 200 OK
> [
>   { code: "O0000042"},
>   { code: "O0000043"}
> ]
< GET /orders/BO0000042
> HTTP/1.1 200 OK
> ETag: osjnfkjbnkq3jlnksjnvkjlsbf
> { code: "BO0000042", ... }
< PUT /orders/O0000042
< If-Match: osjnfkjbnkq3jlnksjnvkjlsbf
< { code: "O0000042", ... }
> HTTP/1.1 204 No Content
or if there was an update since the GET and the entity's etag has changed:
> HTTP/1.1 412 Precondition failed
```
缺点： 需要许多额外的请求来构建有意义的前端。

- 在结果实体中包含ETag  
每个实体的ETag都作为该实体的附加属性返回。在包含多个实体的响应中，每个实体都会有一个独特的ETag，可用于后续的PUT请求。
```
< GET /orders
> HTTP/1.1 200 OK
> [
>   { code: "O0000042", etag: "osjnfkjbnkq3jlnksjnvkjlsbf", ... },
>   { code: "O0000043", etag: "kjshdfknjqlöwjdsljdnfkjbkn", ... }
> ]
< PUT /orders/O0000042
< If-Match: osjnfkjbnkq3jlnksjnvkjlsbf
< { code: "O0000042", ... }
> HTTP/1.1 204 No Content
or if there was an update since the GET and the entity's etag has changed:
> HTTP/1.1 412 Precondition failed
```
优点： 完美的乐观锁定
缺点： 属于HTTP标头的信息成为业务对象的一部分

- 版本号  
实体包含具有版本号的属性。当发出PUT请求时，版本号必须包含在有效负载中. 并且服务器将检查数据库中的版本不高于当前请求的版本。
```
< GET /orders
> HTTP/1.1 200 OK
> [
>   { code: "O0000042", version: 1, ... },
>   { code: "O0000043", version: 42, ... }
> ]
< PUT /orders/O0000042
< { code: "O0000042", version: 1, ... }
> HTTP/1.1 204 No Content
or if there was an update since the GET and the version number in the database is higher than the one given in the request body:
> HTTP/1.1 409 Conflict
```
优点： 完美的乐观锁定
缺点： 属于HTTP标头的功能成为业务对象的一部分


- Last-Modified / If-Unmodified-Since  
在HTTP 1.0中没有ETag，用于乐观锁定的机制基于日期。这仍然是HTTP协议的一部分，可以使用。每个响应都包含带有HTTP日期的Last-Modified标头。当使用PUT请求请求更新时，客户端必须通过头部If-Unmodified-Since提供该值。如果实体的最后修改日期在标题中的给定日期之后，服务器会拒绝该请求。
```
< GET /orders
> HTTP/1.1 200 OK
> Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
> [
>   { code: "O0000042", ... },
>   { code: "O0000043", ... }
> ]
< PUT /block/O0000042
< If-Unmodified-Since: Wed, 22 Jul 2009 19:15:56 GMT
< { code: "O0000042", ... }
> HTTP/1.1 204 No Content
or if there was an update since the GET and the entities last modified is later than the given date:
> HTTP/1.1 412 Precondition failed
```
缺点： 如果客户端与两个不同的实例进行通信，并且它们的时钟不完全同步，则锁定可能会失败。

##	并发控制（CONCURRENCY CONTROL）
### 并发控制方式
并发控制有两种方式：  
1）	“悲观”并发控制：客户首先加锁资源，然后修改，然后解锁。期间服务会阻止其它客户获得资源锁。关系数据库采用这种方式。  
2）	“乐观” 并发控制：客户首先得到一个token，并尝试包含有token的写请求。如果token仍然有效，写入会成功。

HTTP采用的是乐观并发控制：  
1）	连同每个资源呈现，服务提供给客户一个token  
2）	当资源状态发生改变时，token的服务端版本发生改变  
3）	客户在请求修改或删除资源时，提供token给服务端，是为“条件请求”  
4）	服务验证token是否仍然有效。如果无效，并发操作失败，服务中断请求  

### 条件PUT/DELETE请求
服务端：  
如果资源不存在，返回404（Not Found）（或者在许可的情况下创建新资源）。  
如果客户没有提供If-Unmodified-Since和If –Match头，返回403（Forbidden）。  
如果客户提供的If-Unmodified-Since或If –Match值和服务端的实际修改日期和ETag值不符，返回412（Precondition Failed）。  
如果条件满足，更新（或删除）资源，返回200（OK）或者204（No Content）。  

客户端：  
如果收到412错误，利用无条件GET请求得到最新的Last-Modified和ETag值，然后重新提交请求。  

### 条件POST请求
条件POST请求用来发现和阻止重复提交。  
如果用POST创建新资源，会返回201或者303+新的URI。在这种情况下，客户端本地没有该资源呈现的拷贝和与条件请求相关的HTTP头信息。  
方法是通过为每个POST请求提供一个一次性的URI。URI里包含有一个服务端产生的token，只能用于一次POST请求。将所有使用过的token保存在服务端的事务日志里。例如：  
```
Request
GET /vendor/address/address-token HTTP/1.1

Response
HTTP/1.1 204 No Content
Cache-Control: no-cache
Link: <http://neweggcentral-rest/address;t=360eerfg8gg34j4d6ff8>;rel=”http://neweggcentral-rest/rel/resource”
```

如果客户提交的POST请求里含有已经使用过的token，返回403（Forbidden）。否则处理请求，返回201（Created）或者303（See Other），并标记该token为可使用。  
可以通过MD5当前时间+随机数+ETag等来产生用于一次性URI的token。可以在URI中包含数字签名来提高其安全性。  

### 关于缓存与并发的规定
1）	确定每种资源表示的缓存策略  
2）	在使用no-cache或者no-store之前，考虑使用教小数值的max-age替代  
3）	各子系统需要决定各资源的PUT操作在资源不存在时是否能创建新资源  
4）	针对不同情况，确定是否使用条件请求和并发控制  



# 使用429和标头来限制速率（Rate Limits）
希望管理客户端请求率的API必须使用'429 Too Many Requests'响应代码。这些响应还必须包含向客户端提供更多细节的标题信息。服务可以采用两种方法来获取标题信息：
- 返回'Retry-After'标头，表明客户在发出后续请求之前应该等待多长时间。Retry-After标题可以包含要重试的HTTP日期值或要延迟的秒数。API通常更喜欢使用基于秒数的延迟。
- 返回一个'X-RateLimit'标头。这些标头允许服务端以在给定的时间窗口内允许请求的数量以及什么时候时间窗口会被重置来表示服务级别（service level）  

'X-RateLimit'标头是：
- X-RateLimit-Limit ：允许客户端在此窗口中允许的最大请求数。
- X-RateLimit-Remaining ：当前窗口中允许的请求数量。
- X-RateLimit-Reset ：速率限制窗口将被重置时的相对时间（以秒为单位）。

重试（Retry-After） - 对于一般的负载处理和请求限制方案而言通常是足够的。，特别是，并不严格要求诸如租户或指定帐户的主叫实体的概念。  
 'X-RateLimit'标题适用于客户与预先存在的帐户或租赁结构关联的情况。 'X-RateLimit'头部通常在每个请求上返回，而不仅仅是在429上，这意味着实施API的服务具有足够的状态以跟踪在给定窗口内对每个命名实体进行的请求的数量。


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

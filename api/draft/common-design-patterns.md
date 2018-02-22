通用设计模式

# 空的回应（Empty Responses）


# 代表范围（Representing Ranges）
表示范围的字段应使用半开间隔（half-open intervals）和遵守命名约定[start_xxx, end_xxx)，如[start_key, end_key)或[start_time, end_time) 。 PI 应避免使用其他表示范围的方式，例如(index, count)或[first, last] 。


# 列表分页（List Pagination）

# List Sub-Collections
例如，图书馆API有一系列书架，每个书架都有一系列书籍，客户希望在所有书架上搜索书籍。  
建议在子集合上使用标准List ，并为父集合指定通配符集合标识"-" 。注意：选择"-"而不是"\*"是为了避免URL转义。  
GET https://library.googleapis.com/v1/shelves/-/books?filter=xxx


# Get Unique Resource From Sub-Collection
在图书馆API中，如果图书在所有书架上的所有图书中都是唯一的:  
GET https://library.googleapis.com/v1/shelves/-/books/{id}

响应此调用的资源名称必须使用资源的规范名称，并为每个父集合使用实际的父集合标识符而不是"-" 。 例如，上面的请求应该返回一个名称为shelves/shelf713/books/book8141 ，而不是shelves/-/books/book8141 。  

# 排序顺序（Sorting Order）
string order_by = ...;


# 请求验证（Request Validation）
如果一个API方法有副作用（side effects），并且有需要验证一个请求是否符合要求（而不引起这样的副作用），那么请求消息应该包含一个字段： bool validate_only = ...;

如果此字段设置为true ，则服务器不得执行任何副作用，只执行与完整请求一致的特定于实现的验证。

如果验证成功，则必须返回google.rpc.Code.OK并且任何使用相同请求消息的完整请求都不应返回google.rpc.Code.INVALID_ARGUMENT 。 请注意，该请求可能仍会因其他错误（例如google.rpc.Code.ALREADY_EXISTS或竞争条件而失败。


# 请求重复（Request Duplication）
对于网络API，幂等API方法是非常受欢迎的，因为它们在网络故障后可以安全地重试。 然而，一些API方法不容易是幂等的，例如创建资源，并且需要避免不必要的重复。 对于这种用例，请求消息应该包含一个唯一的ID，例如UUID，服务器将使用该ID来检测重复并确保请求只处理一次。
```
// A unique request ID for server to detect duplicated requests.
// This field **should** be named as `request_id`.
string request_id = ...;
```
如果检测到重复请求，则服务器应返回先前成功请求的响应，因为客户端很可能没有收到先前的响应。


# 部分响应(Partial Response)
有时，API客户端只需要响应消息中的特定数据子集。Google API平台通过响应字段掩码支持它。对于任何REST API调用，都有一个隐式系统查询参数$fields ，它是google.protobuf.FieldMask值的JSON表示。响应消息在被发送回客户端之前将被$fields过滤。 API平台针对所有API方法自动处理该逻辑。  

GET https://library.googleapis.com/v1/shelves?$fields=name

# 资源视图（Resource View）
为了减少网络流量，允许客户端限制服务器应在响应中返回资源的哪些部分是有用的，返回资源的视图而不是完整的资源表示。 API中的资源视图支持通过向方法请求添加一个参数来实现，该参数允许客户端指定它希望在响应中接收哪个资源视图。

参数：
    应该是enum类型
    必须命名为view

GET https://library.googleapis.com/v1/shelves/shelf1/books?view=BASIC

# ETags
ETag中允许的字符总结：
    仅可打印ASCII  
    RFC2732允许使用非ASCII字符，但对开发人员不友好  
    没有空格  
    除上述位置外，不得加双引号  
    避免RFC7232推荐的反斜杠（backslashes），以避免混淆escaping


# 仅用于输出字段（Output Fields）   
API可能需要区分客户端提供的字段作为输入和仅由服务器输出时返回的字段。对于仅输出的字段，应该维护字段属性文档。

但如果客户端在请求中设置了仅输出字段，或者客户端指定了带有仅输出字段的google.protobuf.FieldMask ，则服务器必须接受请求而不出错。 这意味着服务器必须忽略输出字段的存在及其任何指示。 这个建议的原因是因为客户端通常会重用服务器返回的资源作为另一个请求输入，如果对输出字段进行验证，则会在客户端上放置额外的工作以清除仅输出字段。


# Singleton Resources
当资源中只有单个资源实例存在时（或者在API中，如果没有父资源），则可以使用singleton资源。

单身资源必须省略标准的Create和Delete方法; 在创建或删除其父项时隐式创建或删除该单例（如果没有父项，则隐式地存在）。 必须使用标准的Get和Update方法访问资源。

例如，具有User资源的API可以将每个用户的设置作为Settings单例公开。


# Mock behaviour
在测试服务器上每个资源都接受一个mock参数。传递此参数应该返回一个模拟数据响应（绕过后端）。在开发早期实现此功能可确保API展现出一致的行为，支持测试驱动的开发方法。
```
GET /api/v1/magazines.json?mock=True HTTP/1.1
Host: example.gov.au
Accept: application/json, text/javascript
```

# Support cross-domain mashups with CORS
应该通过默认提供启用跨源资源共享（CORS）的服务来支持这种模式。




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

conditional PUT requests  
为防止同步更新，应使用Etag和If-(None-)Match标头组合指示服务端暴露冲突，防止丢失更新。

考虑实现可以批量更新集合中多个资源的批量HTTP PUT操作。 PUT请求应该指定集合的​​URI，并且请求主体应该指定要修改的资源的细节。 这种方法可以帮助减少恶意并提高性能。

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


### 使用ETags支持乐观并发（Optimistic Concurrency）
https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-implementation  
- 客户端构造一个PUT请求，该请求包含资源的新细节，并为资源的当前缓存版本提供一个由If-Match HTTP标头引用的ETag。
```
PUT http://adventure-works.com/orders/1 HTTP/1.1
If-Match: "2282343857"
Content-Type: application/x-www-form-urlencoded
Content-Length: ...
productID=3&quantity=5&orderValue=250
```
- Web API中的PUT操作获取所请求数据的当前ETag（上例中的顺序1），并将其与If-Match标头中的值进行比较。
- 如果请求数据的当前ETag与请求提供的ETag相匹配，则资源没有改变，Web API应该执行更新，如果成功则返回带有HTTP状态码204（无内容）的消息。 该响应可以包含用于资源更新版本的Cache-Control和ETag标头。 响应应始终包含引用新更新资源的URI的Location头。
- 如果所请求数据的当前ETag与请求提供的ETag不匹配，则数据已被其他用户更改，因为它已被提取，并且Web API应返回一个HTTP响应，其中包含一个空的消息主体和一个状态码为412（先决条件失败）。
- 如果要更新的资源不再存在，则Web API应返回状态码为404（未找到）的HTTP响应。
- 客户端使用状态码和响应头来维护缓存。 如果数据已更新（状态码204），则对象可保持高速缓存（只要Cache-Control标头未指定无存储）但应更新ETag。 如果其他用户更改了数据（状态码412）或未找到（状态码404），则应放弃缓存的对象。

If-Match头的使用完全是可选的，如果省略，Web API将总是尝试更新指定的顺序，可能会盲目覆盖其他用户所做的更新。 为避免因更新丢失而导致的问题，请始终提供一个If-Match标头。


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

Checking OPTIONS and using the Expect header can't totally shield us from a situation where a change at the service causes subsequent requests to fail.
General consensus is that OPTIONS metadata isn't that fine-grained in time.

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


对在单个API调用的时间跨度内未完成的请求，标准方法也可能会返回一个长时间运行的操作。

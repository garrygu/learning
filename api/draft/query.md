查询是HTTP GET方法最普遍的应用。针对极其复杂的查询也可使用POST方法。不过通常 POST 方法没有缓存机制，因此不是查询数据的首选。

# 查询方法（QUERY METHODS）
API设计者可以考虑以下两个选项：
- URL编码后的查询参数。但需尊重客户端、网关和服务器通常的尺寸限制。
- 当URL编码的查询参数的GET是不可能的，使用带消息体的POST。文档必须标准其蕴含的"GET WITH BODY"语义。
- 使用标头参数来传输长的结构化的请求信息不是一个选择。从概念的角度来看，操作的语义应该始终由资源名称和查询参数表示，即URL中的内容。请求标头保留用于通用上下文信息，例如FlowID。另外，查询参数和标题的大小限制是不可靠的，取决于客户端，网关，服务器和实际设置。因此，切换到标题并不能解决原始问题。


提供一组值作为查询参数有不同的方法。一个特定的类型应该在API定义中明确地选择和陈述。

只有csv或multi格式应该用于多值查询参数：

|收集格式|描述|Example|
|------|------|------|
|csv  |逗号分隔值  |?parameter=value1,value2,value3  |
|multi  |多个参数实例 |?parameter=value1&parameter=value2&parameter=value3  |


请求单个资源，如果资源不存在，返回404
请求集合资源，如果返回列表为空，返回200；如果列表缺少，返回404.
请求集合资源时，应提供足够的过滤（filter)机制以及分页（Pagination）


## 使用HTTP GET
GET是安全的（Safe）和幂等的（Idempotent），是获取资源的默认HTTP方法。不能用GET查询来修改数据。  

GET通过URI的Query String传递查询参数。查询参数包括过滤条件、排序字段、返回字段列表、返回记录数等。例如：  
例如，按Invoice日期排序，从第80张invoice开始，返回20张invoices：  
http://business.com/invoices?numInvoices=20&startIndex=80&sortOrder=invoiceDate

可以通过查询参数选择需要返回的字段。参考Facebook graph API.  
https://graph.facebook.com/bgolub?fields=id,name,picture  

查询参数的不同排列和组合会导致不同的URI。太多的URI会降低缓存的有效性。所以查询参数的排列顺序尽可能保持不变。  

服务要默认所有查询参数都是可以省略的，并在客户省略查询参数的情况下提供参数的默认值或默认行为（对于确实不能省略的查询参数，返回明确的错误信息）。服务需要忽略多余的参数。服务需要对返回的记录总数进行限制（用户已指定最大返回记录数除外）。  

查询参数最主要用来返回资源集合，不应用来提供资源ID和任何操作指示。将资源ID嵌入查询字符串的问题之一是系统不再能根据URL base和资源路径来判断是否同一资源。查询字符串不能有业务操作是因为它不应直接影响资源或者服务器的状态。例如：  
❶　GET	/order/123456			正确  
❷　GET	/order?orderNumber=123456	错误  
如果order 123456不存在，URL❶会返回４０４错误，URL❷会返回空集合。  


## 使用HTTP POST
如果查询条件太长使得URI超出浏览器或服务器对URI长度的限制，可以使用POST方法来执行查询。这时在请求Body中含有使用application/x-www-form-urlencoded编码的查询条件。例如：  
```
Request
POST /order HTTP/1.1
Host: neweggcentral-rest
Content-Type: application/x-www-form-urlencoded

zipcode=91765 & status=’V’

Response
HTTP/1.1 200 OK
Content-Type: application/xml; charset=UTF-8
…
```
使用POST方法来获取资源的坏处：
1）	削弱了REST的“统一接口”原则
2）	返回结果不能缓存
3）	因不能缓存，翻页操作会反复重新执行查询


# 保存查询（STORE QUERIES）
可以为每一次不同的查询创建一个新资源。这样通过资源可以访问和执行历史查询。  

通过POST方法执行的查询，可以首先创建一个关于该查询的新资源，然后利用GET 方法去访问该查询资源，这样POST查询就转成了GET查询。这样查询的结果是可缓存的。任何同样的 查询都可通过重定向到该资源的URI得以执行。而且，该查询还可以接收新的条件。  

资源创建成功后，返回代码201（Created）以及一个指向新资源的Location头。例如：  
```
Request #1
POST /order HTTP/1.1
Host: neweggcentral-rest
Content-Type: application/x-www-form-urlencoded

zipcode=91765 & status=’V’

Response
HTTP/1.1 201 Created
Content-Type: application/xml; charset=UTF-8
Location: http:// neweggcentral-rest/order/query/1

Request #2
GET /order/query/1?start=10 HTTP/1.1
Host: neweggcentral-rest
```


# 预定义查询
预定义普遍使用的查询，有助于服务端优化设计和提高响应速度。例如：返回order查询的summary视图：  
/order? OrderDateFrom=2012-05-21 & view=summary


# 标准查询参数
为了保持各子系统统一接口和风格，有必要约定参数命名。

- HTTP头相关  
小写的HTTP头名称。例如：accept;??   

- 业务对象相关  
一般和字段命名相同，尽量和业界规范保持一致，各子系统务必统一。例如：customerNumber, orderNumber, vendorNumber,  

- 日期、时间相关  
  - 起始/截止日期：dateFrom, dateTo
  - 起始/截止时间：timeFrom, timeTo  
例如：订单起始/截止日期：orderDateFrom，orderDateTo  

- 控制有关   
  - 选择字段：fields= {return fields}  
  - 排序：sort= asc/desc  （排序方向）（也有用+/-来表示排序方向的，例如： /sales-orders?sort=+id）
  -	sortFields (字段用逗号分隔)
  -	视图：view= summary/ meta/ status/ …
  -	预定义查询：query={pre-defined query}
  -	返回记录数：limit= 100
  -	第n页：pageIndex={n}   pageSize
  -	第n条记录：startIndex={n}
  -	方法：method= delete
  -	展开子集合expand= order/ order.items/ …
  -	返回格式：format

q- 默认查询参数
offset - 基于数字的开始页。和limit结合，可用page代替offset

# 关于查询的规定
1） 使用查询参数来指定过滤条件、排序字段或返回字段列表等  
2） 查询参数的排列顺序尽可能保持不变  
3） 如果客户没有设置查询参数，服务返回默认视图  
4） 为普遍使用的查询预定义命名查询  
5） 服务要假设所有查询参数都是可以省略的，并在客户省略查询参数的情况下提供默认视图。  
6） 服务需要忽略多余的参数。。  
7） 提供通过HTTP Link头提供翻页机制  
8） 对于隐藏内容，明确申明，不要让用户以为是服务端配置问题  
9） 如果字段较多，考虑支持fields参数  
10） 考虑支持标题扩展功能以支持返回子对象更详细的信息  
11） 文档化每一个查询参数  

# 其他

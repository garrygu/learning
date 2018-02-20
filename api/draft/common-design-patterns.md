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

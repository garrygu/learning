通用设计模式

# 空的回应（Empty Responses）


# 代表范围（Representing Ranges）
表示范围的字段应使用半开间隔（half-open intervals）和遵守命名约定[start_xxx, end_xxx)，如[start_key, end_key)或[start_time, end_time) 。 PI 应避免使用其他表示范围的方式，例如(index, count)或[first, last] 。


# List Sub-Collections
例如，图书馆API有一系列书架，每个书架都有一系列书籍，客户希望在所有书架上搜索书籍。  
建议在子集合上使用标准List ，并为父集合指定通配符集合标识"-" 。注意：选择"-"而不是"\*"是为了避免URL转义。  
GET https://library.googleapis.com/v1/shelves/-/books?filter=xxx


# Get Unique Resource From Sub-Collection
在图书馆API中，如果图书在所有书架上的所有图书中都是唯一的:  
GET https://library.googleapis.com/v1/shelves/-/books/{id}

响应此调用的资源名称必须使用资源的规范名称，并为每个父集合使用实际的父集合标识符而不是"-" 。 例如，上面的请求应该返回一个名称为shelves/shelf713/books/book8141 ，而不是shelves/-/books/book8141 。  



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

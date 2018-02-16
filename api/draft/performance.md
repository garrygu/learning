常用技术包括：

- gzip压缩
- 查询字段过滤器以检索资源属性的子集（请参阅应该：支持下面的资源字段的过滤）
- 数据项分页列表（请参阅下面的分页）
- ETag和If-(None-)Match头部以避免重新获取未改变的资源（请参阅5月：考虑将ETag与If-（无 - ）匹配头部一起使用
- 为更大（结果）列表的增量访问分页


# gzip压缩
除非请求太多以致压缩时间成为瓶颈，使用gzip压缩API响应的payload。这有助于通过网络更快地传输数据（更少的字节数），并使前端更快地响应。

尽管gzip压缩可能是服务器有效负载的默认选择，服务器还应该通过[Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4)请求标头支持无压缩有效负载和客户端控制。服务器应通过Content-Encoding标头指示使用的gzip压缩。

# 资源字段过滤
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

# 分页（Pagination）
如果列表含有几百个以上记录，应支持分页以获得最佳的客户端批处理和遍历体验。 两种页面遍历技术：
- 基于Offset/Limit的分页: 用数字偏移标识第一个页面入口
- 基于光标(Cursor)的分页： 基于Key的分页，一个唯一key标识第一个页面入口 （参见[Facebook’s guide](https://developers.facebook.com/docs/graph-api/using-graph-api/v2.4#paging))

分页的技术概念还应该考虑用户体验相关的问题。通常，相较于next/previous导航，跳转到特定页面的使用场景要少见得多。 （参见：[Infinite Scrolling, Pagination Or “Load More” Buttons? Usability Findings In eCommerce](https://www.smashingmagazine.com/2016/03/pagination-infinite-scrolling-load-more-buttons/))

应该优先选择基于光标的分页，避免基于偏移的分页：  
与基于偏移量的分页相比，基于光标的分页通常更好并且更高效。特别是当涉及到高数据量以及NoSQL数据库存储时。
- 如果您需要结果总数和/或反向迭代支持，则基于光标的导航可能无法正常工作
- 数据的可变性可能会导致结果页面出现异常
- 性能考虑因素 - 使用基于偏移量的分页的高效服务器端处理几乎不可行：
  - 更高的数据列表卷，尤其是如果它们不驻留在数据库的主内存中
  - 分片或NoSQL数据库


  进一步阅读：
  - [Twitter](https://developer.twitter.com/en/docs/tweets/timelines/overview)
  - [Use the Index, Luke](http://use-the-index-luke.com/no-offset)
  - [Paging in PostgreSQL](https://www.citusdata.com/blog/2016/03/30/five-ways-to-paginate/)


### 在适用的地方使用分页链接
实施[HATEOS]的API可能会使用简化的超文本控件在集合中进行分页。那些集合应该有一个items拥有当前页面项目的属性。当需要时，集合可能包含有关集合或当前页面的其他元数据（例如index，page_size）。

除非明确需要，否则应该避免在API中提供总计数。通常情况下，支持完整计数的系统和性能影响很大。

如果集合包含指向其他资源的链接，则集合名称应在适当时使用[IANA registered link relations](http://www.iana.org/assignments/link-relations/link-relations.xml)作为名称，但使用复数形式。

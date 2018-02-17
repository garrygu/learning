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
# 参见Quey的分页操作

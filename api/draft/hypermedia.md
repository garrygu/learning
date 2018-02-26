# 使用 [REST Maturity Level 2](https://martinfowler.com/articles/richardsonMaturityModel.html#level2)
它使我们能够构建充分利用HTTP动词和状态代码的面向资源的API。
- Must: Avoid Actions — Think About Resources
- Must: Keep URLs Verb-Free
- Must: Use HTTP Methods Correctly
- Must: Use Specific HTTP Status Codes

# Use [REST Maturity Level 3](https://martinfowler.com/articles/richardsonMaturityModel.html#level3) - HATEOAS

HATEOAS方法使客户能够从最初的起点导航和发现资源。 这通过使用包含URI的链接来实现; 当客户端发出HTTP GET请求以获取资源时，响应应该包含使客户端应用程序能够快速定位任何直接相关资源的URI。

HATEOAS具有额外的API复杂性.。其对于我们的SOA架构的价值是有争议的。因此不推荐但也不限制使用它。

深度阅读：
- [RESTistential Crisis over Hypermedia APIs](https://www.infoq.com/news/2014/03/rest-at-odds-with-web-apis)  
- [Why I Hate HATEOAS](https://jeffknupp.com/blog/2014/06/03/why-i-hate-hateoas/)

# 使用完整绝对URI
与其他资源的链接必须始终使用完整的绝对URI。 相对路径增加了客户端的复杂性。

# 超文本控件
将链接嵌入其他资源时，必须使用常用的超文本控件对象。它至少包含一个属性：  
  - href：超文本控件链接到的资源的URI。
例如：
```
{
  "id": "446f9876-e89b-12d3-a456-426655440000",
  "name": "Peter Mustermann",
  "spouse": {
    "href": "https://...",
    "since": "1996-12-19",
    "id": "123e4567-e89b-12d3-a456-426655440000",
    "name": "Linda Mustermann"
  }
}
```


# 为分页（Pagination ）和自我引用（Self-References）使用简化的超文本控件
超文本控制用于内部集合分页和自引用时，应该使用简单的URI值与它们相应的link relations（next，prev，first，last，self），而不是可扩展的普通超文本控件。
```
{
  "self": "https://.../articles/xyz/authors/",
  "index": 0,
  "page_size": 5,
  "items": [
    {
      "href": "https://...",
      "id": "123e4567-e89b-12d3-a456-426655440000",
      "name": "Kent Beck"
    },
    {
      "href": "https://...",
      "id": "987e2343-e89b-12d3-a456-426655440000",
      "name": "Mike Beedle"
    },
    ...
  ],
  "first": "https://...",
  "next": "https://...",
  "prev": "https://...",
  "last": "https://..."
}
```




Links to the next or previous page should be provided in the HTTP header link as well.  
```
Link: <https://blog.mwaysolutions.com/sample/api/v1/cars?offset=15&limit=5>; rel="next",
<https://blog.mwaysolutions.com/sample/api/v1/cars?offset=50&limit=3>; rel="last",
<https://blog.mwaysolutions.com/sample/api/v1/cars?offset=0&limit=5>; rel="first",
<https://blog.mwaysolutions.com/sample/api/v1/cars?offset=5&limit=5>; rel="prev",
```
为了协助客户端应用程序，返回分页数据的GET请求还应包含某种形式的元数据，用于指示集合中可用资源的总数。
X-Total-Count.

#### 在适用的地方使用分页链接
实施[HATEOS]的API可能会使用简化的超文本控件在集合中进行分页。那些集合应该有一个items拥有当前页面项目的属性。当需要时，集合可能包含有关集合或当前页面的其他元数据（例如index，page_size）。

如果集合包含指向其他资源的链接，则集合名称应在适当时使用[IANA registered link relations](http://www.iana.org/assignments/link-relations/link-relations.xml)作为名称，但使用复数形式。


http://bizcoder.com/api-design-notes-smart-paging



# 不要使用Link标头
不要将[Header defined by RFC 5988](https://tools.ietf.org/html/rfc5988#section-5)定义的Link标头和JSON媒体类型使用。直接在payload中嵌入链接。

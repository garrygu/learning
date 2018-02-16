# 使用 [REST Maturity Level 2](https://martinfowler.com/articles/richardsonMaturityModel.html#level2)
它使我们能够构建充分利用HTTP动词和状态代码的面向资源的API。
- Must: Avoid Actions — Think About Resources
- Must: Keep URLs Verb-Free
- Must: Use HTTP Methods Correctly
- Must: Use Specific HTTP Status Codes

# Use [REST Maturity Level 3](https://martinfowler.com/articles/richardsonMaturityModel.html#level3) - HATEOAS
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

# 不要使用Link标头
不要将[Header defined by RFC 5988](https://tools.ietf.org/html/rfc5988#section-5)定义的Link标头和JSON媒体类型使用。直接在payload中嵌入链接。

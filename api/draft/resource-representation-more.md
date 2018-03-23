REST API通常用文本格式来表述资源状态。最常用的就是XML和JSON。 我们倾向于使用JSON格式。JSON的缺点是不支持隐藏注释和包装长字符串。


Rule: Additional envelopes must not be created
A REST API must leverage the message “envelope” provided by HTTP. In other words, the body should contain a representation of the resource state, without any additional, transport-oriented wrappers.


# 资源呈现类型（资源的多重表述/数据格式）
需要支持的类型有：

|类型 |请求方式 |描述 |
|------|------|------|
|XML	|application/xml（资源扩展名.xml，参数名accept=xml）	|避免使用text/xml，因其默认字符集是us-ascii，而application/xml默认使用UTF-8.  |
|[JSON](https://tools.ietf.org/html/rfc7159)	|application/json（资源路径扩展名.json，参数名accept=json）	|||

如果不能同时支持两种类型，JSON格式优先。

JSON API has been properly registered with the IANA. Its media type designation is application/vnd.api+json.

They are 2 resources if 2 URI

## JSON
对于二进制数据（ Binary Data）或替代内容表示（Alternative Content Representations）使用非JSON媒体类型：
- 传输结构不相关的二进制数据或数据。客户端不解析payload结构并直接消费就是这种情况。例如： 下载JPG, PNG, GIF类型的图片。


## 使用标准日期和时间格式
- JSON： 参见json指南。
- HTTP Headers  https://tools.ietf.org/html/rfc7231#section-7.1.1.1


# 为Number和Integer类型定义格式(format)
http://zalando.github.io/restful-api-guidelines/#171


# 公共数据类型
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

## 使用Prolem JSON
[RFC 7807](https://tools.ietf.org/html/rfc7807)定义了媒体类型application/problem+json
API可以定义具有扩展属性（extension properties）的自定义问题类型(custom problem types)

## 不要暴露Stack Traces
Stack traces包含客户不需要的实施细节，或者泄露敏感信息，并可能向攻击者透露可能的系统漏洞

- 尽量避免在资源中包含计算字段。它可能造成大量资源消耗。

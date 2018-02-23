REST可以利用HTTP content-types, caching, status codes, etc.

JSON API has been properly registered with the IANA. Its media type designation is application/vnd.api+json.




## JSON
对于二进制数据（ Binary Data）或替代内容表示（Alternative Content Representations）使用非JSON媒体类型：
- 传输结构不相关的二进制数据或数据。客户端不解析payload结构并直接消费就是这种情况。例如： 下载JPG, PNG, GIF类型的图片。


## 使用标准日期和时间格式
- JSON： 参见json指南。
- HTTP Headers  https://tools.ietf.org/html/rfc7231#section-7.1.1.1


## 为Number和Integer类型定义格式(format)
http://zalando.github.io/restful-api-guidelines/#171


## 公共数据类型
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

## 使用Problem JSON
[RFC 7807](https://tools.ietf.org/html/rfc7807)定义了媒体类型application/problem+json
API可以定义具有扩展属性（extension properties）的自定义问题类型(custom problem types)


## Link & Link Relations


https://restpatterns.mindtouch.us/Articles/Resources_and_Representations
https://knpuniversity.com/screencast/rest/rest#play



6）使用Accept-Language头来指定和业务数据无关的一般语言偏好；使用查询参数languageCode来匹配实际业务数据中的语言类型。  
7）如果服务不支持请求的语言类型，将使用客户或BU（Business Unit）默认使用的语言类型。  
7. 内部客户总是使用Accept-Encoding: gzip （生产环境IIS的 gzip压缩是打开的）。 

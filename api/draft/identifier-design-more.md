 [RFC3986: Uniform Resource Identifier (URI): Generic Syntax](https://tools.ietf.org/html/rfc3986)  

REST APIs使用URIs(Uniform Resource Identifiers)来标识资源。 标识每个资源的URI必须具有唯一性。

URI可能很清晰地描述了一个资源模型，也可以是很难理解的一个字符串，例如`http://api.example.restapi.org/68dd0-a9d3-11e0-9f1c-0800200c9a66`. 客户端应该将URIs作为非透明标识符对待。但API设计者应该使URIs包含REST API的资源模模型信息。
> The only thing you can use an identifier for is to refer to an object. When you are not dereferencing, you should not look at the contents of the URI string to gain other information.  
--Tim Berners-Lee http://www.w3.org/DesignIssues/Axioms.html


一个API含有多个end points
一组服务含有多个API
一个Domain含有多组服务
一个Tenant有多个Domain


# 有效的URL
## URL长度  
HTTP协议规范 没有对URL长度进行限制。但特定的浏览器及服务器可能会对它有所限制。IE对URL长度的限制是2083字符 。其他浏览器（Firefox、Safari等）理论上对URL长度没有限制。  

服务端可能返回的错误： 414 (Request-URI Too Long)


## URL合法字符  
- URL必须符合W3 Uniform Resource Identifier语法规范。这意味着URL中的字符只能是ASCII字符集的一个特殊子集。

|集合	  |字符	  |URL中用途|
|------|------|------|
|字母数字|	a b c d e f g h i j k l m n o p q r s t u v w x y z A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 0 1 2 3 4 5 6 7 8 9	|文本, 协议 (http), 端口 (8080)等.|
|非保留字符|	- _ . ~|	文本|
|保留字符|! * ' ( ) ; : @ & = + $ , / ? % # [ ]|控制字符, 文本|
- URL中的特殊字符（例如汉字）和用于文本的保留字符（例如“?”）必须经过URL编码（URL Encoding）。
- 根据RFC3986，空格会被编码成 %20；但如果使用application/x-www-form-urlencoded媒体类型（HTTP form元素使用 ），空格会被编码成 +。服务端需要正确地识别编码方式。
- [RFC3986](https://tools.ietf.org/html/rfc3986)定义URL是大小写敏感的，但协议、域名和参数部分除外。
- 必须将任何用户的URL输入当作文本对待。

## [URI模板（Template）](https://tools.ietf.org/html/draft-gregorio-uritemplate-08)
URI路径中含有变量(variables),通常用`{}`表示。


# URI路径设计
`/`隔开的URI路径的每一个部分（节点）都应该是有意义的名称，代表一个独立的资源。这样做有助于用户理解资源模型的层次结构。

例如：

|Domain|Version|Collection ID|Resource ID | Collection ID |
|------|------|------|------|
|/sales-order |/v1  |/order |/{order#} | |
|/sales-order |/v1  |/customer |/{customer#} |/order |

资源名称：
  /sales-order/v1/order/{order#}  
  /sales-order/v1/customer/{customer#}/order

API生产者可以为资源和集合ID选择任何可接受的值，只要它们在资源层次结构中是唯一的。

完整资源名称  "//library.googleapis.com/shelves/shelf1/books/book2"
相对资源名称  "shelves/shelf1/books/book2"


## 资源名称与URL
尽管完整的资源名称与正常的URL类似，但它们并不是一回事。 一个资源可以通过不同的API版本，API协议或API网络端点公开。 完整的资源名称未指定此类信息，因此必须将其映射到特定的API版本和API协议以供实际使用。  

要通过REST API使用完整资源名称， 必须将其转换为REST URL，方法是在服务名称前添加HTTPS方案，在资源路径前添加API主版本，以及URL转义资源路径。 例如：  
```
// This is a calendar event resource name.
"//calendar.googleapis.com/users/john smith/events/123"

// This is the corresponding HTTP URL.
"https://calendar.googleapis.com/v3/users/john%20smith/events/123"
```

资源名称应该像普通文件路径一样处理，并且不支持％-encoding。


##  URI查询设计
查询参数也是资源唯一标识符的一部分。和URI的其他元素不同，查询部分对客户应该是透明的。
应使用HTTP头，而不是查询参数来确定Cache行为。

查询组件的作用：
- 过滤
- 分页



# Q&A
- Why: URL主干部分（不包括参数）全部小写  
除了协议（scheme）和域名（host）部分，[RFC 3986](https://tools.ietf.org/html/rfc3986)定义URI是大小写敏感的。


- Why：资源全部使用单数形式  
这样有助于简化用户对API的使用。用户不需要区分单复数形式。（虽然有很多文档建议集合资源使用复数形式）


- Why: URL的末尾不添加 / 字符   
虽然绝大多数Web程序会将下面两个URL看成是一样的。但是，URI里的所有字符组成了一个资源的唯一识别符。两个不同的URIs映射两个不同的资源。反之亦然。
```
/customer/{customer#}
/customer/{customer#}/
```

- Why: 使用连字符 `-` ，不使用下划线 `_`  
增加URI的可读性。

  一些文本程序会在URI下面显示下划线，表示该URI是可以点击的。这种情况下`_`就可能被遮住或者模糊不清。

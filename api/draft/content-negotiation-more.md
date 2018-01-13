REST框架下资源呈现和资源本身是有区别的。资源具体呈现出来的形式，叫做它的"表现形式"（representation）。各种表现形式由已定义的媒体类型、字符集和编码的字节流构成。一个资源可以没有，或者有一个，或者有多个表现形式。如果有多个表现形式存在，则称该资源是可协商的(negotiable)。  

# 代理驱动 VS. 服务驱动（AGENT-DRIVEN VS. SERVER-DRIVEN NEGOTIATION）
服务驱动的内容协商使用HTTP请求头来决定响应变体。服务驱动的局限：   
- 内容协商不包括货币单位、距离单位、日期格式等区域设置。  
- 有时候由于复杂的本地化需求，可能需要为不同的区域维护不同的资源。  
- Web浏览器普遍会为Accept头设置一个范围很广的媒体类型选项，使得浏览器难以通过内容协商机制来呈现资源。  

代理驱动 的内容协商是对每种响应变体（资源呈现）都使用不同的URI。虽然可以为所有的`Accept-*`头实施代理协商，但最常使用的是媒体类型和语言。代理协商普遍使用的方法包括：    

|方法|	示例|
|------|------|
|通过查询参数|	/order/status/12345678? format=json<br>/order/status/12345678? format=xml|
|通过子域名|	en.wikipedia.org （显示英语）<br>de.wikipedia.org（显示德语）|
|通过虚拟文件名扩展（virtual file extension）|在URI后面添加.xml或者.json等。不建议|

通过查询参数的形式来指定media type实际上是一种tunneling: 使用URI传递元数据而不是HTTP倾向的Accept头。

Agent-driven Negotiation  
https://tools.ietf.org/html/draft-ietf-httpbis-p3-payload-16#section-5.2


# 媒体类型（Media Types）
`Metia Types`最初叫`MIME(Multipurpose Internet Mail Extensions) Types`

HTTP头Content-Type接受的值必须是Media Types.

## Media Type语法
```
type "/" subtype *( ";" parameter )
```
- type: application, audio, image, message, model, multipart, text, or video  
REST API通常会用到`application`类型
- `subtype`的值和`type`有关
- parameter
  - required或optional，取决于该media type规范
  - 格式：attribute=value
  - 参数名称非大小写敏感（case-insentitive）
  - 参数值通常大小写敏感（case-sensitive），可以使用双引号`""`
  - 可以指定多个参数，顺序不重要

例如：  
```
Content-type: text/html; charset=ISO-8859-4
Content-type: text/plain; charset="us-ascii"
```

## 注册过的Media Types
[IANA（Internet Assigned Numbers Authority）](https://www.iana.org/assignments/media-types/media-types.xhtml)负责管理Metia Types注册，并提供到每种Metia Types规范(RFC)的链接。IANA 也允许任何人[提议](https://www.iana.org/form/media-types)新的Media Types.  

一些常用的Media Types:
- [text/plain](http://www.rfc-editor.org/rfc/rfc2046.txt)
- [text/html](http://www.rfc-editor.org/rfc/rfc2854.txt)
- [image/jpeg](http://www.rfc-editor.org/rfc/rfc2046.txt)
- [application/xml](http://www.rfc-editor.org/rfc/rfc3023.txt)
- [application/atom+xml](http://www.rfc-editor.org/rfc/rfc4287.txt)
- [application/javascript](http://www.rfc-editor.org/rfc/rfc4329.txt)
- [application/json](http://www.rfc-editor.org/rfc/rfc4627.txt)


## Vendor-Specific Media Types
在子类型中使用`vnd`前缀来标识一个媒体类型为公司拥有或控制。例如：
```
application/vnd.ms-excel
application/vnd.lotus-notes
text/vnd.sun.j2me.app-descriptor
```  

自定义媒体类型使得用户可以定义公司专有的数据显示格式。这时它就成为客户和服务之间的数据格式合约。自定义媒体媒体类型可以和特定资源相关。
例如：
```
GET /customers/1234 HTTP/1.1
Host: newegg.com
Accept: application/vnd.newegg.customer+xml
```
有些API使用自定义媒体类型来控制版本。

Vendor-Specific Media Types也可以通过IANA注册.


## Media Type设计
除了具有XML或者JSON等格式外，REST API请求或者返回的HTTP消息体通常也有特别的语义（semantics）。诸如Content-Type：application/json固然可以精准地描述消息体内容的语法（syntax），但是完全忽略了资源呈现的语义和结构。



# 用于内容协商的HTTP头
客户端可通过HTTP头`Accept-*`将它的偏好和处理能力传递给服务端，包括表现形式（representation）、语言偏好、字符集、对压缩的支持等。

|HTTP请求头（Accept-* ）	|描述|
|------|------|
|Accept|设置可接受的媒体格式。可指定多种媒体格式，媒体格式越具体优先级越高。另外，通过在每种媒体格式后添加q参数 来设置相对偏好。q=0表示不接受，q=1表示最大偏好，q参数缺省默认为q=1。|
|Accept-Charset|	设置可接受的字符集。例如：Accept-Charset: iso-8859-5, unicode-1-1;q=0.8|
|Accept-Encoding|	设置可接受的数据压缩方式。例如：Accept-Encoding: compress, gzip, deflate|
|Accept-Language|	设置可接受的语言。例如：Accept-Language: da,en-gb;q=0.8,en;q=0.7|

服务响应中相关的HTTP头包括：  

|HTTP响应头（Content-*）|	描述|	示例|
|------|------|------|
|Content-Type|	媒体类型(或称media-type, MIME type)，可以有charset等参数。例如：如果是application/xml或者以+xml结尾，则可以用XML解析器处理消息。JSON媒体类型application/json不指定charset参数，默认使用UTF-8 。|	application/xml;charset=UFT-8<br>application/json|
|Content-Length|	内容长度（bytes）|
|Content-Language|	本地化语言。使用两个字母的RFC5646语言标签。“-”+ 两个字母的国家代码是可选的。|	en-US|
|Content-MD5|	内容的MD5摘要，用于一致性检查（注：TCP用户使用传输级别的校验和（checksum）进行一致性检查）。接收方可以用这个头校验数据的完整性，特别是在不稳定的网络上发送或接受大量数据时。（由于可能被篡改，不能用于安全控制目的）|bbdc7bbb8ea589666e33ac922c0f83|
|Content-Encoding|	gzip, compress或deflate编码。如果是gzip，接收方在解析消息前必须先对消息进行解压。客户端可用Accept-Encoding来设置对Content-Encoding的偏好。避免在HTTP请求中使用这个头，除非服务端支持。|	gzip|
|Last-Modified|	服务端最近一次修改资源或呈现的时间	|Sun, 29 Mar 2009 04:51:38 GMT|

`Accept-*`通常指定的是一个范围 (Range) ； `Content-*`通常返回的是一个特定的值。
例如，如果客户：  
- 偏好French但English也能接受
- 只能处理application/atom+xml和application/xml媒体类型
- 知道怎么处理使用gzip压缩的资源呈现
```
Request
Accept: application/atom+xml;q=1.0, application/xml;q=0.6, */*;q=0.0
Accept-Language: fr;q=1.0, en;q=0.5
Accept-Encoding: gzip
###
Response
Content-Type: application/atom+xml; charset=UTF-8
Content-Language: fr
Content-Encoding: gzip
Vary: Accept-Encoding
```  
使用标准的HTTP头来使用/产生不同的MIME类型，在未来可以在不用改变服务接口的情况下很容易地支持新的内容类型。

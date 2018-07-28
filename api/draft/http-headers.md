

## 常见的HTTP标头
从概念的角度来看，操作的语义和意图应始终由URL路径和查询参数，方法和内容来表示。标题更常用于实现接近协议考虑的功能，如流量控制，内容协商和认证。因此，头部被保留用于通用上下文信息（[RFC-7231](https://tools.ietf.org/html/rfc7231#section-5)）。

HTTP headers不是大小写敏感的（not case-sensitive）  

- 正确使用内容标头（Content Headers）
内用标头是带有Content-的标头，用来描述消息体的内容。
- 使用标准化标头。
参见https://en.wikipedia.org/wiki/List_of_HTTP_header_fields  
- 使用Content-Location Header
- 使用[Location](https://tools.ietf.org/html/rfc7231#section-7.1.2)而不是[Content-Location](https://tools.ietf.org/html/rfc7231#section-3.1.4.2) Header  
由于Content-Location在语义和缓存方面的正确使用很困难，因此我们不鼓励使用Content-Location。在大多数情况下，通过使用Location来引导客户端访问资源位置就足够了，而不会触及Content-Location特定的歧义和复杂性。
- 使用Prefer来指示处理首选项  
[RFC7240](https://tools.ietf.org/html/rfc7240)定义的Prefer头允许客户端从服务器请求处理行为。RFC7240预先定义了一些偏好（preferences），并且是可扩展的，以允许定义其他偏好。
对Prefer的支持完全是可选的。但建议以Prefer取代为定义处理指令（processing directives）专有的"X-"头。
支持API可能会返回RFC7240中Preference-Applied定义的头，以指示是否应用了首选项。

- 考虑使用ETag和If-(None-)Match标头
参见并发控制部分。


### 自定义HTTP Headers
作为一般规则，应该避免专有的HTTP标头。但一个有用的场景是可用专有的标头提供上下文信息

- 自定义HTTP Headers应只用于提供信息目的（客户端可忽略），避免用于业务逻辑或流程控制
- 自定义头命名使用`X-`前缀  
- 预定义HTTP头:

X-标头最初是为非标准化参数保留的，但X-标头的使用已过时[RFC-6648](https://tools.ietf.org/html/rfc6648)。这些指导原则限制X-可以使用哪些标头以及如何使用它们。


# 自定义HTTP Headers
比较著名的自定义HTTP头有： X-Powered-By, X-Cache, X-Pingback, X-Forwarded-For, X-HTTP-Method-Override等。
- X-HTTP-Method-Override  
最早用于[Google Data Protocol](https://developers.google.com/gdata/docs/2.0/basics). 如果防火墙不支持某个HTTP方法，例如PATCH, 可以用POST代替，并在`X-HTTP-Method-Override`指定应该使用的HTTP方法。 例如：
```
POST /myfeed/1/1/
X-HTTP-Method-Override: PATCH
Content-Type: application/xml
...
```
Allow overriding HTTP method?
Some proxies support only POST and GET methods. Use the custom HTTP Header X-HTTP-Method-Override to override the POST Method.


**推荐做法：**  
与其使用X-HTTP-Method-Override，不如利用一个单独的资源来POST同样的请求。因为任何客户和服务端之间的中间层都有可能忽略自定义头。


**推荐做法：**  
自定义HTTP Headers只用于提供信息目的（客户端可忽略）  
习惯于使用`X-`前缀命名自定义HTTP头。

# HTTP指南（HTTP Guidelines）
## HTTP版本
某些API功能可能只能由较新版本的HTTP协议支持，例如服务器推送和优先级; 有些仅完全用HTTP / 2指定，如全双工流媒体。 如果您需要将这些功能中的任何一个作为API规范的一部分，请注意不同HTTP版本的限制。

通常我们推荐使用HTTP / 2来获得更好的性能以及对网络故障的恢复能力。

## 长请求URL
URL具有实际的长度限制，通常在2K到8K之间。如果您的API使用超过长度限制的URL的GET请求，则这些请求可能会被浏览器拒绝。 为了绕过这个限制，客户端代码应该使用带有Content-Type application/x-www-form-urlencoded和HTTP头X-HTTP-Method-Override: GET的POST请求。 这种方法也适用于DELETE请求。


#### Newegg专有头(Proprietary Headers)
- X-Flow-ID    
String.请求的流程ID，写入日志并传递给被调用的服务。有助于操作故障排除和日志分析。它应该是一个由可打印的ASCII字符组成的字符串（即没有空格）。
- X-Tenant-ID  
标识租户。必须根据从OAuth令牌提取的业务伙伴ID设置X-Tenant-ID。
- X-Sales-Channel
销售渠道由零售商所有，代表特定的消费者细分市场
- X-Frontend-Type
不同的应用程序类型，例如mobile app 或browser
- X-Device-Type  
smartphone, tablet, desktop, other  
- X-Device-OS  
iOS, Android, Windows, Linux, MacOS  
- X-App-Domain  

###

一个API含有多个end points
一组服务含有多个API
一个Domain含有多组服务
一个Tenant有多个Domain

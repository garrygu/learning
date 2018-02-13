# 业界常用的版本控制方法
- 使用HTTP ACCEPT头传递版本信息  
例如：
GET /account/1 HTTP/1.1  
#1： Accept: application/vnd.steveklabnik-v2+json  
#2： Accept: application/xml; version=1.0  

- 使用URI传递版本信息  
例如：  
http://api.newegg.com / order_mgmt /order/v1/order  
http://api.newegg.com /order_mgmt/claim/v1/claim

如果消费者还支持超媒体链接（hypermedia links）来驱动工作流程（HATEOAS），情况很快变得复杂。紧耦合和复杂的发布管理


- 关于版本控制的规定
•	使用URI来传递版本信息  
•	版本控制以Sub Domain为单位  
•	如果API存在多版本，新旧版本必须同时支持  
•	使用latest关键词提供指向最新版本的快捷方式  
•	对旧版本，只修复严重bug  
•	如果不支持传入的版本号，服务端应该返回代码415和提示信息  
•	需要新版本（打破现有的客户端）  
  o	移除或重命名URI  
  o	同一URI返回不同的数据  
  o	针对已有资源移除HTTP方法  
  o	从数据类型移除一个字段（客户端可能未处理值缺少的情况）  
  o	重新排列字段顺序或者添加一个新字段（依赖字段顺序的客户会受到影响。客户端应基于字段名字定位）  
  o	改变URL的结构等（返回给用户301- Moved Permanently，同时在Location头返回新的资源URI）（最好保留旧结构的URL）  
  o	重命名字段等（XML和JSON都是大小写敏感的）  
•	不需要新版本  
  o	新资源    
  o	新的资源呈现  
  o	针对已有资源的新的HTTP方法等  
•	服务的版本号和开发的版本号是不同的。可能多次发布，但服务的版本号不用改变  


# Compatibility
- 不能打破向后兼容性（Backward Compatibility）
更改API，但是保持所有消费者正常使用。专注于稳定性，避免无法提供额外价值的更改。API是服务提供商和消费者之间的合同，不能通过单方面的决定来破坏。  

有两种技术可以在不改变它们的情况下更改API：
- 遵循兼容扩展的规则
- 引入新的API版本并仍支持旧版本
推荐使用兼容扩展（compatible extensions）而阻止版本扩展


# API设计者应该应用以下规则以向后兼容的方式来演化（evolve）RESTful API：  
- 只添加可选的，从不必填的字段.
- 切勿改变字段的语义（例如，将客户号码的语义更改为客户ID，因为两者都是不同的唯一客户密钥）
- 输入字段可能含有通过服务端业务逻辑验证的（复杂的）约束。切勿将验证逻辑更改为更具限制性，并确保所有约束都在描述中明确定义。
- 枚举
  - 仅当服务器准备好接受并处理旧范围值时，才可以减少枚举范围作为输入参数。 用作输出参数时，可以减少枚举范围。
  - 当用于输出参数时，不能扩展枚举范围 - 客户端可能没有准备好处理它。 但是，用于输入参数时，枚举范围可以扩展。
  - 支持重定向以防万一URL改变（ 301永久移动 ）。


# 服务客户应该应用健壮性原则
- 对API请求和作为输入传输的数据保持保守（conservative）.
- 在处理和阅读API响应数据方面要有耐心
  - 在有效负载中容忍未知字段（另请参阅Fowler的“TolerantReader”帖子），即忽略新字段，但如果后续PUT请求需要，则不会从有效负载中消除它们。
  - 准备x-extensible-enum返回参数可以提供新的值; 要么是不可知的，要么为未知值提供默认行为。
  - 准备处理未在端点定义中明确指定的HTTP状态代码。 还要注意，该状态码是可扩展的。 默认处理是你如何处理相应的x00代码（参见RFC7231第6节 ）。
  - 当服务器返回HTTP状态301永久移动时，请遵循重定向。

# 保守地设计API
在从客户端接收什么样的信息方面API的设计者应保持保守和准确：  
- Payload中的未知输入字段或未知URL不应被忽略; 服务器应通过HTTP 400响应代码向客户端提供错误反馈。
- 准确定义输入数据约束（如格式，范围，长度等） - 检查约束并返回违规情况下的专用错误信息。
- 更具体和更具限制性（如果符合功能要求），例如通过定义字符串的长度范围。 它可以简化实现，同时作为兼容的扩展为进一步演化提供自由。

不忽略未知输入字段是与Postel定律的一种特定偏离和强烈的建议。服务器可能想要采取不同的处理方法，但应了解以下问题并明确支持的内容：
- 忽略未知输入字段实际上不是PUT的选项，因为它随后的GET响应变得不对称，并且HTTP明确了PUT“替换”语义和默认往返期望（参见RFC7231第4.3.4节 ）
- 服务器无法识别某些客户端错误，例如，在没有服务器错误反馈的情况下，属性名称打字错误将被忽略。 当客户端的实际意图是提供一个可选输入字段时，服务器不能区分客户端有意提供额外字段与发送错误命名字段的客户端
- 输入数据结构的未来扩展可能与已经被忽略的字段冲突，因此将不兼容，即破坏已经使用此字段但具有不同类型的客户端。

在特定情况下，如果不再需要（已知）输入字段，则只要服务器忽略此特定参数，它就可以保留在API定义中，并且可以使用“不再使用（not used anymore）”的描述，也可以从API定义中删除。  

API designers must document clearly how unexpected fields are handled for PUT, POST and PATCH requests.


# 必须：始终将JSON对象作为顶级数据结构返回以支持可扩展性
以支持将来的可扩展性。 JSON对象通过附加属性支持兼容的扩展。 这使您可以轻松扩展您的响应，例如稍后添加分页，而不会破坏向后兼容性。


# 使用开放指列表（Open-Ended List）而不是枚举
根据定义，枚举是封闭的值集合，被认为是完整的，而不是用于扩展。当枚举必须扩展时，枚举的封闭性原则会造成兼容性问题。

# 应尽量避免升级版本
当改变你的RESTful API中，以兼容的方式这样做，避免产生额外的API版本。

如果改变API不能兼容的方式来完成：
- 在旧资源以外创建另外一个新的资源（变体）
- 创建一个新的服务端点 - 即具有新的API一个新的应用程序（使用新的域名）
- 基于同一个微服务创建平行于旧的API新的API版本

强烈建议只是用前面两种方法。


# 使用媒体类型进行版本管理
基于媒体类型的版本是不太紧密耦合，因为它支持内容协商并因此减少发布管理的复杂性。  

在这里，版本信息和媒体类型经由HTTP Content-Type头一起设置， 例如：`application/x.zalando.cart+json;version=2`。对于不兼容的改变，为资源创建一个新版本的媒体类型。要生成新的表示版本，消费者和生产者可以使用HTTP Content-Type和Accept头进行内容协商。注意：此版本仅适用于请求和响应内容的架构，而不是URI或方法的语义。

Using header versioning should:

include versions in request and response headers to increase visibility  
include Content-Type in the Vary header to enable proxy caches to differ between versions  

提示：直到不兼容的更改是必要的，建议留在标准application/json的媒体类型。


# Deprecation



[Postel's Law (Robustness Principle)](http://davepacheco.github.io/se411/distrib/postels-law.html)  
be liberal in what you accept, and conservative in what you send  
[TolerantReader](https://martinfowler.com/bliki/TolerantReader.html)  
[The Robustness Principle Reconsidered](https://cacm.acm.org/magazines/2011/8/114933-the-robustness-principle-reconsidered/fulltext)

[Enterprise Integration Using REST](https://martinfowler.com/articles/enterpriseREST.html)

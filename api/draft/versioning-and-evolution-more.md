由于一个API服务可能提供多个API接口（API interfaces），因此API版本控制策略适用于API接口级别，而不是API服务级别。 为了方便起见，术语API在以下章节中指的是API接口。

MAJOR版本，当你做出不兼容的API更改时，
MINOR版本以向后兼容的方式添加功能时，

在guideline之前的保持原状
Publish后再修改需要检查兼容性

Use a simple ordinal number and avoid dot notation such as 2.5.

# 业界常用的版本控制方法
- 媒体类型版本控制(Media type versioning)  
使用HTTP ACCEPT头传递版本信息  
例如：
GET /account/1 HTTP/1.1  
#1： Accept: application/vnd.steveklabnik-v2+json  
#2： Accept: application/xml; version=1.0  


- 题版本控制 （Header versioning）
您可以实现一个自定义标题来指示资源的版本，而不是将版本号附加为查询字符串参数。这种方法要求客户端应用程序将适当的头添加到任何请求，尽管如果版本头被省略，处理客户端请求的代码可以使用默认值（版本1）。
```
GET http://adventure-works.com/customers/3 HTTP/1.1
Custom-Header: api-version=1
```
```
GET http://adventure-works.com/customers/3 HTTP/1.1
Custom-Header: api-version=2
```

- 使用URI传递版本信息（URI versioning）    
例如：  
http://api.newegg.com / order_mgmt /order/v1/order  
http://api.newegg.com /order_mgmt/claim/v1/claim

- 查询字符串版本控制 （Query string versioning）  
例如http://adventure-works.com/customers/3?version=2

这种方法具有语义优势，即始终从相同的URI中检索相同的资源，但它取决于处理请求以解析查询字符串并发送相应的HTTP响应的代码。

- 没有版本控制（No versioning）    
这是最简单的方法。 重大变化可以表示为新资源或新链接。


如果消费者还支持超媒体链接（hypermedia links）来驱动工作流程（HATEOAS），情况很快变得复杂。紧耦合和复杂的发布管理

当您选择版本控制策略时，您还应该考虑对性能的影响，特别是Web服务器上的缓存。URI版本控制和查询字符串版本控制方案对缓存友好，因为每次相同的URI /查询字符串组合都引用相同的数据。

- 关于版本控制的规定
Versions should be integers, not decimal numbers, prefixed with ‘v’.  

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


# Deprecation（弃用）
只要用户还在使用API，就不允许破坏性更新（breaking changes）。
关闭（API的或版本）的API之前，生产者必须确保，所有客户端已同意关闭端点。所有客户端都迁移后，生产者可以关闭过时的API。

API弃用是API定义的一部分。如果路径上的一个方法，一整条路径，甚至整个API端点（多个路径）需要被弃用，生产者必须将被弃用的每个方法/路径元素设置deprecated=true。如果deprecated设置为true，生产者必须在API定义中描述客户应该使用什么端口替代，以及什么时候该API将被关闭。

## 必须监控已过时的API使用
直到API被关闭之前，生产环境中API的所有者必须监控失效API的使用，以避免出现破坏性更新效果。

## 应该在响应中添加一个警告标头
在弃用阶段，生产者应该添加一个Warning标头（https://tools.ietf.org/html/rfc7234#section-5.5）。 添加Warning标题时，warn-code必须为299，warn-text应该为"The path/operation/parameter/…​ {name} is deprecated and will be removed by {date}. Please see {link} for details."。该link链接到一个文档描述为什么API不再支持，以及客户应该如何处理。

# https://cloud.google.com/apis/design/versioning
新的主要版本的API 不能依赖于以前的主要版本的相同的API 。 API 可以依赖于其他API，并且了解与这些API相关的依赖性和稳定性风险。 稳定的API版本只能依赖其他API的最新稳定版本。

在一段时间内，相同API的不同版本必须能够在单个客户端应用程序中同时工作。 这是为了帮助客户顺利从旧版本转换到新版本的API。

只有在弃用期结束后才能删除较早的API版本。

## 向后兼容性（Backwards compatibility）
向后兼容（不间断 non-breaking)）更改：  
- 将API接口添加到API服务  
从协议的角度来看，这总是安全的。 唯一需要注意的是客户端库可能已经在手写代码中使用了新的API接口名称。

- 将方法添加到API接口  
除非你添加一个与客户端库中已经生成的方法冲突的方法，否则应该没问题。

- 将HTTP绑定添加到方法  
假设绑定不会引入任何歧义，使服务器对之前拒绝的URL做出响应是安全的。

- 将字段添加到请求消息  
只要未指定字段的客户端在新版本中的处理方式与旧版本中的处理方式相同，添加请求字段就可以不是破坏性的（non-breaking）。不正确的做法最明显的例子是分页： 如果API的v1.0不包含对集合进行分页，则不能在v1.1中添加分页操作，除非默认的page_size被视为无限（通常一个坏主意）。 否则，期望从单个请求中获得完整结果的v1.0客户端可能会收到截断结果，而不知道该集合包含更多资源。

- 将字段添加到响应消息  
一个不是资源的响应消息可以在不中断客户端的情况下进行扩展，只要这不会改变其他响应字段的行为。 任何以前填充到响应中的字段都应该以相同的语义继续填充，即使这会引入冗余（redundancy）。

- 将值添加到枚举  
仅在请求消息中使用的枚举可以自由扩展以包含新元素。 例如，使用资源视图模式，可以在新的次要版本中添加新视图。 客户永远不需要收到这个枚举，所以他们不必知道他们不关心的值。  
对于资源消息和响应消息，默认的假设是客户端应该处理它们不知道的枚举值。  

- 添加仅输出(output-only)资源字段  
服务器可以验证请求中的任何客户端提供的值是否有效，但如果省略该值，则不能返回验证失败。


向后不兼容（中断 breaking）的变化：
- 删除或重命名服务，接口，字段，方法或枚举值  
基本上，如果客户端代码可以引用某些内容，那么删除或重命名它就是一个重大改变，

- 更改HTTP绑定
这里的“更改”实际上是“删除并添加”。例如，如果您决定确实想要支持PATCH，但是您的发布版本支持PUT，或者您使用了错误的自定义动词名称，则可以添加新的绑定，但不能删除旧的绑定。

- 更改字段的类型
- 更改资源名称格式  
资源不得更改其名称 - 这意味着集合名称不能更改。

- 更改现有请求的可见行为（visible behavior）
- 在HTTP定义中更改URL格式  
除了上面列出的资源名称更改之外，资源参数名称：从v1/shelves/{shelf}/books/{book}到v1/shelves/{shelf_id}/books/{book_id}不会影响替换的资源名称，但可能会影响代码生成。

- 将读/写字段添加到资源消息  
客户通常会执行读取/修改/写入操作。 大多数客户不会提供他们不知道的字段的值。您可以指定消息类型（而不是基本类型）的任何缺失字段意味着更新不会应用于这些字段，但是这使得从实体中明确删除这样的字段值变得更加困难。

原始类型（包括string和bytes ）不能用这种方式处理，因为proto3在明确指定int32字段为0和没有指定它之间没有区别。

如果所有更新都使用字段掩码执行，则这不是问题，因为客户端不会隐式覆盖它不知道的字段。 但是，这将是一个不寻常的API决策：大多数API允许“全部资源”更新。


并不总是很清楚什么是一个突破（不兼容）的变化。这里的指导应该被视为指示性的而不是每一个可能的变化的全面清单。



[Postel's Law (Robustness Principle)](http://davepacheco.github.io/se411/distrib/postels-law.html)  
be liberal in what you accept, and conservative in what you send  
[TolerantReader](https://martinfowler.com/bliki/TolerantReader.html)  
[The Robustness Principle Reconsidered](https://cacm.acm.org/magazines/2011/8/114933-the-robustness-principle-reconsidered/fulltext)

[Enterprise Integration Using REST](https://martinfowler.com/articles/enterpriseREST.html)

精心设计的网络API应该旨在支持：
- 平台独立性（Platform independence）  
无论API如何在内部实现，任何客户端都应该能够调用API。

- 服务演化（Service evolution）
Web API应该能够独立于客户端应用程序发展和添加功能。 随着API的发展，现有的客户端应用程序应该继续运行而不需要修改。 所有的功能应该是可发现的，以便客户端应用程序可以充分利用它。


OpenAPI promotes a contract-first approach, rather than an implementation-first approach. Contract-first means you design the API contract (the interface) first and then write code that implements the contract.


# The Job of the API Designer

## Define requirements
- Use [Test-driven development](http://wiki.c2.com/?TestDrivenDevelopment): Specify the expected responses and write a test checking them.
- Have a look how competitors define similar API’s and don’t reinvent the wheel (Principle of Least Astonishment).https://en.wikipedia.org/wiki/Principle_of_least_astonishment





## 设计流程  
https://cloud.google.com/apis/design/resources  
建议在设计面向资源的API时采取以下步骤:  
- 确定API提供的资源。
- 确定资源之间的关系。
- 根据类型和关系决定资源名称方案 (resource name schemes)。
- 决定资源模式 (resource schemas)。
- 为资源添加最少的方法。


API Owner
breaking Changes
Resource & Representation


要开发良好的基于HTTP的服务，需理解REST架构风格背后的理念。
一致性不仅对外部客户而且对内部服务消费者也很有价值，而且这些准则提供了对任何服务都有用的最佳实践。

好的API看起来像是一个team开发的

原则：
- API as a product
- API Design Principles

# General guidelines
- Follow API First Principle
- Provide API Specification using OpenAPI
- design based on general requirements

使用RAML定义API；使用Developer portal configure &manage API
owner, 共享，边界
不能是表的翻译


这些准则的豁免有合理的理由。 很明显，实现或必须与某些外部定义的REST API进行互操作的REST服务必须与该API兼容，而不一定是这些指南。 某些服务也可能有特殊的性能需求，需要不同的格式，如二进制协议。

# 错误
错误或者更具体的服务错误被定义为客户机将无效数据传递给服务并且服务正确地拒绝该数据。 示例包括无效的凭据，不正确的参数，未知的版本ID或类似信息。 这些通常是“4xx”HTTP错误代码，并且是客户端传递错误或无效数据的结果。

错误不会影响整个API的可用性。

# 故障
故障或者更具体的服务故障被定义为服务无法正确返回以响应有效的客户端请求。 这些通常是“5xx”HTTP错误代码。

故障确实有助于整体API的可用性。

由于速率限制或配额故障而失败的呼叫绝不能算作故障。 由于服务快速失败请求而失败的调用（通常是为了自己的保护）会被视为错误。

# 延迟
延迟定义为特定API调用需要完成多长时间，尽可能接近客户端进行度量。 这个指标同样适用于同步API和异步API。 对于长时间运行的调用，延迟是在初始请求时测量的，并测量该调用（而非整体操作）需要多长时间才能完成。



# 客户端指南


- 基于HTTP统一接口的CRUD等操作
- HATEOAS：让一个设计良好的客户端可以像人运行互联网一样运行API

=======================
什么是API？
- 与service不同
- 多个端口
- 合约


- HTTP标头字段首选Hyphennated-Pascal-Case。例如：    
```
Accept-Encoding
Apply-To-Redirect-Ref
Disposition-Notification-Options
Original-Message-ID
```
参阅[HTTP Headers are case-insensitive (RFC 7230).](https://tools.ietf.org/html/rfc7230#page-22)

2)	模块化。Web服务的资源和方法可能增长很快。使用Domain和sub domain对资源进行分组或分隔。只使用规定的domain和sub domain名字。  

1. 在设计阶段就要规划好URI结构：{host: port}/{ domain}/{version#}/{sub domain}/{resource}  

CORS

# REST简介
传统的RPC API演化的最终结果可能就是存在大量的接口和方法，每个都与其他方法不同。开发人员必须仔细地学习每一个，才能正确使用它。这既耗时又容易出错。

Roy Fielding于2000年首次提出了[REST](https://en.wikipedia.org/wiki/Representational_state_transfer)作为设计Web服务的架构风格。其核心原则是定义可使用少量方法操作的命名资源(resource names)。资源和方法被称为API的名词和动词。

REST独立于任何底层协议，不一定与HTTP绑定。但是，大多数常见的REST实现使用HTTP作为应用程序协议。基于HTTP的REST的主要优点是它使用开放标准，并且针对API实现或客户端应用程序，它不绑定任何特定的实现。使用HTTP协议，资源名称自然映射到URL，方法自然映射到HTTP方法。

以下是使用HTTP的RESTful API的一些主要设计原则：  
（参阅https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design）   
- REST API是围绕`资源`设计的，这些资源是客户端可以访问的任何类型的对象，数据或服务。
- 资源具有`标识符`（identifier），该标识符就是唯一标识该资源的URI。 例如：  
```
# 特定销售订单
https://apis.newegg.org/so/v1/orders/12345678
# 某个客人的所有订单
https://apis.newegg.org/so/v1/customers/167498/orders
```
- 客户端通过交换`资源表示`（representations of resources）来与服务端交互。
- REST API使用`统一接口`（uniform interfaces），这有助于分离客户端和服务端的实现。   
对于构建于HTTP上的REST API，统一接口包括使用标准HTTP动词对资源执行操作。最常见的操作是GET，POST，PUT，PATCH和DELETE。
- REST API使用`无状态`请求模型（stateless request model）。  
HTTP请求是独立的，并且可以以任何顺序发生，因此在请求之间保持状态信息是不可行的。唯一存储信息的地方是资源本身。这个约束使Web服务具有高度的可伸缩性，因为不需要保留客户端和特定服务器之间的任何关联。任何服务器都可以处理任何客户端的请求。
- REST API由资源表示（representation）中包含的`超媒体链接`驱动，如下例的`links`节点所示：
```
{
    "orderID":3,
    "productID":2,
    "quantity":4,
    "orderValue":16.60,
    "links":
        {"rel":"product","href":"http://adventure-works.com/customers/3", "action":"GET" },
        {"rel":"product","href":"http://adventure-works.com/customers/3", "action":"PUT" }
    ]
}
```
这实际上是REST成熟度模型3级（HATEOAS）的要求。


# REST成熟度模型
2008年，Leonard Richardson为Web API提出了以下成熟度模型：  
- 级别0：定义一个URI，所有的操作都是对这个URI的POST请求。
- 级别1：为单个资源创建单独的URI。
- 级别2：使用HTTP方法来定义资源上的操作。
- 等级3：使用超媒体（HATEOAS）。

根据Fielding的定义，级别3对应一个真正的RESTful API。实际上，许多已发布的Web API都在第2级左右。

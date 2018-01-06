资源(Resource)是REST架构的基础，是系统中所有可用URI来定位的具体或抽象实体。所有的操作都是面向资源而不是对象或活动。设计REST API的第一步是设计资源模型。这个过程类似于对一个关系数据库系统的数据建模，或对一个面向对象系统的对象建模。

资源包括：
- 类型
- 关联数据
- 与其他资源的关系
- 一组操作该资源的方法


# 识别资源
资源模型识别和归类所有客户需要和服务进行交互的资源。目的是找出值得识别、呈现和操作的资源。

- 个体资源  
和集合资源相对而言。例如单一一个Customer，一张Sales Order，客户的一个Shipping Address等。每个个体资源都有唯一的ID。

- 集合资源(Collections)  
集合本身也是资源，包含0到多个个体资源。例如所有Customers，一个客户的所有Sales Orders等。通常将集合作为一个工厂，通过向集合提交一个HTTP POST请求来创建一个新成员。  
The concept of a “compound document” — in the API world at least — is the art of squashing related data into the main requested resources. This is done to reduce the number of HTTP calls a client has to make.

- 复合资源(Composites)  
将关联数据引入主请求资源的艺术，用于减少客户端HTTP请求次数。
由2种以上的资源构成。例如Customer资源可以包括Billing Address资源和Shipping Address资源。复合资源使得它们的呈现和别的资源有重叠，通常只用于查询。

- 抽象资源  
某些操作无法映射到普通的HTTP方法上，例如计算运费，校验信用卡等。这时将对应的处理函数抽象成一个资源，例如ShippingCalculator，CreditCardValidator。资源的呈现就是计算的结果。
因为这类方法是安全和幂等的，GET方法最适合的。

- Controller资源（事务）  
为资源设计Controller可以将更新多个资源的操作作为原子操作处理，或者能够使客户触发一系列复杂的业务操作。例如：Void SO操作，假设需要1）更新Order资源，2）发送Email给客户，与其分别更新/order/{order#}资源和添加/order/{order#}/email资源，可以设计一个controller资源/order/cancellation/{order#}来处理Void SO 操作。


尽量避免在资源中包含计算字段。它可能造成大量资源消耗。

## 资源粒度
从客户和网络的角度来确定合适的资源粒度(granularity)。其他因素：
- 缓存(Cacheability)
- 变化频率(Frequency of change)
- 可变性(Mutability)


例如：客户和地址。是把地址数据作为客户资源的一部分，还是把客户和地址作为两个不同的资源呢？如果我们要把客户缓存到gateway上面，包含address数据可能就太大了。

基于用户的使用模式而不是数据库或对象模型来设计资源。

# 关于Tunneling
Tunneling: 用同一URI，使用同样的方法，来完成不同的事情。例如：
```
POST /book/1234
op=updateDiscount&discount=15

POST /book/1234
op=30dayOffer&ebook_from=...
```
Tunneling削弱了协议级别（protocol-level）的可见性(visibility)，因为请求的可见部分如URI，HTTP方法，HTTP头和媒体类型(media types)无法无歧义地描述一个操作。

宁可使用一个单独资源(例如controller)，也要尽量避免Tunneling。

面向资源的设计（Resource Oriented Design）
避免Action, 想想资源。

资源(Resource)是REST架构的基础，是系统中所有可用URI来定位的具体或抽象实体。所有的操作都是面向资源而不是对象或活动。

设计REST API的第一步是设计资源模型。这个过程类似于对一个关系数据库系统的数据建模，或对一个面向对象系统的对象建模。

资源包括：
- 类型
- 关联数据
- 与其他资源的关系
- 一组操作该资源的方法


# 识别资源
面向资源的API通常建模为资源层次结构（resource hierarchy），其中每个节点都是简单资源或集合资源 。通常分别被称为资源和集合。

资源是命名实体（named entities），资源名称（resource names）是它们的标识符（identifiers）。每个资源必须具有自己唯一的资源名称。资源名称由资源本身的ID，任何父资源的ID以及其API服务名称组成。

集合是一种特殊的资源，其中包含相同类型的子资源列表。例如，目录是文件资源的集合。 集合的资源ID称为集合ID。

资源名称使用由正斜杠（'/'）分隔的集合ID和资源ID进行分层次组织。 如果资源包含子资源，则通过指定父资源名称和子资源的ID来形成子资源的名称 - 再次用正斜杠分隔。

应尽可能使用URL友好的资源ID

资源模型识别和归类所有客户需要和服务进行交互的资源。目的是找出值得识别、呈现和操作的资源。

- 个体资源  
和集合资源相对而言。例如单一一个Customer，一张Sales Order，客户的一个Shipping Address等。每个个体资源都有唯一的ID。

资源有一些状态和零个或多个子资源。 每个子资源可以是简单资源或集合资源。

- 集合资源(Collections)  （collection resource）
集合包含相同类型的资源列表。 例如，用户拥有一组联系人。

包含0到多个个体资源。例如所有Customers，一个客户的所有Sales Orders等。通常将集合作为一个工厂，通过向集合提交一个HTTP POST请求来创建一个新成员。  
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


Instead of thinking of actions (verbs), it’s often helpful to think about putting a message in a letter box: e.g., instead of having the verb cancel in the url, think of sending a message to cancel an order to the cancellations letter box on the server side.

## Sub-resources
某些API可能含有或者引用子资源。  
每一个子路径都是对一个资源或者资源集合的有效引用。例如：
如果/a/b/c/d是一个有效路径，那/a/b/c, /a/b, /a 原则上也应是有效的。

如果子资源只能通过父资源访问，或者只能因父资源存在，使用nested url。如果可以通过ID访问，应该暴露成顶层资源。

资源能否被单独创建？还是必须和父资源一起创建？
在一个事务中分别创建多个资源？


## Misc
只有在必要时才使用UUID。
我们应该总是使用字符串而不是数字类型作为标识符。   
Usually, random UUID is used - see UUID version 4 in RFC 4122

限制资源数量；  
限制资源层级（<=3）



## 资源粒度
从客户和网络的角度来确定合适的资源粒度(granularity)。其他因素：
- 缓存(Cacheability)
- 变化频率(Frequency of change)
- 可变性(Mutability)


例如：客户和地址。是把地址数据作为客户资源的一部分，还是把客户和地址作为两个不同的资源呢？如果我们要把客户缓存到gateway上面，包含address数据可能就太大了。

基于用户的使用模式而不是数据库或对象模型来设计资源。

API应该包含完整的业务流程，包含涉及流程的所有资源。这防止API只是database的一个简单包装，避免将业务逻辑移往客户端。

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

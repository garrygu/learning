# 什么是统一接口(Uniform Interfaces)？
[HTTP协议](https://tools.ietf.org/html/rfc2616)定义了由下列标准方法组成的统一接口：
  - OPTIONS
  - GET
  - HEAD
  - POST
  - PUT
  - DELETE
  - TRACE

所谓统一接口，指这些方法的语法和含义不会随着应用或者资源而改变。

REST一般只使用有限的HTTP操作集合，包括HTTP `GET`, `PUT`, `DELETE`和`POST`.



# 安全与幂等
在处理HTTP方法时，RFC 2616规范定义了两个重要概念：`Safe`和`Idempotent`（幂等）。`Safe`指方法不能修改资源的状态；`Idempotent`指将一个请求发送多次和发送一次的结果是一样的。

|方法   |Safe     |Idempotent |描述|
|------ |:------: |:------:   |------|
|GET    |Y        |Y          |根据URL获取资源的一个呈现。<br>GET请求永远不应导致资源状态的改变。|
|PUT    |N        |Y          |更新资源（如果指定资源不存在，在允许的条件下创建新资源）|
|DELETE |N        |Y          |删除资源|
|POST   |N        |Y          |提交数据到服务器，对指定资源进行处理。用于创建新资源或者向已有资源添加数据。|



# 自定义HTTP方法
除了上述的标准HTTP方法外，还有一些非标准方法，例如WebDAV提供的`PROPFIND, PROPPATCH, MOVE, LOCK, UNLOCK`等, `PATCH`, `MERGE`等。

**使用非标准HTTP方法可能存在的问题：**  
代理、缓存、HTTP库等通常会将非标准HTTP方法按照非幂等、不安全、不能缓存进行对待（和`POST`类似）。

**推荐做法：**   
尽量使用`POST`来代替非标准HTTP方法。

**HTTP头：Allow**  
通过HTTP [Allow](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.7)头可以知道资源支持哪些HTTP方法。例如：  
```
Allow: GET, PUT, DELETE
```

# 自定义HTTP Headers


**推荐做法：**  
自定义HTTP Headers只用于提供信息目的（客户端可忽略）

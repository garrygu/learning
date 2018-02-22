


## 推荐返回格式
- GET  
单个资源，返回的资源将映射到整个响应主体。  
```
get /sales-order/{so#}
{
  _self: ...
  _next: ...
  sales_order: {
    ...
  }
}
```

- LIST
响应主体应该包含资源列表以及可选的元数据。
```
get /sales-order?dateFrom={x}&DateTo={x}
{
  _self: ...
  _next: ...
  sales_orders: [
    sales-order:{}
    sales-order:{}
  ]  
}
```

- 创建 (Create)
它在指定的父项下创建一个新资源，并返回新创建的资源。  
返回的资源将映射到整个响应主体。  

如果Create方法支持客户端分配的资源名称并且该资源已存在，则该请求应该失败并显示错误代码ALREADY_EXISTS  


- 更新（Update）
它更新指定的资源及其属性，并返回更新后的资源。  
任何重命名或移动资源的功能都不得在Update方法中发生，而应由自定义方法处理。  

- 标准Update方法应支持部分资源更新，并使用名为update_mask的FieldMask字段使用HTTP动词PATCH 。
- Update方法需要更高级的修补语义，例如添加到重复字段（repeated field）， 应该通过自定义方法提供 。
- 如果Update方法只支持完整资源更新，则必须使用HTTP动词PUT 。 但是，极度不鼓励完全更新，因为它在添加新资源字段时存在向后兼容性问题。  
- 响应消息必须是已更新的资源本身。

如果API接受客户端分配的资源名称，则服务器可能允许客户端指定不存在的资源名称并创建新资源。 否则， Update方法应该失败并显示不存在的资源名称。 如果它是唯一的错误条件，则应该使用错误代码NOT_FOUND 。

一个支持资源创建的Update方法的API也应该提供一个Create方法。


- 删除（Delete）
- API 不应该依赖任何由Delete方法返回的信息，因为它不能被重复调用。
- 没有请求主体; API配置不能声明body子句。
- 如果Delete方法立即删除资源，它应该返回一个空的响应。
- 如果Delete方法启动一个长时间运行的操作，它应该返回长时间运行的操作。
- 如果Delete方法只标记资源被删除，它应该返回更新的资源。

调用Delete方法应该是幂等的，但不需要产生相同的响应。 任何数量的Delete请求都应该导致（最终）删除资源，但只有第一个请求才会导致成功代码。 随后的请求应该会导致google.rpc.Code.NOT_FOUND 。


# 非标准方法
Send mail:创建一个email消息并不一定需要发送(draft)。和把消息移到Outbox集合相比，custom method更为直观更易为API用户发现（discoverable）。

批处理方法：对性能要求很高的方法，提供批处理方法可能减少per request成本。

- Cancel (POST)
- BatchGet: batch get of multiple resources (GET)
- Move (POST)
- Search (GET)
- Undelete (POST)   restore a resource






do not use verbs
- /getAllCars         GET /cars
- /createNewCar       POST /cars
- /deleteAllRedCars   DELETE /cars








# References

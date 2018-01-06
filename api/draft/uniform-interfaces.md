# 统一接口

1）	支持GET, POST, PUT, DELETE四种HTTP方法。使用GET查询资源；POST创建资源；PUT更新资源；DELETE删除资源  
2）	如果服务器、防火墙或网络不支持PUT和DELETE操作，应允许使用method参数，通过POST方法执行相应操作。例如：
POST https://graph.facebook.com/COMMENT_ID?method=delete.
3）	DELETE with payload
Allow to delete with sending context data.  例如，约束检查  
4）	POST
let the server indicate, with an HTTP “Location” header, the new resource’s URL  
5）	404 error  
6) we ask the resource what operations it's prepared to process using the HTTP OPTIONSverb

# 自定义HTTP Headers
- 自定义HTTP Headers应只用于提供信息目的（客户端可忽略），避免用于业务逻辑或流程控制
- 自定义头命名使用`X-`前缀  
- 预定义HTTP头:
  ...v

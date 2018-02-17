# HTTP Headers
- Content-Type
- Last-Modified  
该时间戳表示资源表示最近改变的时间。客户端和缓存据此判断本地的数据副本是否过时。  
GET请求返回应总是提供该header.
- ETag


Custom HTTP headers must not be used to change the behavior of HTTP methods




# 其他
- 空字段的处理


All exceptions should be mapped in an error payload.
```
{
  "errors": [
   {
    "userMessage": "Sorry, the requested resource does not exist",
    "internalMessage": "No car found in the database",
    "code": 34,
    "more info": "http://dev.mwaysolutions.com/blog/api/v1/errors/12345"
   }
  ]
}

```

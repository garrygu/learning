# 请求参数
每一种请求都可以通过以下三种方式传值：
	URI 参数
	Request Body
	HTTP Header
## URI参数
URI QueryString，主要用于GET和DELETE请求。
例如： GET /order?customerNumber=123456&orderStatus=open HTTP/1.1
## REQUEST BODY
把相应的请求数据以XML / JSON / NameValue形式通过Request DTO POST/PUT到服务器上。
Request Body主要用于POST/PUT请求。不推荐在GET请求中包含Request Body 。

Scheme
## HTTP HEADER
任何HTTP协议允许的Request Headers都可以直接使用。但不应将放在实体主体里的信息放进报头。
暂不可以使用自定义Request Headers。
X-API-* key ? requested by gateway


https://tools.ietf.org/html/rfc6648

# 请求格式
当向服务端POST/PUT数据时, 可以通过HTTP头Content-Type来指定请求数据的格式。如果不能正确指定, 在服务器端会解析错误。
Content-Type一般有以下几种：  
	XML：application/xml  
	JSON：application/json  
	JSON：application/jsv  
	URL Encoded：application/x-www-form-urlencoded  
如果不指定Content-Type, 会默认请求数据编码为：application/x-www-form-urlencoded.  
请求格式示例请参考： `/* Key/Cross-Team Projects/* API/4. API接口描述`  

#	批量请求（BATCH REQUEST）
允许客户同时创建、更新或删除多个资源。  
在一个Request Body里包含多条记录，一次提交。

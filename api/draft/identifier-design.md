1)	无歧义识别目标资源  
2)	模块化。Web服务的资源和方法可能增长很快。使用Domain和sub domain对资源进行分组或分隔。只使用规定的domain和sub domain名字。  
3)	在设计阶段就要规划好URI结构：{host: port}/{ domain}/{version#}/{sub domain}/{resource}  
4)	URL长度不超过2000字符（包括协议、域名和端口部分）  


12)	URI对客户是不透明（opaque）的，即客户不需要了解URL的结构以自行拼接URI。可将实际的URI包含在服务响应中。这样做的坏处是使输出变大，但可以很容易将客户引向新的URI，从而降低客户端和服务端的耦合。  
13)	URI尽量不要改变。如果必须改变，当客户访问旧的URI使用301（Move Permanently）将客户重新引导到新的URI；或者在指定的时间后，返回410（Gone）或者404（Not Found）。例如：  
```
Request
GET /order/12345678 HTTP/1.1
Host: <neweggcentral-rest>
Accept: application/xml; charset=UTF-8

Response #1
HTTP/1.1 301 Move Permanently
Location: http:// neweggcentral-rest2/order/12345678

Response #2
HTTP/1.1 410 Gone
Content-Type：application/xml; charset=UTF-8
Expires: Sat, 01 Jan 2013 00:00:00 GMT
```

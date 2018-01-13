1)	无歧义识别目标资源  
2)	模块化。Web服务的资源和方法可能增长很快。使用Domain和sub domain对资源进行分组或分隔。只使用规定的domain和sub domain名字。  
3)	在设计阶段就要规划好URI结构：{host: port}/{ domain}/{version#}/{sub domain}/{resource}  
4)	URL长度不超过2000字符（包括协议、域名和端口部分）  
5)	URL主干部分（不包括参数）全部小写；参数名称第一个字母小写  
6)	资源全部使用单数形式。例如查询order集合，使用/order而不是/orders。  
7)	**使用连字符 `-` ，不使用下划线 `_`**，以增加URI的可读性    
8)	**使用 / 表示资源的层次关系。**`/`分开的URI路径的每一部分都对应一种资源。例如：
```
/customer/{customer#}
/shape/polygon/quadrilateral/square
```

9)	**URL的末尾不添加 / 字符**   
10)	使用 ，或 ；表示平行资源。例如：POST /order/{order1},{order2} : 同时处理{order1}和{order2}。  
11)	URI标识的是资源而不是操作。除了特殊的Controller资源外，URI中的资源尽量只有名词形式。例如，从一个账户转钱到另一个账户，不是使用动词transfer，而是把之看成是一次交易记录，使用名词形式transaction。例如：
```
Request
POST /transactions HTTP/1.1
Host: <snip, and all other headers>
from=1&to=2&amount=500.00

Response
HTTP/1.1 201 OK
Date: Sun, 3 Jul 2011 23:59:59 GMT
Content-Type: application/json
Content-Length: 12345
Location: http://foo.com/transactions/1  
{"transaction":{"id":1,"uri":"/transactions/1","type":"transfer"}}
```
特别地，CRUD名字不要出现在URI中。
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

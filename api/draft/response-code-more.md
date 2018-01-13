 RFC 2616定义了状态行（Status-Line）语法：
 ```
 Status-Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF
 ```

 HTTP定义了40个标准状态码，共分5类：

 |类别|描述|
 |------|------|
 |1xx<br>Informational|传输协议层信息|
 |2xx<br>Success|客户请求成功
 |3xx<br>Redirection|客户必须采取额外的行动以完成请求|
 |4xx<br>Client Error|客户端错误|
 |5xx<br>Server Error|服务端错误|

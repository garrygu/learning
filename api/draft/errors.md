错误模型以及开发人员如何正确生成和处理错误的一般指导。

# Error Model

错误模型由[google.rpc.Status](https://github.com/googleapis/googleapis/blob/master/google/rpc/status.proto)逻辑定义，当发生API错误时，该实例会返回给客户端。

- int32 code  
// A simple error code that can be easily handled by the client. The
// actual error code is defined by `google.rpc.Code`.
- string message  
// A developer-facing human-readable error message in English. It should
// both explain the error and offer an actionable resolution to it.  
- string[] details  
// Additional error information that the client code can use to handle
// the error, such as retry delay or a help link.
例如在批处理中返回包含多个错误信息。

```
message Status {
  // The status code, which should be an enum value of [google.rpc.Code][google.rpc.Code].
  int32 code = 1;

  // A developer-facing error message, which should be in English. Any
  // user-facing error message should be localized and sent in the
  // [google.rpc.Status.details][google.rpc.Status.details] field, or localized by the client.
  string message = 2;

  // A list of messages that carry the error details.  There is a common set of
  // message types for APIs to use.
  repeated google.protobuf.Any details = 3;
}
```

```
{
  "error": {
    "code": 401,
    "message": "Request had invalid credentials.",
    "status": "UNAUTHENTICATED",
    "details": [{
      "@type": "type.googleapis.com/google.rpc.RetryInfo",
      ...
    }]
  }
}
```

# Error Code
Google API 必须使用[google.rpc.Code](https://github.com/googleapis/googleapis/blob/master/google/rpc/code.proto)定义的规范错误代码。 各个API 应避免定义其他错误代码，因为开发人员不太可能编写逻辑来处理大量的错误代码。 作为参考，每个API调用平均处理3个错误代码意味着大多数应用程序逻辑仅仅用于错误处理，这不是一个好的开发人员体验。


# Error Messages
- 不要假设用户知道你的服务实现或者熟悉错误的上下文（比如日志分析）。
- 保持简短的错误信息。 如果需要，请提供一个链接，让困惑的读者可以提出问题，提供反馈，或获取更多不符合错误信息的信息。 否则，请使用详细信息字段进行展开。

# Error Details
https://github.com/googleapis/googleapis/blob/master/google/rpc/error_details.proto

# Error Localization
message字段是面向开发人员的，并且必须使用英文。  
默认情况下，API服务应使用经过身份验证的用户的区域设置或HTTP Accept-Language标头来确定本地化的语言。


# Handling Errors
## Error Retries (错误重试)
客户应重试500和503指数退避错误。 除非另有说明，否则最小延迟应为1秒。 对于429错误，客户端可能会以最少30秒的延迟重试。 对于所有其他错误，重试可能不适用 - 首先确保您的请求是幂等的，并查看错误消息以获取指导。

## 错误传播(Error Propagation)
如果您的API服务依赖于其他服务，则不应该盲目地将错误从这些服务传播到您的客户端。 在翻译错误时，我们建议如下：

- 隐藏实施细节和机密信息。
- 调整对错误负责的一方。 例如，从另一个服务接收到INVALID_ARGUMENT错误的服务器应该将INTERNAL传播给它自己的调用者。

## 生成错误（Generating Errors）
如果您是服务器开发人员，则应该用足够的信息来产生错误，以帮助客户开发人员了解并解决问题。 与此同时，您必须了解用户数据的安全性和隐私，并避免在错误消息和错误详细信息中公开敏感信息，因为错误通常会被记录下来并可能被其他人访问。 例如，“客户端IP地址不在白名单128.0.0.0/8”之类的错误消息公开了有关服务器端策略的信息，用户可能无法访问该信息。

 服务器应用程序可以并行检查多个错误条件，并返回第一个错误条件。  

 注 ：由于客户端无法修复服务器错误，因此生成其他错误详细信息无用。 为避免在错误情况下泄露敏感信息，建议不要生成任何错误消息，并只生成google.rpc.DebugInfo错误详细信息。 DebugInfo是专门为服务器端日志记录而设计的，不能发送给客户端。

 google.rpc包定义了一组标准错误有效载荷，它们比自定义错误有效载荷更受欢迎。


Error responses examples:  
```
{
  "developerMessage" : "Verbose, plain language description of the problem.
             Provide developers suggestions about how to solve their problems here",
  "userMessage" : "This is a message that can be passed along to end-users, if needed.",
  "errorCode" : "444444",
  "moreInfo" : "http://api.example.gov/v1/documentation/errors/444444.html"
}
```

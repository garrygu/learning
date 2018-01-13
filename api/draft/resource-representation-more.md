REST API通常用文本格式来表述资源状态。最常用的就是XML和JSON。 我们倾向于使用JSON格式。JSON的缺点是不支持隐藏注释和包装长字符串。


Rule: Additional envelopes must not be created
A REST API must leverage the message “envelope” provided by HTTP. In other words, the body should contain a representation of the resource state, without any additional, transport-oriented wrappers.


# Link & Link Relations

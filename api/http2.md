- [Hypertext Transfer Protocol Version 2 (HTTP/2)](https://tools.ietf.org/html/rfc7540)

- [HTTP/2 Frequently Asked Questions](https://http2.github.io/faq/#what-are-the-key-differences-to-http1x)  
At a high level, HTTP/2:
  - is binary, instead of textual
  - is fully multiplexed, instead of ordered and blocking
  - can therefore use one connection for parallelism
  - uses header compression to reduce overhead
  - allows servers to “push” responses proactively into client caches  

  Currently not 100% of browsers support HTTP/2 in it’s entirety, but the situation is pretty good.  

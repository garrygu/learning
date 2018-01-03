# Links
- [HTTP/1.1, part 6: Caching](https://tools.ietf.org/html/draft-ietf-httpbis-p6-cache-16#section-3.2.3)
- [HTTP Caching](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching)

- [GraphQL vs REST: Caching](https://philsturgeon.uk/api/2017/01/26/graphql-vs-rest-caching/)
  - Client Caching
  - Network Caching
    > Tools like Varnish or Squid. An endpoint-based API can start off adding Etag and Cache-Control tags in the application itself.  
  - Application Caching
    > Software like Memcache, Redis, etc. These tools can leverage HTTP headers like Etag, Vary, Cache-Control to handle cache validation

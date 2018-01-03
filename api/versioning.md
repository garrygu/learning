# Links

## Resources
- [Semantic Versioning 2.0.0](https://semver.org)

## Discussion
- [API Versioning Has No “Right Way”](https://blog.apisyouwonthate.com/api-versioning-has-no-right-way-f3c75457c0b7)
- [API Change Management](https://blog.goodapi.co/api-change-management-2fe5bba32e9b)
- [Evolving HTTP APIs (12/04/2012)*](https://www.mnot.net/blog/2012/12/04/api-evolution.html)  
Suggested Practices:  
  - Keep Compatible Changes Out of Names
  - Avoid New Major Versions
  - Make Changes Backwards-Compatible
  - Think about Forwards-Compatibility
- [Web API Versioning Smackdown](https://www.mnot.net/blog/2011/10/25/web_api_versioning_smackdown)
- [The Sunset HTTP Header draft-wilde-sunset-header-03](https://tools.ietf.org/html/draft-wilde-sunset-header-03)  

  The Sunset HTTP response header field allows a server to communicate the fact that a resource is expected to become unresponsive at a specific point in time.  

  The Sunset value is an HTTP-date timestamp, as defined in [Section 7.1.1.1 of [RFC7231]](https://tools.ietf.org/html/rfc7231#section-7.1.1.1).  

  For example:  
  Sunset: Sat, 31 Dec 2018 23:59:59 GMT

  Scenarios:  
  - Temporary Resources
  - Migration
  - Retention
  - Deprecation

- [APIs as infrastructure: future-proofing Stripe with versioning](https://stripe.com/blog/api-versioning)
- [Versioning REST Web Services (05/11/2008)](http://barelyenough.org/blog/2008/05/versioning-rest-web-services/)

- [API Change Management (04/18/2017)](https://blog.goodapi.co/api-change-management-2fe5bba32e9b)  
What to version  
  - Server has a version
  - Client has a version
  - Message format passed between server and client has a version
  - API description file has a version
  - Resource doesn’t have a version
  - Relation between resources doesn’t have a version
  - API itself doesn’t have a version

  Rules for extending  
  1. You MUST NOT take anything away
  2. You MUST NOT change processing rules
  3. You MUST NOT make optional things required
  4. Anything you add MUST be optional

  Breaking change  
  1. Resource Identifier (URI) including any query parameters and their semantics
  2. Resource metadata (e.g. HTTP headers)
  3. Resource data (e.g. the HTTP body) fields and their semantics
  4. Actions the resource affords (e.g., available HTTP Methods)
  5. Relations with other resources (e.g., Links)

- [Your API versioning is wrong, which is why I decided to do it 3 different wrong ways (02/10/2014)](https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/)

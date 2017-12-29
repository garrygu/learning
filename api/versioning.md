# Links

## Resources
- [Semantic Versioning 2.0.0](https://semver.org)

## Discussion
- [API Versioning Has No “Right Way”](https://blog.apisyouwonthate.com/api-versioning-has-no-right-way-f3c75457c0b7)
- [API Change Management](https://blog.goodapi.co/api-change-management-2fe5bba32e9b)
- [Evolving HTTP APIs](https://www.mnot.net/blog/2012/12/04/api-evolution.html)  
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

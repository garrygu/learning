- [Representational State Transfer (REST)](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)


## Specifications
- [HTTP/1.1, part 3: Message Payload and Content Negotiation](https://tools.ietf.org/html/draft-ietf-httpbis-p3-payload-20)

### HTTP/2
- [HTTP/2 Frequently Asked Questions](https://http2.github.io/faq/#what-are-the-key-differences-to-http1x)  
At a high level, HTTP/2:
  - is binary, instead of textual
  - is fully multiplexed, instead of ordered and blocking
  - can therefore use one connection for parallelism
  - uses header compression to reduce overhead
  - allows servers to “push” responses proactively into client caches  

  Currently not 100% of browsers support HTTP/2 in it’s entirety, but the situation is pretty good.  


## OpenAPI
- [www.openapis.org](https://www.openapis.org/)
- [OpenAPI-Specification](https://github.com/OAI/OpenAPI-Specification)
- https://github.com/Microsoft/OpenAPI.NET


## API
- [The API Stack](http://theapistack.com/)


## Micro-services
- [microservices.io](http://microservices.io/index.html)
- [Eventuate Platform](http://eventuate.io/)
- [istio](https://istio.io/)  
>An open platform to connect, manage, and secure microservices


## Blogs
- [The API Evangelist](https://apievangelist.com/)
- [API Handyman](https://apihandyman.io/)
- [Launchany blog ](http://launchany.com/articles/)
- [everydeveloper.com](http://everydeveloper.com)


## API Newsletters
- [API weekly Newsletter](http://launchany.com/subscribe/)
- [REST API Notes](https://tinyletter.com/RESTAPINotes)
- [GET PUT POST](https://tinyletter.com/getputpost)
- [APIdays Newsletter](http://global.apidays.io/)
- [NordicAPIs Newsletter ](http://nordicapis.com/newsletter/)
- [API craft google group](https://groups.google.com/forum/#!forum/api-craft)
- [ProgrammableWeb](http://www.programmableweb.com/)
- [The New Stack](http://thenewstack.io/)


## Frameworks
- [swagger](http://swagger.io/)


## Description Formats
- Swagger (Open API)
- RAML
- API Blueprint
  - [MSON](https://github.com/apiaryio/mson)
  > Markdown Syntax for Object Notation (MSON), a Markdown syntax compatible with describing JSON and JSON Schema.

- WADL




## Tools
### Design
- https://swaggerhub.com/
> SwaggerHub Supports the Entire API Lifecycle and the Tools that Drive it

### Documentation
- [readme](https://readme.io/)
- [3scale ActiveDocs](https://www.3scale.net/api-management/interactive-api-documentation/)
- [lord/slate](https://github.com/lord/slate)
> Beautiful static documentation for your API

### Search and Discovery
- http://apis.io/
- [Google API Discovery Service](https://developers.google.com/discovery/)

### Sharing
- https://apimatic.io/
- https://www.getpostman.com/
- https://apiary.io/

### Aggregation
- https://www.api2cart.com/
> Connect with shopping carts. As many as you need. Via one integration.
- https://zapier.com/
> Connect Your Apps and Automate Workflows

### Monitoring & Testing
- https://www.runscope.com/
- https://www.apiscience.com/
- https://smartbear.com/


## Media Types
- [Media Types](http://amundsen.com/media-types/)
- [Collection+JSON - Hypermedia Type](http://amundsen.com/media-types/collection/)
- [hyper-item: a hypermedia specification](https://github.com/mdemuth/hyper-item)


## Misc
- http://apibusters.com
> A no-nonsense podcast about APIs, HTTP, service-orientated architecture, Internet of Things, etc.

- http://nordicapis.com/

- https://streamdata.io/doc/
>SSE

- https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
> Representational State Transfer (REST)

- [AWS SDK for Go 2.0 Developer Preview](https://aws.amazon.com/blogs/developer/aws-sdk-for-go-2-0-developer-preview/?sc_channel=sm&sc_campaign=Developer_Blog&sc_publisher=TWITTER&sc_country=Global&sc_geo=GLOBAL&sc_outcome=awareness&trk=_TWITTER&sc_content=blog&linkId=46195064)

- [JSON Patch]https://tools.ietf.org/html/draft-ietf-appsawg-json-patch-10)
- [PATCH Method for HTTP](https://tools.ietf.org/html/rfc5789)
> PATCH is neither safe nor idempotent as defined by [RFC2616], Section 9.1.  
> Clients using this kind of patch application SHOULD use a conditional request such that the request will fail if the resource has been updated since the client last accessed the resource.  For example, the client    can use a strong ETag [RFC2616] in an If-Match header on the PATCH request.

- [HTTP/1.1, part 1: URIs, Connections, and Message Parsing](https://tools.ietf.org/html/draft-ietf-httpbis-p1-messaging-16#page-43)

## Concepts
- [What Is REST? (video)](http://www.restapitutorial.com/lessons/whatisrest.html)  

The REST architectural style describes six constraints. These constraints, applied to the architecture, were originally communicated by Roy Fielding in his doctoral dissertation (see http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) and defines the basis of RESTful-style.

The six constraints are: (click the constraint to read more)
  - Uniform Interface
  - Stateless
  - Cacheable
  - Client-Server
  - Layered System
  - Code on Demand (optional)

- [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)
- [Richardson Maturity Model](https://www.martinfowler.com/articles/richardsonMaturityModel.html)  
A model (developed by Leonard Richardson) that breaks down the principal elements of a REST approach into three steps. These introduce resources, http verbs, and hypermedia controls.

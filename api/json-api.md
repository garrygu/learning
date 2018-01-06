http://jsonapi.org  
http://jsonapi.org/format/  
https://github.com/json-api/json-api  

# Latest Specification (v1.0)
> to minimize both the number of requests and the amount of data transmitted between clients and servers.

JSON API media type (application/vnd.api+json)  
http://www.iana.org/assignments/media-types/application/vnd.api+json


## Content Negotiation
### Client Responsibilities
```
Content-Type: application/vnd.api+json
```

### Server Responsibilities
```
Content-Type: application/vnd.api+json
```
Servers MUST respond with a `415 Unsupported Media Type` status code if a request specifies the header `Content-Type: application/vnd.api+json` with any media type parameters.  

Servers MUST respond with a `406 Not Acceptable` status code if a request’s Accept header contains the JSON API media type and all instances of that media type are modified with media type parameters.

## Document Structure
### Top Level
A JSON object MUST be at the root of every JSON API request and response containing data. This object defines a document’s “top level”.

A document MUST contain at least one of the following top-level members:
- data: the document’s “primary data”
- errors: an array of error objects
- meta: a meta object that contains non-standard meta-information.

A document MAY contain any of these top-level members:
- jsonapi: an object describing the server’s implementation
- links: a links object related to the primary data.
- included: an array of resource objects that are related to the primary data and/or each other (“included resources”).

The document’s “primary data” is a representation of the resource or collection of resources targeted by a request.

The top-level links object MAY contain the following members:
- self: the link that generated the current response document.
- related: a related resource link when the primary data represents a resource relationship.
- pagination links for the primary data.

### Resource Objects
“Resource objects” appear in a JSON API document to represent resources.  
A resource object MUST contain at least the following top-level members:
- id
- type

### Resource Identifier Objects
A “resource identifier object” is an object that identifies an individual resource.  
A “resource identifier object” MUST contain `type` and `id` members.

### Compound Documents
To reduce the number of HTTP requests, servers MAY allow responses that include related resources along with the requested primary resources.   


# Links
- [Making the Most of JSON-API](https://blog.apisyouwonthate.com/making-the-most-of-json-api-7fb51f4407aa)
- [](http://discuss.jsonapi.org)

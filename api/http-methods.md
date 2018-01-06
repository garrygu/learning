[PUT vs PATCH vs JSON-PATCH](https://philsturgeon.uk/api/2016/05/03/put-vs-patch-vs-json-patch/)  
The existing HTTP PUT method only allows a complete replacement of a document. This proposal adds a new HTTP method, PATCH, to modify an existing HTTP resource.  

[PATCH Method for HTTP](https://tools.ietf.org/html/rfc5789)  
PATCH is neither safe nor idempotent as defined by [RFC2616], Section 9.1.  

Clients using this kind of patch application SHOULD use a conditional request such that the request will fail if the resource has been updated since the client last accessed the resource.  For example, the client    can use a strong ETag [RFC2616] in an If-Match header on the PATCH request.

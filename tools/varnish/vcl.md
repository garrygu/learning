# Varnish Configuration language
Every written VCL code will be compiled to binary code when you start your Varnish Cache.


## vcl syntax
- Comments:  
You can use `//`, `#`, or `/* your comment inside */`  

- Assignment and logical operators:  
Assignments: `=`  
Comparisons: `==` or `!=`  
Logical operations: `&&` for `AND`, `||` for `OR`

- Regular expressions:  
Varnish uses the **Perl-compatible regular expressions (PCRE Regex).**  
contains: `~`  
does not contain: `!~`


## vcl functions
- regsub() and regsuball()  
`regsub()` function replaces only the first match and the `regsuball()` function replaces all matching occurrences.
```
regsub(req.url, "\?.*", "");
```
Ref:
  - [VCL regular expression cheat sheet](https://docs.fastly.com/en/guides/vcl-regular-expression-cheat-sheet)


- purge   
 Invalidate stale content. Send an HTTP `PURGE` request to Varnish Cache whenever your backend receives an HTTP `POST` or `DELETE`.

- ban() and ban_url()   
They create a filter, instructing if a cached object is supposed to be delivered or not. Adding a new filter to the ban list will not remove the content that is already cached—what it really does is exclude the matching cached objects from any subsequent requests, forcing a cache miss.
```
ban_url("\.xml$")
ban("req.http.host ~ " + yourdomain.com )
```
Too many entries in the ban list will consume extra CPU space.

- return()  
 It determines to which subroutine the request should proceed to.
```
return(restart)
return(lookup)
return(pipe)
return(pass)
```

- hash_data()  
It is responsible for setting the hash key used to store an object and it is only available inside the `vcl_hash` subroutine. The default value for the `hash_data` variable is `req.url` (requested URL).  In a multi-domain website, concatenating the value of the `req.http.Host` variable would add the domain name to the hash key, making it unique.

- error  
The first argument is the HTTP error code and the second is a string with the error code message.
```
if (!client.ip ~ purge) {
  error 405 "Not allowed.";
}
```

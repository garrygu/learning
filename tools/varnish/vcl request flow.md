
# vcl - request flow
https://book.varnish-software.com/3.0/VCL_Basics.html
![](https://book.varnish-software.com/3.0/_images/vcl.png)


## vcl_recv
The `vcl_recv` routine is the first subroutine to be executed when a request comes in. At this point, you can normalize URL, add or subtract HTTP headers, strip or delete cookies, define which backend or director will respond to the request, control access, and much more.  

### Default `vcl_recv` subroutine
```
if (req.restarts == 0) {
  if (req.http.x-forwarded-for) {
    set req.http.X-Forwarded-For =
    req.http.X-Forwarded-For + ", " + client.ip;
  } else {
    set req.http.X-Forwarded-For = client.ip;
  }
}
```
The `req.restarts` object is an internal counter that indicates how many times the request was restarted. Restarting is commonly done when a backend server is not responding or an expected error arises, and by doing so, you can choose another backend server, rewrite the URL, or take other actions to prevent an error page.

```
if (req.request != "GET"&&
  req.request != "HEAD"&&
  req.request != "PUT"&&
  req.request != "POST"&&
  req.request != "TRACE"&&
  req.request != "OPTIONS"&&
  req.request != "DELETE") {
  /* Non-RFC2616 or CONNECT which is weird. */
  return (pipe);
}
```
If the incoming request is not a known HTTP method, Varnish will pipe it to the backend and let it handle the request.

```
if (req.request != "GET"&&req.request != "HEAD") {
  /* We only deal with GET and HEAD by default */
  return (pass);
}
```

```
if (req.http.Authorization || req.http.Cookie) {
  /* Not cacheable by default */
  return (pass);
}
```
Avoiding cache for user-specific requests.

```
return (lookup);
```
The final statement inside the `vcl_recv` subroutine instructs Varnish to look up the requested URL in cache.


### Stripping cookies from requests
```
if (req.url ~ "\.(png|gif|jpeg|jpg|ico|swf|css|js|txt|xml)"
|| req.url ~ "/static/"
|| req.url ~ "/images/") {
  unset req.http.cookie;
}
```


### Define which backend or director will receive the request
```
if(req.url ~ "/java/"
  set req.backend = java;
} else {
  set req.backend = php;
}
```


### Create security barriers to avoid unauthorized access
```
if (req.url ~ "\.(conf|log|old|properties|tar|war)$") {
  error 403 "Forbidden";
}
```


## vcl_hash
Its responsibility is to generate a hash that will become the key in the memory map of stored objects and play an important role in achieving a high hit ratio.  
The default value for an object's key is its URL.

### Default `vcl_hash` subroutine
```
sub vcl_hash {
  hash_data(req.url);
  if (req.http.host) {
    hash_data(req.http.host);
  } else {
    hash_data(server.ip);
  }
  return (hash);
}
```

### Avoiding double caching for www.youdomain.com and yourdomain.com
```
sub vcl_hash {
  hash_data(req.url);
  if (req.http.host) {
    set req.http.host = regsub(req.http.host,"^www\.yourdomain\.com$", "yourdomain.com");
    hash_data(req.http.host);
  } else {
    hash_data(server.ip);
  }
  return (hash);
}
```

### Removing domain name (host) from static files of multi-language websites:
```
if (req.url ~ "\.(png|gif|jpeg|jpg|ico|swf|css|js|txt|xml)"{
  set req.http.X-HASH = regsub(req.url,".*\.yourdomain\.[a-zA-Z]{2,3}\.[a-zA-Z]{2}", "");
}
hash_data(req.http.X-HASH);
```

### Normalizing SEO URLs for easier purging
```
if (req.url ~ "\.(html|htm)(\?[a-z0-9=]+)?$" {
  set req.http.X-HASH = regsub(req.http.X-HASH, "^/.*__", "/__");
}
hash_data(req.http.X-HASH);
```



## vcl_pipe & vcl_pass
- `vcl_pipe` subroutine will transmit the bytes back and forth after a pipe instruction is executed at the `vcl_recv` subroutine (and the subsequent VCL code will not be processed),
- `vcl_pass` subroutine will transmit the request to the backend without caching the generated response.
- Piping or passing a request can be useful when users reach a protected section of the website.
- Passing requests is the default action for protected or personalized sections of your website and requires no extra work.
- By piping requests, you can stream large objects, but you need to be careful or all other subsequent requests for that same client will also be piped.  
- Both subroutines respect a `keep-alive` header.

### Default `vcl_pipe` subroutine:  
```
sub vcl_pipe {
      # Note that only the first request to the backend will have
      # X-Forwarded-For set.  If you use X-Forwarded-For and want to
      # have it set for all requests, make sure to have:
      # set bereq.http.connection = "close";
      # here. It is not set by default as it might
  # break some broken web applications, like IIS with NTLM    
  # authentication.
      return (pipe);
}
```

### Default `vcl_pass` subroutine:
```
sub vcl_pass {
      return (pass);   //The request will be passed to the backend and the generated response will not be cached.
}
```

### Add a `connection` header to piped requests:  
```
sub vcl_pipe {
      set bereq.http.connection = "close";
      return (pipe);
}
```


## vcl_fetch
The `vcl_fetch` subroutine is the first subroutine to deal with the response phase (called when the response has been gathered from the backend before placing it in the cache) and it plays an important role on caching policies and [Edge-side Include (ESI)](https://www.w3.org/TR/esi-lang).

When dealing with a legacy system that does not provide a `cache-control` header, you can hardcode a `time to live (ttl)` value to the content that should be cached.  

While you can manipulate requests based on client-provided data using the `req.*` variable in the `vcl_recv` subroutine, you can do the same data manipulation in the `vcl_fetch` subroutine, but with data provided by a backend server using the `beresp.*` variable (beresp = backend response).  


### Default `vcl_fetch` subroutine:  
```
sub vcl_fetch {
      if (beresp.ttl <= 0s ||
          beresp.http.Set-Cookie ||
          beresp.http.Vary == "*") {
                    /*
                     * Mark as "Hit-For-Pass" for the next 2 minutes
                     */
                    set beresp.ttl = 120 s;
                    return (hit_for_pass);  
      }
      return (deliver);
}
```
 The default vcl_fetch behavior will not cache the response if your backend server provides a zero or negative `ttl` value, a `Set-cookie` header, or a `Vary` header.  Instead, Varnish will cache a dummy object that instructs the next requests for this URL to be passed for the next two minutes. This is called `hit-for-pass`.


### Overriding the default time to live of a cached object:  
```
if ( req.url ~ "\.(png|gif|jpeg|jpg|ico|css|js|txt|xml)(\?[a-z0-9=]+)?$"){
  set beresp.ttl = 1d;
}
```
If our backend server provides an HTTP `cache-control` or `expires` header with a different time frame, we will override it with the set command.  

Disabling cache:
```
if ( req.url ~ "/donotcache\.html") {
  set beresp.ttl = 0s;
}
```

### Stripping cookies for static content:  
```
if ( req.url ~ "/static/") {
  set beresp.ttl = 30m;
  unset beresp.http.set-cookie;
}
```
Removing the HTTP `set-cookie` header from the response allows us to sanitize the object before inserting it into memory.


### Restart requests that failed:  
```
if ( beresp.status>= 500 &&req.request != "POST") {
  return(restart);
}
```
Restarting will take the request back to the `vcl_recv` subroutine. Let the restarted request pick another backend instead of our original failed server:  
```
sub vcl_recv {
  if (req.restarts == 0) {
    # Try the director first.
    set req.backend = director1;
  } else if (req.restarts == 1) {
    # Director has failed and we will try the backend 1.
    set req.backend = b1;
  } else if (req.restarts == 2) {
    # Backend 1 has failed. Try backend 2.
    set req.backend = b2;
  } else {
    # All backend servers have failed. Go to error page.
    error 503 "Service unavailable";
  }
}
```


### Inspecting why the response was not cached:  
```
sub vcl_fetch {
  if (beresp.ttl<= 0s) {
    # Cannot cache. Backend provided an expired TTL
    set beresp.http.X-Cacheable = "NO:ExpiredTTL";
  } elsif (req.http.Cookie) {
    # Presence of cookies.
    set beresp.http.X-Cacheable = "NO:Cookies";
  } elsif (beresp.http.Cache-Control ~ "private") {
    # Cache-control is private
    set beresp.http.X-Cacheable = "NO:Cache-Control=private";
  } else {
    set beresp.http.X-Cacheable = "YES";
  }
  return(deliver);
}
```
Sending a debug header alongside the response can help you understand the behavior of the cache and why a specific content was not cached.


## vcl_deliver
The `vcl_deliver` subroutine is the last step before a response returns to the client and can be used for server obfuscation, adding debug headers, and a last overall headers' clean up.

Because the `vcl_deliver` subroutine is executed after the object is placed into cache, all manipulation that happens inside the `vcl_deliver` subroutine will not be persisted.

### Default vcl_deliver behavior
```
sub vcl_deliver {
  return (deliver);
}
```

### Remove the server name for obfuscating purposes
```
sub vcl_deliver {
  unset resp.http.Server;
  return (deliver);
}
```

### Removing extra headers added by Varnish
```
sub vcl_deliver {
  unset resp.http.Server;
  unset resp.http.Via;
  unset resp.http.Age;
  unset resp.http.X-Varnish;
  return (deliver);
}
```

### Add cache hit or miss debug headers
```
sub vcl_deliver {
  if (obj.hits> 0) {
    set resp.http.X-Cache = "HIT";
  } else {
    set resp.http.X-Cache = "MISS";
  }
  unset resp.http.Via;
  unset resp.http.Age;
  unset resp.http.X-Varnish;
  unset resp.http.Server;
  return (deliver);
}
```


## vcl_error
The vcl_error subroutine handles odd behaviors from backend servers, and can also be used when you want to deny access to a request or redirect requests to a new location.

### Default vcl_error subroutine
```
sub vcl_error {
      set obj.http.Content-Type = "text/html; charset=utf-8";
      set obj.http.Retry-After = "5";
      synthetic {"
    <?xml version="1.0" encoding="utf-8"?>
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
     "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    <html>
        <head>
          <title>"} + obj.status + " " + obj.response + {"</title>
        </head>
        <body>
          <h1>Error "} + obj.status + " " + obj.response + {"</h1>
          <p>"} + obj.response + {"</p>
          <h3>Guru Meditation:</h3>
          <p>XID: "} + req.xid + {"</p>
          <hr>
          <p>Varnish cache server</p>
        </body>
    </html>
  "};
      return (deliver);
}
```

### Maintenance page
```
sub vcl_recv {
  error 503 "Service unavailable";
}
sub vcl_error{
  if ( obj.status == 503 ) {
    set obj.http.Content-Type = "text/html; charset=utf-8";
    set obj.http.Retry-After = "5";
    synthetic {"
      ### Insert here the HTML for the maintenance page ###
    "}
  }
}
```

### Redirecting users
```
sub vcl_error {
  if (obj.status>= 500) {
    set obj.http.location = "http://www.yourwebsite.com";
    set obj.status = 302;
    return (deliver);
  }
}
```



## References
- https://book.varnish-software.com/3.0/VCL_functions.html

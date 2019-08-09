

## vcl - request flow
https://book.varnish-software.com/3.0/VCL_Basics.html
![](https://book.varnish-software.com/3.0/_images/vcl.png)

## vcl_recv
## vcl_hash

## vcl_pipe & vcl_pass
- `vcl_pipe` subroutine will transmit the bytes back and forth after a pipe instruction is executed at the `vcl_recv` subroutine (and the subsequent VCL code will not be processed),
- `vcl_pass` subroutine will transmit the request to the backend without caching the generated response.
- Piping or passing a request can be useful when users reach a protected section of the website.
- Passing requests is the default action for protected or personalized sections of your website and requires no extra work.
- By piping requests, you can stream large objects, but you need to be careful or all other subsequent requests for that same client will also be piped.

Default `vcl_pipe` subroutine:  
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

Default `vcl_pass` subroutine:
```
sub vcl_pass {
      return (pass);   //The request will be passed to the backend and the generated response will not be cached.
}
```

Add a `connection` header to piped requests:  
```
sub vcl_pipe {
      set bereq.http.connection = "close";
      return (pipe);
}
```

## vcl_fetch
When dealing with a legacy system that does not provide a `cache-control` header, you can hardcode a `time to live (ttl)` value to the content that should be cached.  

While you can manipulate requests based on client-provided data using the `req.*` variable in the `vcl_recv` subroutine, you can do the same data manipulation in the `vcl_fetch` subroutine, but with data provided by a backend server using the `beresp.*` variable (beresp = backend response).  


Default `vcl_fetch` subroutine:  
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


Overriding the default time to live of a cached object:  
```
if ( req.url ~ "\.(png|gif|jpeg|jpg|ico|css|js|txt|xml)(\?[a-z0-9=]+)?$"){
  set beresp.ttl = 1d;
}
```
If our backend server provides an HTTP cache-control or expires header with a different time frame, we will override it with the set command.  

Stripping cookies for static content:  
```
if ( req.url ~ "/static/") {
  set beresp.ttl = 30m;
  unset beresp.http.set-cookie;
}
```
Removing the HTTP `set-cookie` header from the response allows us to sanitize the object before inserting it into memory.


Restart requests that failed:  
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

Inspecting why the response was not cached:  
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


## References
- https://book.varnish-software.com/3.0/VCL_functions.html

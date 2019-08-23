

## Command
```
// reload the configuration
# sudo service varnish reload

// run the daemon with the -C argument that will instruct the daemon to compile and print the C language code to the console.
# varnishd -C -f /etc/varnish/default.vcl
```



## Connecting to backend servers
- A simple backend declaration:
```
backend server01 {
  .host = "localhost";
  .port = "8080";
}
```


- Include a probing request to our backend:
```
backend website {
  .host = "localhost";
  .port = "8080";
  .probe = {
    .url = "/favicon.ico";
    .timeout = 60ms;
    .interval = 2s;
    .window = 5;
    .threshold = 3;
  }
}
```
To determine if a backend is healthy, it will analyze the last five probes. If three of them result in 200 – OK, the backend is marked as Healthy and the requests are forwarded to this backend; if not, the backend is marked as Sick and will not receive any incoming requests until it's Healthy again.  


- Probe the backend servers that require additional information:  
```
backend api {
  .host = "localhost";
  .port = "8080";
  .probe = {
    .request =
    "GET /status HTTP/1.1"
    "Host: www.yourhostname.com"
    "Connection: close"
    "X-API-Key: e4d909c290d0fb1ca068ffaddf22cbd0"
    "Accept: application/json"
    .timeout = 60ms;
    .interval = 2s;
    .window = 5;
    .threshold = 3;
  }
}
```
In case your backend server requires extra headers or has an HTTP basic authentication, you can change the probing from `URL` to `Request` and specify a raw HTTP request. When using the `Request` probe, you'll always need to provide a `Connection: close` header or it will not work.


- Declare probes as a separated configuration block:  
```
probe favicon {
  .url = "/favicon.ico";
  .timeout = 60ms;
  .interval = 2s;
  .window = 5;
  .threshold = 3;
}
probe robots {
  .url = "/robots.txt";
  .timeout = 60ms;
  .interval = 2s;
  .window = 5;
  .threshold = 3;
}
backend server01 {
  .host = "localhost";
  .port = "8080";
  .probe = favicon;
}
backend server02 {
  .host = "localhost";
  .port = "8080";
  .probe = robots;
}
```


- Choose a backend server based on incoming data:
```
vcl_recv {
  if ( req.url ~ "/api/") {
    set req.backend = api;
  } else {
    Set req.backend = website;
  }
}
```


## Load balance requests
There are six types of director groups to be configured: random, client, hash, round-robin, DNS, and fallback.

- Group the backend servers to load balance requests
```
director dr1 round-robin {
  { .backend = server01 }
  { .backend = server02 }
  { .backend = server03 }
  { .backend = server04 }
}
```

- Create a sticky-session pool of servers
```
director dr1 client {
  { .backend = server01 }
  { .backend = server02 }
  { .backend = server03 }
  { .backend = server04 }
}
```

Inside your `vcl_recv` subroutine, you'll need to specify what identifies a unique client. Varnish will use, as default, the client IP address,
```
sub vcl_recv {
  set client.identity = req.http.X-Forwarded-For;
}
```
The [`X-Forwarded-For`](https://tools.ietf.org/html/draft-ietf-appsawg-http-forwarded-10) header is used to maintain information lost in the proxying process.

- Weight-based load balancing:
```
director dr1 client {
  { .backend = server01 ; .weight=2; }
  { .backend = server02 ; .weight=2; }
  { .backend = server03 ; .weight=2; }
  { .backend = server04 ; .weight=1; }
}
```
Weighting the servers is only possible in the random or client directors.



# Concepts
## RxHeader host & TxHeader host
Rx and Tx mean "receive" and "transmit" respectively.  A request header
will be logged as RxHeader when received from the client, and with
TxHeader when sent to the backend.  Likewise, a response header will be
logged as RxHeader when received from the backend and as TxHeader when
sent to the client.  

Varnish uses whatever Host: header it received from the client;
otherwise it wouldn't be able to accelerate servers with multiple
virtual hosts.  If the Host: header you get from the client is not
correct, you can adjust req.http.host it in vcl_recv or bereq.http.host
in vcl_miss, e.g.:
```
vcl_recv {
    if (req.http.host ~ "^(www\.)?example.com$") {
        req.backend = example_backend;
        req.http.host = "backend.example.com";
    } else {
        error 404 "Unknown virtual host";
    }
}
```
https://varnish-cache.org/lists/pipermail/varnish-misc/2007-July/015153.html


# Tools
## varnishncsa


Ref:
- https://docs.varnish-software.com/tutorials/enabling-logging-with-varnishncsa/



## varnishhist
In short, you want as many | symbols as possible and you want everything far toward the left hand side. The closer to the left the faster the responses are regardless if they are cached or not. The more | symbols then more items were served from cache.
```
'|' is cache HIT
'#' is cache MISS
'n:m' numbers in left top corner is vertical scale
'n = 2000' is number of requests that are being displayed (from 1 to 2000)
```
The times on the X-axis are as such:
```
1e1 = 10 sec
1e0 = 1 sec
1e-1 = 0.1 secs or 100 ms (milliseconds)
1e-2 = 0.01 secs or 10 ms
1e-3 = 0.001 secs or 1 ms or 1000 µs (microseconds)
1e-4 = 0.0001 secs or 0.1 ms or 100 µs
1e-5 = 0.00001 secs or 0.01 ms or 10 µs
1e-6 = 0.000001 secs or 0.001 ms or 1 µs or 1000 ns (nanoseconds)
```

## varnishstat


## [varnishtop](https://varnish-cache.org/docs/trunk/reference/varnishtop.html)
Varnish log entry ranking  
### SYNOPSIS
```
varnishtop [-1] [-b] [-c] [-C] [-d] [-f] [-g <session|request|vxid|raw>] [-h] [-i <taglist>] [-I <[taglist:]regex>] [-L <limit>] [-n <dir>] [-p <period>] [-Q <file>] [-q <query>] [-r <filename>] [-t <seconds|off>] [-T <seconds>] [-x <taglist>] [-X <[taglist:]regex>] [-V]

V3:
varnishtop [-1] [-b] [-C] [-c] [-d] [-f] [-I regex] [-i tag] [-n varnish_name] [-r file] [-V] [-X regex] [-x tag]
```

`-b`     Include log entries which result from communication with a backend server.  If neither -b nor -c is specified, varnishtop acts as if they both were.  
`-c`     Include log entries which result from communication with a client.  If neither -b nor -c is specified, varnishtop acts as if they both were.  
`-i tag` Include log entries with the specified tag.  If neither -I nor -i is specified, all log entries are included.


### Examples  
` varnishtop -1 -c > /tmp/xxx`  
run once (-1) and display only records concerning the client/requester (-c).  

`varnishtop -i ReqURL`  
live view of the most requested URL's  

`varnishtop -I ReqHeader:Accept-Language`  
live view of the client browsers language settings

`varnishtop -I ReqHeader:User-Agent.*Googlebot`  
show User-Agents containing the string "Googlebot"


## [varnishlog](https://varnish-cache.org/docs/4.1/reference/varnishlog.html?highlight=varnishlog)

### SYNOPSIS  
```
V3:  
varnishlog [-a] [-b] [-C] [-c] [-D] [-d] [-I regex] [-i tag] [-k keep] [-n varnish_name] [-o] [-O] [-m tag:regex ...] [-P file] [-r file] [-s num] [-u] [-V] [-w file] [-X regex] [-x tag]
```

`-I regex`
       Include log entries which match the specified regular expression.  If neither -I nor -i is specified, all log entries are included.

`-i tag` Include log entries with the specified tag.  If neither -I nor -i is specified, all log entries are included.

`-m tag:regex` only list transactions where tag matches regex. Multiple -m options are AND-ed together.  Can not be combined with -O



### Examples  
`varnishlog -q "ReqHeader ~ 'X-Forwarded-For: 10.10.40.40'"`  //invalid 'q' ?  
analyze traffic coming from my the IP address (10.10.40.40)

`varnishlog -q "RespHeader eq 'X-Cache: MISS' and RespStatus != 200"`  
non-cached traffic (X-Cache: MISS) which don't receive a HTTP 200 status as response

3.x:  
```
Filter on IPs:
# varnishlog -c -m ReqStart:$IP
# varnishlog -c -m ReqStart:10.0.1.5
# varnishlog -c -m ReqStart:2a03:2880:10:cf07:face:b00c::1

To see the backend requests, match on the TxHeader:
# varnishlog -b -m TxHeader:$IP
# varnishlog -b -m TxHeader:10.0.1.5
# varnishlog -b -m TxHeader:2a03:2880:10:cf07:face:b00c::1

Filter on an X-Forwarded-For header:  
# varnishlog -c -m RxHeader:$IP
# varnishlog -b -m RxHeader:10.0.1.5
# varnishlog -b -m RxHeader:2a03:2880:10:cf07:face:b00c::1
```

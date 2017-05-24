
## Web Storage Test  
http://dev-test.nemikor.com/web-storage/support-test/  
> This page will test whether your browser supports localStorage, sessionStorage or globalStorage.
> The tests will also determine if there is a storage limit for any storage system that is supported.


## localStorage and sessionStorage
- sessionStorage  
Maintains a separate storage area for each given origin that is available for the duration of the page session.

- localStorage  
Similar to sessionStorage but persists even when the browser is closed and reopened. It comes in handy for transferring data between windows or tabs, if that's required.


## Storing, Reading and Removing Data
- setItem()
```
# The key must be of type string, but the type of value may vary
let localStorage = window.localStorage;
localStorage.setItem(“key1”, “value1”);
let sessionStorage = window.sessionStorage;
sessionStorage.setItem(“key2”, “value2”);
```

- getItem()
```
let string1 = localStorage.getItem("key1");
let string2 = sessionStorage.getItem("key2");
```

- removeItem()
```
localStorage.removeItem("key1");
sessionStorage.removeItem("key2");
```

- clear()
```
# Remove all data
localStorage.clear();
sessionStorage.clear();
```


## References
- [The Beginner’s Guide to Web Storage API and Related Tools](https://dzone.com/articles/the-beginners-guide-to-web-storage-api-and-related)
- [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- [HTML Living Standard - 11 Web Storage](https://html.spec.whatwg.org/multipage/webstorage.html)

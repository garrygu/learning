## Promise
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

### Syntax
```
new Promise( /* executor */ function(resolve, reject) { ... } );
```
The executor function is executed immediately by the Promise implementation, passing resolve and reject functions (the executor is called before the Promise constructor even returns the created object).The resolve and reject functions, when called, resolve or reject the promise, respectively.  

The executor normally initiates some asynchronous work, and then, once that completes, either calls the resolve function to resolve the promise or else rejects it if an error occurred.  

### States
- `pending`: initial state, neither fulfilled nor rejected.
- `fulfilled`: meaning that the operation completed successfully.
- `rejected`: meaning that the operation failed.

### Methods
- `Promise.all(iterable)`  
Returns a promise that either fulfills when all of the promises in the iterable argument have fulfilled or rejects as soon as one of the promises in the iterable argument rejects.

- `Promise.prototype.catch(onRejected)`  
Appends a rejection handler callback to the promise, and returns a new promise resolving to the return value of the callback if it is called, or to its original fulfillment value if the promise is instead fulfilled.

- `Promise.prototype.then(onFulfilled, onRejected)`  
Appends fulfillment and rejection handlers to the promise, and returns a new promise resolving to the return value of the called handler, or to its original settled value if the promise was not handled.

- `Promise.race(iterable)`  
Returns a promise that fulfills or rejects as soon as one of the promises in the iterable fulfills or rejects, with the value or reason from that promise.

- `Promise.reject(reason)`  
Returns a Promise object that is rejected with the given reason.

- `Promise.resolve(value)`  
Returns a Promise object that is resolved with the given value.

## Articles
- [Mastering Async Await in Node.js](https://blog.risingstack.com/mastering-async-await-in-nodejs/)
- [Array utilities for ESnext async/await](https://github.com/developit/asyncro)

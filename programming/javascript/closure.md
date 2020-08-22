

* Is closure over a single variable or over the entire scope?  
https://twitter.com/getify/status/1228059706486337541

* [whatthefuck.is   ·   a closure](https://whatthefuck.is/closure)  
You have a closure when a function accesses variables defined outside of it.
```
function liveADay() {
  let food = 'cheese';
  function eat() {
    console.log(food + ' is good');
  }
  // Call eat after five seconds
  setTimeout(eat, 5000);
}
liveADay();
```
The JavaScript engine needs to keep the food variable from that particular liveADay() call available until eat has been called.

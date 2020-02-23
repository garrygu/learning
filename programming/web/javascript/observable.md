
import
```
import {Observable} from 'rxjs/Observable';  
import 'rxjs/add/operator/map';  
...
```  

.share  
.map  
.subscribe  
.filter

```
observable
  .map(val=>val*val)
  .subscribe(val=>console.log(val));

observable
  .filter(val=>val%2===0)
  .subscribe(...);
```

```
<button id='increment' value='1'>+1</button>
<button id='decrement' value='-1'>+1</button>

const incrementClicks$=Observable.fromEvent(document.getElementById('increment'),'click')
const decrementClicks$=Observable.fromEvent(document.getElementById('decrement'),'click')

const clicks$=Observable.merge(incrementClicks$,decrementClicks$);
const value$=clicks$.map((v:any)=>parseInt(v.target.value,10));

const total$=value$
  .scan((prev,next)=>prev+next,0)   //reduce
  //.subscribe(v=>console.log(v));

total$.subscribe(total=>document.getElemenyById('counter').innerText=total.toString());
```


## ref
- [What Is a Higher-Order Observable?](https://blogs.msmvps.com/deborahk/higher-order-observable/)

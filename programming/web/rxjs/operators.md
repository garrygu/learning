https://rxjs-dev.firebaseapp.com/guide/operators


## Pipeable Operators
> A Pipeable Operator is a function that takes an Observable as its input and returns another Observable. It is a pure operation: the previous Observable stays unmodified.

Syntax: `observableInstance.pipe(operator())`



## Creation Operators
> Which can be called as standalone functions to create a new Observable. For example: of(1, 2, 3) creates an observable that will emit 1, 2, and 3, one right after another.


##
### [debounceTime](https://rxjs-dev.firebaseapp.com/api/operators/debounceTime)
> Emits a value from the source Observable only after a particular time span has passed without another source emission.

**Example**:
```
import { fromEvent } from 'rxjs';
import { debounceTime } from 'rxjs/operators';

const clicks = fromEvent(document, 'click');
const result = clicks.pipe(debounceTime(1000));
result.subscribe(x => console.log(x));
```

### [distinctUntilChanged](https://rxjs-dev.firebaseapp.com/api/operators/distinctUntilChanged)
> Returns an Observable that emits all items emitted by the source Observable that are distinct by comparison from the previous item.

** Example **
```
import { of } from 'rxjs';
import { distinctUntilChanged } from 'rxjs/operators';

of(1, 1, 2, 2, 2, 1, 1, 2, 3, 3, 4).pipe(
    distinctUntilChanged(),
  )
  .subscribe(x => console.log(x)); // 1, 2, 1, 2, 3, 4
```

### [switchMap](https://rxjs-dev.firebaseapp.com/api/operators/switchMap)
> Projects each source value to an Observable which is merged in the output Observable, emitting values only from the most recently projected Observable.  

** Example **
```
import { fromEvent, interval } from 'rxjs';
import { switchMap } from 'rxjs/operators';

const clicks = fromEvent(document, 'click');
const result = clicks.pipe(switchMap((ev) => interval(1000)));
result.subscribe(x => console.log(x));
```


## Marble Diagrams
- [Understanding Marble Diagrams for Reactive Streams](https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5)
- [Interactive diagrams of Rx Observables](https://rxmarbles.com/)

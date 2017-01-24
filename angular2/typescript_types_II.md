## Union types
```
var path : string[]|string;
path = '/temp/log.xml';
path = ['/temp/log.xml', '/temp/errors.xml'];
```


## Type guards
We can examine the type of an expression at runtime by using the `typeof` or `instanceof` operators.
```
var x: any = { /* ... */ };
if(typeof x === 'string') {
  console.log(x.splice(3, 1)); // Error, 'splice' does not exist on 'string'
}
// x is still any
x.foo(); // OK
```
TypeScript will automatically assume that x must be a string and let us know that the splice method does not exist on the type string. This feature is known as **type guards**.


## Type alias
Type aliases can be declared using **type** keyword: 
```
type PrimitiveArray = Array<string|number|boolean>;
type MyNumber = number;
type NgScope = ng.IScope;
type Callback = () => void;
```
If you work as part of a large team, the indiscriminate creation of aliases can lead to maintainability problems.

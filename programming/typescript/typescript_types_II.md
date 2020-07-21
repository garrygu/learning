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


## Ambient declarations
Ambient declaration allows you to create a variable in your TypeScript code that will not be translated into JavaScript at compilation time.  
You can use the declare operator to create an ambient declaration.

```
interface ICustomConsole {
    log(arg : string) : void;
}
declare var customConsole : ICustomConsole;

customConsole.log("A log entry!"); // ok
```

TypeScript includes, by default, a file named lib.d.ts that provides interface declarations for the built-in JavaScript library as well as the DOM.

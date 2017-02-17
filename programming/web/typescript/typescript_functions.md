# Functions

## Function Declaration vs. Function Expression
**Function Declaration**
```
function greetNamed(name : string) : string {
  if(name) {
    return "Hi! " + name;
  }
}
```

** Function Expression**
```
var greetUnnamed = function(name : string) : string {
  if(name){
    return "Hi! " + name;
  }
}
```

The interpreter can evaluate a function declaration as it is being parsed. On the other hand, the function expression is part of an assignment and will not be evaluated until the assignment has been completed.

** Variable Hoisting **  
The main cause of the different behavior of these functions is a process known as variable hoisting.


## Functions with optional Parameters
```
function add(foo : number, bar : number, foobar? : number) : number {
  var result =  foo + bar;
  if(foobar !== undefined){
    result += foobar;
  }
  return result;
}
```

## Functions with default parameters
```
function add(foo : number, bar : number, foobar : number = 0) : number {
  return foo + bar + foobar;
}
```
Just like optional parameters, default parameters must be always located after any required parameters in the function's parameter list.

## Functions with rest parameters
A rest parameter allows users to pass as many parameters as they may need.  
A rest parameter must be of an array type.
```
function add(...foo : number[]) : number {
  var result = 0;
  for(var i = 0; i < foo.length; i++){
    result += foo[i];
  }
  return result;
}
```
To JavaScript:
```
function add() {
    var foo = [];
    for (var _i = 0; _i < arguments.length; _i++) {
        foo[_i] = arguments[_i];
    }
    var result = 0;
    for (var i = 0; i < foo.length; i++) {
        result += foo[i];
    }
    return result;
}
```
JavaScript functions have a built-in object called the **arguments** object:  
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments

Can avoiding using rest parameters by using an array as the only parameter of the function:
```
function add(foo : number[]) : number {
  var result = 0;
  for(var i = 0; i < foo.length; i++){
    result += foo[i];
  }
  return result;
}
```


## Function overloading
Multiple methods with the same name but a different number of parameters or types.  
Overload a function: specify all function signatures of a function, followed by a signature known as the implementation signature.   

```
function test(name: string) : string;    // overloaded signature
function test(age: number) : string;     // overloaded signature
function test(single: boolean) : string; // overloaded signature
function test(value: (string | number | boolean)) : string { // implementation signature
  switch(typeof value){
    case "string":
      return `My name is ${value}.`;
    case "number":
      return `I'm ${value} years old.`;
    case "boolean":
      return value ? "I'm single." : "I'm not single.";
    default:
      console.log("Invalid Operation!");
  }
}
```
To JavaScript:
```
function test(value) {
    switch (typeof value) {
        case "string":
            return "My name is " + value + ".";
        case "number":
            return "I'm " + value + " years old.";
        case "boolean":
            return value ? "I'm single." : "I'm not single.";
        default:
            console.log("Invalid Operation!");
    }
}
```

The **implementation** signature must be compatible with all the overloaded signatures, always be the last in the list, and take any or a union type as the type of its parameters.   

**Template Strings**: enclosed by the back-tick (` `), can contain placeholders indicated by the dollar sign and curly braces (${expression}).


## Specialized overloading signatures
Create multiple methods with the same name and number of parameters but a different return type.   

When we declare a specialized signature in an object, it must be assignable to at least one non-specialized signature in the same object.  

When writing overloaded declarations, we must list the non-specialized signature last.   

```
interface Document {
  createElement(tagName: "div"): HTMLDivElement; // specialized
  createElement(tagName: "span"): HTMLSpanElement;  // specialized
  createElement(tagName: "canvas"): HTMLCanvasElement; // specialized
  createElement(tagName: string): HTMLElement; // non-specialized
}
```

Can also use union types to create a method with the same name and number of parameters but a different type.


## Function scope
**lexical scoping**  - Use the structure of the program source code to determine what variables we are referring to (Used by the majority of modern programing languages including TypeScript).  
-  Variables are scoped to a block (a section of code delimited by curly braces {}) mostly
- n TypeScript (and JavaScript), variables are scoped to a function

**dynamic scoping** - Use the runtime state of the program stack to determine what variable we are referring to.

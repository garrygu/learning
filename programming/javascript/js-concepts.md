## Immediately invoked function expressions  
An immediately invoked function expression (IIFE) is a function that's executed as soon as it's created.   
An IIFE can also be described as a self-invoking anonymous function. Its most common usage is to limit the scope of a variable made via `var` or to encapsulate context to avoid name collisions.


## Scope
Scope is the `context environment` (also known as `lexical environment`) created when a function is written. This context defines what other data it has access to.  
https://medium.com/free-code-camp/deep-dive-into-scope-chains-and-closures-21ee18b71dd9

Identifier lookup works from the inside out and stops at the first match.

There are two types of scope:
- local
- global  
In JavaScript, if an identifier is not proceeded with a var, let, or const, it is implicitly declared in the global scope.

###  Execution Context
When function is invoked, it forms a new execution context.  

Two types of execution context:   
- global execution context
- function execution context

After execution context is formed:
- Hoisting
- An `Activation Object` is formed. This object holds all declared variables, functions, and parameters passed within that context (its scope or accessibility range).
- Create a hidden `[[scope]]` property (This hidden [[Scope]] is a property of the function, created at declaration, not invocation.)
- In the global execution context, a `Variable Object` is created, and if it is a function, it is an `Activation Object`. They are pretty much identical.

Each execution context has an associated variable object.


### Execution stack

### Scope Chain
The scope chain is created at function invocation and is based on where variables and/or blocks of code are written (lexical environment).


## Closures
A closure in JavaScript is an inner function that has access to its outer function's scope, even after the outer function has returned control. A closure makes the variables of the inner function private. Closures are used in event handling and callbacks.

[Example:](https://medium.com/free-code-camp/deep-dive-into-scope-chains-and-closures-21ee18b71dd9)
```
function firstName(first){
    function fullName(last){
        console.log(first + " " + last);
    }
    return fullName;
}
var name = firstName("Mister");
name("Smith") // Mister Smith
name("Jones"); //Mister Jones
```


## Prototypes
Every JavaScript function has a prototype property that is used to attach properties and methods.  
JavaScript supports inheritance only through the prototype property. In case of an inherited object, the prototype property points to the object’s parent.


## Private properties, using closures


## The Module pattern
The Module pattern is the most frequently used design pattern in JavaScript for achieving loosely coupled, well-structured code. It allows you to create public and private access levels.


## Hoisting
JavaScript moves variables and function declarations to the top of their scope before code execution. This is called hoisting.


## Currying
Currying is a method of making functions more flexible. With a curried function, you can pass all of the arguments that the function is expecting and get the result, or you can pass only a subset of arguments and receive a function back that waits for the rest of the arguments.


## The apply, call, and bind methods
The three functions are similar in that their first argument is always the `“this”` value, or context, that you want to give the function you are calling the method on.  
Of the three, `call` is the easiest. It's the same as invoking a function while specifying its context.   
`apply` is nearly the same as `call`. The only difference is that you pass arguments as an array and not separately.
The `bind` method allows you to pass arguments to a function without invoking it. A new function is returned with arguments bounded preceding any further arguments.


## Memoization
Memoization is an optimization technique that speeds up function execution by storing results of expensive operations and returning the cached results when the same set of inputs occur again. JavaScript objects behave like associative arrays, making it easy to implement memoization in JavaScript.


## Method overloading
Method overloading allows multiple methods to have the same name but different arguments. The compiler or interpreter determines which function to call based on the number of arguments passed.


## References
- [10 JavaScript concepts every Node.js programmer must master](http://www.infoworld.com/article/3196070/application-development/10-javascript-concepts-nodejs-programmers-must-master.html)

## Basic Types
- Boolean  
- Number  
All numbers in TypeScript are floating point values; supports decimal, hexadecimal, binary and octal

- String  
Uses double quotes (") or single quotes (') to surround string data

- Array  
```
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

- Tuple  
Tuple types allow you to express an array where the type of a fixed number of elements is known, but need not be the same.  
```
let x: [string, number];
x = ["hello", 10]; // OK
x = [10, "hello"]; // Error
```

- Enum  
```
enum Color {Red, Green, Blue};
let c: Color = Color.Green;
// or manually set all the values in the enum:
enum Color {Red = 1, Green = 2, Blue = 4};
let c: Color = Color.Green;
// look up the name of an enum value
enum Color {Red = 1, Green, Blue};
let colorName: string = Color[2];
```

- Any  
Let the values pass through compile-time checks
```
let notSure: any = 4;
let list: any[] = [1, true, "free"];
list[1] = 100;
```

- Void  
void is a little like the opposite of any: the absence of having any type at all.
```
function warnUser(): void {
    alert("This is my warning message");
}
// Declaring variables of type void is not useful because you can only assign undefined or null to them
let unusable: void = undefined;
```

- Null and Undefined  
By default null and undefined are subtypes of all other types. That means you can assign null and undefined to something like number.  
However, when using the **--strictNullChecks** flag, null and undefined are only assignable to void and their respective types. In cases where you want to pass in either a string or null or undefined, you can use the union type **string | null | undefined**.  

- Never  
The never type represents the type of values that never occur.  
The never type is a subtype of, and assignable to, every type; however, no type is a subtype of, or assignable to, never (except never itself). Even any isn’t assignable to never.
```
// Function returning never must have unreachable end point
function error(message: string): never {
    throw new Error(message);
}
function infiniteLoop(): never {
    while (true) {
    }
}
```

## Type assertions
*Type assertions* are a way to tell the compiler “trust me, I know what I’m doing.” It has no runtime impact, and is used purely by the compiler.
```
// angle-bracket” syntax:
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
// as-syntax: (when using TypeScript with JSX, only as-style assertions are allowed.)
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

## Variable Declarations
- var
  - Scoping rules  
  *var* declarations are accessible anywhere within their containing function, module, namespace, or global scope regardless of the containing block.  
  - Variable capturing

- let
  - Block-scoping (lexical-scoping)  
  Unlike variables declared with var whose scopes leak out to their containing function, block-scoped variables are not visible outside of their nearest containing block or for-loop.
  - [temporal dead zones](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)  
  You can’t access a variable before the let statement.
  - Re-declarations and Shadowing  
  The block-scoped variable needs to be declared within a distinctly different block when be declared with a function-scoped variable.
  ```
  function f(condition, x) {
    if (condition) {
        let x = 100;
        return x;
    }

    return x;
  }
  ```
  The act of introducing a new name in a more nested scope is called shadowing.
  ```
  function sumMatrix(matrix: number[][]) {
    let sum = 0;
    for (let i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (let i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }
  ```
  - Block-scoped variable capturing

- const  
Unless you take specific measures to avoid it, the internal state of a const variable is still modifiable. (TypeScript allows you to specify that members of an object are readonly.)
```
const kitty = {
    name: "Aurora",
    numLives: numLivesForCat,
}
kitty.name = "Rory";
kitty.name = "Kitty";
```

### [Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- Array destructuring
```
let input = [1, 2];
let [first, second] = input;
// swap variables
[first, second] = [second, first];
// and with parameters to a function
function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
// you can create a variable for the remaining items in a list using the syntax ...name
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // outputs 1
console.log(rest); // outputs [ 2, 3, 4 ]
```

- Object destructuring
```
let o = {
    a: "foo",
    b: 12,
    c: "bar"
}
let {a, b} = o;
// have assignment without declaration
// parentheses are used as JavaScript normally parses a { as the start of block
({a, b} = {a: "baz", b: 101});  
```
  - Property renaming  
  ```
  let {a: newName1, b: newName2} = o;
  // specify types
  let {a, b}: {a: string, b: number} = o;
  ```
  - Default values
  ```
  function keepWholeObject(wholeObject: {a: string, b?: number}) {
    let {a, b = 1001} = wholeObject;
  }
  ```

- Function declarations
```
type C = {a: string, b?: number}
function f({a, b}: C): void {
    // ...
}
//
function f({a, b} = {a: "", b: 0}): void {
    // ...
}
f(); // ok, default to {a: "", b: 0}
// give a default for optional properties on the destructured property instead of the main initializer
function f({a, b = 0} = {a: ""}): void {
    // ...
}
f({a: "yes"}) // ok, default b = 0
f() // ok, default to {a: ""}, which then defaults b = 0
f({}) // error, 'a' is required if you supply an argument
```

## Interfaces
### Optional Properties
```
interface SquareConfig {
    color?: string;
    width?: number;
}
//
function createSquare(config: SquareConfig): {color: string; area: number} {
    let newSquare = {color: "white", area: 100};
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}
//
let mySquare = createSquare({color: "black"});
```

### Readonly properties
```
interface Point {
    readonly x: number;
    readonly y: number;
}
```

*ReadonlyArray<T>* 
```
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
```

*readonly vs const*  
Variables use const whereas properties use readonly.

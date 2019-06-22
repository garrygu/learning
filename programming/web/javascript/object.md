

# Object
Only six things are not objects. They are — `null`,`undefined`, `strings`, `numbers`, `boolean`, and `symbols`. These are called primitive values or primitive types. Anything that is not a primitive value is an Object.

ECMA specification:
> An Object is logically a collection of properties.

## Object Operations
### Creation
- using “literal” notation
```
const obj = { }; // Creating an object using literal notation
```

- using Object constructor ([not recommended](https://stackoverflow.com/questions/4597926/what-is-the-difference-between-new-object-and-object-literal-notation))
```
const obj = new Object();
```

#### Computed Properties
```
const propertyName = 'firstName';
const obj = {
  [propertyName.toUpperCase()]: 'Alex',
}
```

### Addition (Adding properties to an object)
- Dot Notation
```
obj.address = 'Earth';
```
- Bracket Notation
```
obj["address"] = 'Earth' // Note the quotes
```

### Reading/Retrieving
By default, all objects have their parent as `Object.prototype`    
To get the parent (aka “prototype”) of an object:  `Object.getPrototypeOf`


### Existence
- in operator
```
'firstName' in obj;       // => true
'middleName' in obj;      // => false
'isPrototypeOf' in obj;   // => true
```  
Note: the entire prototype chain is consulted before true or false is returned.  

- hasOwnProperty  
Properties that exist only on the current object.
```
obj.hasOwnProperty('firstName');        // => true
obj.hasOwnProperty('middleName');       // => false
obj.hasOwnProperty('isPrototypeOf');    // => false
```
Ref: [Using hasOwnProperty as a property name](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty#Using_hasOwnProperty_as_a_property_name)


### Updation
### Deletion
```
delete obj.firstName;      // => true
delete obj['firstName'];   // => true
```

### Iteration (Enumeration)
- for-in loops
```
for (const property in obj) {
  const value = obj[property]; // Read the value
  console.log(property, value);
}
```
Notes:  
  1. for-in loop will return all string properties. It won’t give Symbol properties.
  2. The prototype chain is consulted, and all enumerable properties of parents are also returned.


- Object.keys()  
Only returns the object’s own keys in an array.  
```
const allProperties = Object.keys(obj);
```

- Object.values()  
- Object.entries()   
Get an array of [key, value] pairs.  
```
for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

- Reflect.ownKeys()  
Return both string-based and symbol properties.   


### Comparison
When you use the == or === operators, they only compare the references of the objects. References can be understood as “memory addresses” of the objects. Only primitives types are compared by values.  


### Copying  
When we use the = operator on an object, we merely copy its reference (shallow copy).   

Deep Copy:
- Object Spread Operator
```
const obj1 = {
  name: 'Alex',
  nestedObj: {
    address: 'Earth'
  }
}
const obj2 = {
  ...obj1      // Spreading the properties of obj1
}
```
Note:   
Only effective for the top-level primitive properties. nestedObj is still copied shallowly.  
Also, if the value of a property is a function then we can’t create a copy of function reliably in JavaScript. Hence, the functions/methods have to be shared.


- JSON.stringify  
Nested objects are also deeply copied. But keys whose values are undefined, or functions or a Symbol are skipped.
```
const obj2 = JSON.parse(JSON.stringify(obj));
```
[More Info](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Exceptions)


- Object.assign
It copies all the enumerable, own key-value pairs in the target from the source.  
Does not copy deeply on all levels and both String and Symbol properties are copied.
```
const obj2 = Object.assign({}, obj1);
```



## Type of Properties
### Data Properties
### Accessor Properties  
An accessor property is a combination of two functions: the `get` and the `set` function (getters and setters).

```
const accessorObj = {
  get name() {
    return 'Arfat';
  }
};  

accessorObj.name;
// => 'Arfat'
```


## Object Property Descriptors
### Property Attributes
Every key of an object contains a set of property attributes that define the characteristics of the value associated with the key.   
The set of property attributes is called the property descriptor.  

In total, there are six property attributes:  

| Attributes | Description     | Data Properties | Accessor Properties |
| :------------- | :------------- | | |
| [[Value]]      |       | x | |
| [[Get]] | | |x |
| [[Set]] | | |x |
| [[Writable]]  | boolean <br>whether we can overwrite the value or not| x | |
| [[Enumerable]]  | boolean <br>whether the property is going to appear in `for-in` loops or not|x | x|
| [[Configurable]] | boolean <br> When is false: 1) Unable to delete the property 2) Unable to convert a Data Property to Accessor Property, and vice-versa 3) Unable to change the attribute values 4) If data property, can only set `writable` from `true` to `false` 5) If data property, can change its `[[value]]` before `writable` becomes `false`. Once `writable` is false, and `configurable` is false too, the property becomes unwritable, undeletable and unchangeable.   | x |x|

Note:  
Double square brackets mark internal properties used by the ECMA specifications. These are properties that JS programmer cannot touch directly from within the code.

Example:  
![](https://cdn-images-1.medium.com/max/1600/1*iKJx57JU9sKdff-Os7upyA.png)


### Working with Descriptors
- Object.getOwnPropertyDescriptor  
Return either `undefined` or an object containing the descriptors.

- Object.defineProperty  
It’s a static method on Object that can define or modify a new property on a given object.
- Object.defineProperties


## Protecting Objects
- Object.preventExtensions  
- Object.isExtensible  
Prevent new properties from ever being added to an object


- Object.seal  
- Object.isSealed
  * It prevents new properties from being added.
  * It marks all existing properties as non-configurable.
  * Values of present properties can still be changed as long as they are writable.
  * In short, it prevents adding and/or removing properties.  


- Object.freeze
- Object.isFrozen
  * It seals the object using Object.seal
  * It also prevents modifying any existing properties at all.
  * It also prevents the descriptors from being changed as the object is already sealed.

Summary:  
![](https://cdn-images-1.medium.com/max/1600/1*z6mLOd1zFsXt3sMc3_1o-Q.png)

Note:  
- These methods deal only with the direct properties of the objects, do not modify nested objects.
- `const` doesn't prevent you from changing properties on objects, only from re-assigning another value to the const variable itself. ([Ref1](https://dev.to/remotesynth/confused-by-javascript-s-const-me-too-4dfo), [Ref2](https://web.archive.org/web/20151113135159/http://blog.getify.com/constantly-confusing-const))




## References
- [The Chronicles of JavaScript Objects](https://blog.bitsrc.io/the-chronicles-of-javascript-objects-2d6b9205cd66)
- [Diving Deeper in JavaScripts Objects](https://blog.bitsrc.io/diving-deeper-in-javascripts-objects-318b1e13dc12)
- [MDN: Object initializer
](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)

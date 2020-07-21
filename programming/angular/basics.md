# Angular 2 Concepts

> It has **web components** at the heart of its design and it harnesses the power of **Shadow DOM** to maximize the responsiveness of our web entities against state changes.


## Web components
Encompasses four technologies:  
- Templates
- Custom Elements
- Shadow DOM
- HTML Imports


## TypeScript
> Angular 2 applications can be coded in a wide variety of languages and syntaxes: ECMAScript 5, Dart, ECMAScript 6, TypeScript, or ECMAScript 7.  

> TypeScript is a typed superset of ECMAScript 6 (also known as ECMAScript 2015) that compiles to plain JavaScript and is widely supported by modern OSes. It features a sound object-oriented design and supports annotations, decorators, and type checking.


## Most Common Packages
- @angular/core  
Encompassing the backbone of Angular and its most common elements, such as directives and components.

- @angular/common  
Contains the definitions of all the directives, services, and pipes contained by Angular 2, among other relevant classes.

- @angular/compiler  
Responsible for compiling the HTML templates and turning them into code that can render the application's UI output.

- @angular/platform-browser  
Contains classes and functions required for composing and interacting with the DOM in a web browser context. Also contains the functions required to compile templates offline in production environments.

- @angular/platform-browser-dynamic

- @angular/http  
Angular 2 HTTP client

- @angular/router  
Angular 2 built-in router

- @angular/router-deprecated


## 3rd-party libraries
- **es6-shim**  
Introduces ECMAScript 6 compatibility polyfills for legacy JavaScript engines (mostly Microsoft Internet Explorer).

- zone.js  
A polyfill for the Zone specification that is used to handle change detection in Angular 2 applications.

- reflect-metadata  
Brings support for decorators in our Angular 2 classes and metadata reflection in our components. Decorators are a core part of Angular 2.

- rxjs  
A library (developed by Microsoft Open Technologies, Inc.) for managing Observables, which allow us to make our applications fully reactive to asynchronous state changes.

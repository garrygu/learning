# TypeScript
> https://www.typescriptlang.org  
> TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.  
  Any browser. Any host. Any OS. Open source.

## Installation
For NPM:  
`npm install -g typescript`

For Atom:  
`apm install atom-typescript`

## DefinitelyTyped
> https://github.com/DefinitelyTyped/DefinitelyTyped  
The repository for high quality TypeScript type definitions. http://definitelytyped.org/

### The TypeScript Definition Manager
- [typings](https://github.com/typings/typings)
  ```
  npm install typings --global
  typings search tape
  typings search --name react
  ```

  Sources:  
  ```
  npm - dependencies from NPM
github - dependencies directly from GitHub (E.g. Duo, JSPM)
bitbucket - dependencies directly from Bitbucket
bower - dependencies from Bower
common - "standard" libraries without a known "source"
shared - shared library functionality
lib - shared environment functionality (mirror of shared) (--global)
env - environments (E.g. atom, electron) (--global)
global - global (window.<var>) libraries (--global)
dt - typings from DefinitelyTyped (usually --global)
  ```
- [tsd](https://github.com/DefinitelyTyped/tsd) (deprecated)
- [TypeSearch](https://microsoft.github.io/TypeSearch/)

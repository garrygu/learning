- [lite-server](https://github.com/johnpapa/lite-server)    
Lightweight node server  
`$ npm install -g lite-server`

- [json-server](https://github.com/typicode/json-server)  
Get a full fake REST API with zero coding  
`$ npm install -g json-server `

- [Angular in-memory-web-api](https://github.com/angular/in-memory-web-api)  
> An in-memory web api for Angular demos and tests that emulates CRUD operations over a RESTy API.  

  `$ npm i angular-in-memory-web-api -D`

  ```
  import { InMemoryDbService } from 'angular-in-memory-web-api';

  export class InMemHeroService implements InMemoryDbService {
    createDb() {
      let heroes = [
        { id: 1, name: 'Windstorm' },
        { id: 2, name: 'Bombasto' },
        { id: 3, name: 'Magneta' },
        { id: 4, name: 'Tornado' }
      ];
      return {heroes};
    }
  }
  ```
**Ref**:  
[Mocking with Angular: More than just unit testing](https://medium.com/@amcdnl/mocking-with-angular-more-than-just-unit-testing-cbb7908c9fcc)

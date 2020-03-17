The Angular DI framework manages dependencies for our Angular components, directives, pipes, and services.

## Hierarchical injectors
An injector in Angular is a dependency container that is responsible for storing dependencies and dispensing them when asked for.

### [Two injector hierarchies](https://angular.io/guide/hierarchical-dependency-injection#two-injector-hierarchies)
- **ModuleInjector** hierarchy- configure a ModuleInjector in this hierarchy using an @NgModule() or @Injectable() annotation.
- **ElementInjector** hierarchy- created implicitly at each DOM element. An ElementInjector is empty by default unless you configure it in the providers property on @Directive() or @Component().


### [Configure an injector with a service provider](https://angular.io/guide/dependency-injection#configure-an-injector-with-a-service-provider)
You can configure injectors with providers at different levels of your app, by setting a metadata value in one of three places:
- In the @Injectable() decorator for the service itself.
- In the @NgModule() decorator for an NgModule.
- In the @Component() decorator for a component.

### Registering component level dependencies
There is a similar providers attribute on the @Component decorator that allows us to register dependency at the component level. Dependency registration done at the component level is available on the component and its descendants.

Dependencies are singleton in nature. Once created, the Injector will always return the same dependency every time.  This data sharing/interaction/data flow pattern can be used to share state between any number of components.

While dependencies registered with the injector are singleton, Injector itself is not! In fact, injectors are created in the same hierarchy as the component tree. Every component doesn't need its own injector. But it is true that every component has an injector (even if it shares that injector with another component) and there may be many different injector instances operating at different levels of the component tree. (It is useful to pretend that every component has its own injector.)

https://angular.io/guide/dependency-injection#injector-hierarchy-and-service-instances  
Whenever Angular creates a new instance of a component that has providers specified in @Component(), it also creates a new child injector for that instance. Similarly, when a new NgModule is lazy-loaded at run time, Angular can create an injector for it with its own providers.

A component's injector is a child of its parent component's injector, and inherits from all ancestor injectors all the way back to the application's root injector.


#### [Providing services in @Component()](https://angular.io/guide/hierarchical-dependency-injection#providing-services-in-component)
- with a **providers** array
```
@Component({
  ...
  providers: [
    {provide: FlowerService, useValue: {emoji: '🌺'}}
  ]
})
```

- with a **viewProviders** array
```
@Component({
  ...
  viewProviders: [
    {provide: AnimalService, useValue: {emoji: '🐶'}}
  ]
})
```


### Angular DI dependency walk
 If it fails to find the requested dependency, it queries the parent component injector for the dependency, and its parent if the probing fails again, and so on and so forth till it finds the dependency or reaches the root injector. The takeaway: any dependency search is hierarchy-based.


## Dependency injection with @Injectable

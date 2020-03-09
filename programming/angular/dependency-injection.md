The Angular DI framework manages dependencies for our Angular components, directives, pipes, and services.

## Hierarchical injectors
An injector in Angular is a dependency container that is responsible for storing dependencies and dispensing them when asked for.


### Registering component level dependencies
There is a similar providers attribute on the @Component decorator that allows us to register dependency at the component level. Dependency registration done at the component level is available on the component and its descendants.

Dependencies are singleton in nature. Once created, the Injector will always return the same dependency every time.  This data sharing/interaction/data flow pattern can be used to share state between any number of components.

While dependencies registered with the injector are singleton, Injector itself is not! In fact, injectors are created in the same hierarchy as the component tree. Every component doesn't need its own injector. But it is true that every component has an injector (even if it shares that injector with another component) and there may be many different injector instances operating at different levels of the component tree. (It is useful to pretend that every component has its own injector.)


### Angular DI dependency walk
 If it fails to find the requested dependency, it queries the parent component injector for the dependency, and its parent if the probing fails again, and so on and so forth till it finds the dependency or reaches the root injector. The takeaway: any dependency search is hierarchy-based.


 ## Dependency injection with @Injectable

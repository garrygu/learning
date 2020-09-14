# Four major framework pieces
- The Router (Router)
- Routing configuration (Route)
- RouterOutlet component
- RouterLink directive


# Routing setup
## Generate an app with routing enabled (using the CLI)
`ng new routing-app --routing`

## Manually Setup
Add a package reference to the router in `package.json`:  
```
"@angular/platform-browser-dynamic": "2.0.0",
"@angular/router": "3.0.0",
```

Reference the package in `systemjs.config.js` to allows **SystemJS** to load the router module:  
```
var ngPackageNames = [
...
    'router',
...];
```

Add the `base` reference to the `head` section of `index.html`:
```
<base href="/">
```

Import the `AppRoutingModule` into `AppModule` and add it to the `imports` array:  
```
import { AppRoutingModule } from './app-routing.module'; // CLI imports AppRoutingModule

@NgModule({
  ...
  imports: [
    BrowserModule,
    AppRoutingModule // CLI adds AppRoutingModule to the AppModule's imports array
  ],
  ...
})
```

## Setup routes in `AppRoutingModule`
```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router'; // CLI imports router

import { FirstComponent } from './first/first.component';
import { SecondComponent } from './second/second.component';

// sets up routes constant where you define your routes
const routes: Routes = [
  { path: 'first-component', component: FirstComponent },
  { path: 'second-component', component: SecondComponent },
  { path: '',   redirectTo: '/first-component', pathMatch: 'full' }, // redirect to `first-component`  
  { path: '**', component: PageNotFoundComponent }, // wildcard routes
];

// configures NgModule imports and exports
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## Add routes to application
```
<nav>
  <ul>
    <li><a routerLink="/first-component" routerLinkActive="active">First Component</a></li>
    <li><a routerLink="/second-component" routerLinkActive="active">Second Component</a></li>
  </ul>
</nav>
<!-- The routed views render in the <router-outlet>-->
<router-outlet></router-outlet>
```

## Nesting routes
```
const routes: Routes = [
  {
    path: 'first-component',
    component: FirstComponent, // this is the component with the <router-outlet> in the template
    children: [
      {
        path: 'child-a', // child route path
        component: ChildAComponent, // child route component that the router renders
      },
      {
        path: 'child-b',
        component: ChildBComponent, // another child route component that the router renders
      },
    ],
  },
];
```

Template file:  
```
<h2>First Component</h2>

<nav>
  <ul>
    <li><a routerLink="child-a">Child A</a></li>
    <li><a routerLink="child-b">Child B</a></li>
  </ul>
</nav>

<router-outlet></router-outlet>
```

## Relative paths
Template:
```
<li><a routerLink="../second-component">Relative Route to second component</a></li>
```

Specifying a relative route:  
```
goToItems() {
  this.router.navigate(['items'], { relativeTo: this.route });
}
```

# Pushstate API and server-side url-rewrites
The router uses the `pushstate` mechanism for URL navigation. The router intercepts any navigation events, loads the appropriate component view, and finally updates the browser URL. The request never goes to the server.  
> Modern HTML 5 browsers support `history.pushState`, a technique that changes a browser's location and history without triggering a server page request. The router can compose a "natural" URL that is indistinguishable from one that would otherwise require a page load.

**But what if we refresh the browser?**  
The Angular router cannot intercept the browser's refresh event, and hence a complete page refresh happens. In such a scenario, the server needs to respond to a resource request that only exists on the client side. A typical server response is to send the app host file (such as `index.html`) for any arbitrary request that may result in a 404 (Not Found) error. This is what we call **server url-rewrite**.  

Each server platform has a different mechanism to support url-rewrite.   

**Hash-based routing**  
The default routing option is pushState-based. To change it to hash-based routing, the route configuration for the top level routes changes during route setup as shown in this example:
```
export const routing: ModuleWithProviders = RouterModule.forRoot(routes, { useHash: true } );
```

# Getting route information
## Using the [ActivatedRoute](https://angular.io/api/router/ActivatedRoute) service to access route params
```
import { Router, ActivatedRoute, ParamMap } from '@angular/router';

// Inject an instance of ActivatedRoute
constructor(
  private route: ActivatedRoute,
) {}

ngOnInit() {
  this.route.queryParams.subscribe(params => {
    this.name = params['name'];
  });
}
```
- [Router](https://angular.io/api/router)  
- [ActivatedRoute](https://angular.io/api/router/ActivatedRoute)  
- [ParamMap](https://angular.io/api/router/ParamMap)

In the component you want to navigate from:  
```
import { ActivatedRoute } from '@angular/router';
import { Observable } from 'rxjs';
import { switchMap } from 'rxjs/operators';

heroes$: Observable;
...
ngOnInit() {
  this.heroes$ = this.route.paramMap.pipe(
    switchMap(params => {
      this.selectedId = Number(params.get('id'));
      return this.service.getHeroes();
    })
  );
}
```

In the component that you want to navigate to:  
```
ngOnInit() {
  const heroId = this.route.snapshot.paramMap.get('id');
  this.hero$ = this.service.getHero(heroId);
}
```

# [Lazy loading](https://angular.io/guide/lazy-loading-ngmodules)


# References
- https://angular.io/guide/router

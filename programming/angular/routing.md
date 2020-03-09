## Four major framework pieces
- The Router (Router)
- Routing configuration (Route)
- RouterOutlet component
- RouterLink directive


### Angular Router
#### Routing setup
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

#### Pushstate API and server-side url-rewrites
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

#### Using the ActivatedRoute service to access route params

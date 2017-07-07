- Strongly layered product architecture
  - This supports the separation of visual presentation from business logic.
  - Magento tweaks the classic Model-View-Controller architectural model, though: files within modules are typically grouped by functionality rather than file type.


## Magento Framework


## Magento component
- module
- theme
- language package

### Module
A module is a logical group – that is, a directory containing blocks, controllers, helpers, models – that are related to a specific business feature.  

The purpose of each module is to provide specific product features by implementing new functionality or extending the functionality of other modules.

Each module is designed to function independently.

#### Where do modules live?
`app/code/<Vendor>/<ModuleName>`  
For example, the Customer module of Magento can be found at `app/code/Magento/Customer`.  

If you are creating a new module for distribution, you can just create the `<ModuleName>` directory and the required directories within it.

#### Areas
An `area` is a logical component that organizes code for optimized request processing.  

**Magento is organized into these main areas:**  
- Magento Admin (adminhtml)  
The `/app/design/adminhtml` directory contains all the code for components you’ll see while working in the Admin panel.
- Storefront (frontend)  
- Basic (base)  
Used as a fallback for files absent in adminhtml and frontend areas.

#### Modules and areas
- Modules define which resources are visible and accessible in an area, as well as an area’s behavior.  
- The same module can influence several areas. For instance, the RMA module is represented partly in the adminhtml area and partly in the frontend area.
- Each area declares itself within a module.
- You can enable or disable an area within a module.
- Areas are registered in the Dependency Injection framework `di.xml` file.

#### Note about Magento request processing
- Magento processes a URL request by first stripping off the base URL.
- The first path segment of the remaining URL identifies the request area.
- After the area name, the URI segment specifies the full front name.
- When an HTTP request arrives, the handle is extracted from the URL. Magento uses the handle to identify which controller (a PHP class) and action (a PHP method in the class) to execute.

#### Module conventions
Modules must conform to Magento conventions regarding **code location** and **file names**.  
Refer to: [Coding Standards](http://devdocs.magento.com/guides/v2.0/coding-standards/bk-coding-standards.html)

- Module location conventions  
A module must include a `registration.php` file in its root folder.
```
Entity	                           Location
Code base of your custom module      /app/code/<Vendor>/<Module>
Custom theme files (storefront)	  /app/design/frontend/<Vendor>/<theme>
Custom theme files (modules)	     <Module>/<theme>
If you want to use a library	     /lib/<Vendor_Library>
```

#### Module relationships
- uses
- reacts to
- customizes
- implements
- replaces

#### Managing module dependencies
At a high level, there are three main steps for managing module dependencies:
- Name and declare the module in the module.xml file.
- Declare any dependencies that the module has in the module’s `composer.json` file.
- (Optional) Define the desired load order of config files and .css files in the module.xml file.


## Magento libraries
- Magento PHP libraries
- [Magento UI libraries](https://github.com/magento/magento2/blob/2.0/lib/web/css/docs/source/README.md) (located in the `lib/web` directory)
- Third-party libraries

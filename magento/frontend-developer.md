# Rendering flow
The Magento application entry point is its `index.php` file. All of the HTTP requests go through it.


# View elements
## Ui components
The full set of Ui components is located under the `vendor/magento/framework/View/Element/` directory.  
The components themselves are located under the `vendor/magento/module-ui/Component/` directory.  
Components implement `UiComponentInterface`, which is defined under the `vendor/magento/framework/View/Element/UiComponentInterface.php` file


## Containers
Container renders all of its children automatically.  
We cannot create instances of containers because they are an abstract concept, whereas we can create instances of blocks.  
Containers are rendered via the `_renderContainer` method of the `Magento\Framework\View\Layout` class  


## Blocks
Although containers determine the layout of the page, they do not contain actual content directly. Pieces that contain the content and are nested within containers are called blocks.  
Each block can contain any number of child content blocks or child containers.  
Layout defines a sequence of blocks on the page, not their location.  
Templates are the thing that actually draw elements within a page; blocks are the thing that contain the data. In other words, templates are PHTML or HTML files pulling data through variables or methods sent on a linked PHP block class.    

In a nutshell, each container has blocks assigned to it. Each block usually (but not always) renders a template. The template itself may or may not call other blocks, and so on. Blocks are rendered when they are called from the template.  


## Block architecture and life cycle
Blocks extend from the `Magento\Framework\View\Element\AbstractBlock` class and implement `Magento\Framework\View\Element\BlockInterface`.

The most important block types are:  
- Magento\Framework\View\Element\Text
- Magento\Framework\View\Element\Text\ListText
- Magento\Framework\View\Element\Messages
- Magento\Framework\View\Element\Template


## Templates
Templates are snippets of HTML mixed with PHP.   
Magento uses the PHTML file extension for template files.  
Templates are located under an individual module's view/{_area_}/templates/ directory.  

## Layouts
It enables us to describe the page layout and content placement.
Two types of layouts:
- ayout: XML wrapped in `<layout>`  
- page: XML wrapped in `<page>`  

`Page` layouts represent a full page in HTML, whereas `layout` layouts represent a part of a page. Both types of layout XML files are validated by the XSD schema found under the `vendor/magento/framework/View/Layout/etc/` directory:
- layout – layout_generic.xsd
- page – page_configuration.xsd  

Based on the application components that provide <layout> and <page> elements , we can further section them as base and theme layouts.  
The base layouts are provided by the modules, usually at the following locations:  
- <module_dir>/view/frontend/layout: page configuration and generic layout files
- <module_dir>/view/frontend/page_layout: page layout files  

The theme layouts are provided by the themes, usually at the following locations:
- `<theme_dir>/<Namespace>_<Module>/layout`: page configuration and generic layout files
- `<theme_dir>/<Namespace>_<Module>/page_layout`: page layout files


## Themes

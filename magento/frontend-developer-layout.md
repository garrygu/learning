http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/layouts/layout-overview.html

Functionally, layout is a page structure, represented by hierarchy of elements (element tree), which can be of two types: **blocks** and **containers**.   
Technically, layout is defined in the ·`xml` files, which contain element declarations and element manipulation instructions.

## Terms
- Layout handle  
A layout handle is a uniquely identified set of layout instructions that serves as a name of a layout file.  

- Three kinds of layout handles:
  - page type layout handles: for example, catalog_product_view
  - page layout handles: for example, catalog_product_view_type_simple_id_128.
  - arbitrary handles



## Basic layout elements  
The basic components of Magento page design are **blocks** and **containers**.

- container
A container has no additional content except the content of included elements. Examples of containers include the header, left column, main column, and footer.
- block  
A block represents each feature on a page and employs templates to generate the HTML to insert into its parent structural block.


## Basic layouts
- `<Magento_Theme_module_dir>/view/frontend/layout/default.xml`: defines the page layout.
- `<Magento_Theme_module_dir>/view/frontend/layout/default_head_blocks.xml`: defines the scripts, images, and meta data included in pages’ <head> section.


# Layout instructions and types
## Layout instructions
http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/layouts/xml-instructions.html  
Two  ways to customize a layout on your Magento project. The first is to change the layout configuration files (XML); the second is to change the template files (phtml).

The layout configuration files have the following instructions in tags format and attributes:
- `<block>`: Refers to the specific block of content that can be in HTML format and uses templates (phtml) to render its contents
- `<container>`: Groups elements that can be blocks and even other containers
- `before` and `after` attributes: Attributes are used to define the display order of blocks and containers
- `<referenceBlock>` and `<referenceContainer>`: Allows you to create references to blocks that you have already created earlier, even if the blocks belongs to other layout configuration files
- `<move>`: Allows you to move a block or container inside another block or container
- `<remove>`: Removes static elements, such as CSS and JS, particularly header blocks or containers
- `<update>`: Includes a new template in the block or container
- `<argument>`: Makes the transfer of arguments to the template directly

Example:  
catalog_product_index.xml:  
```
<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <update handle="styles"/>
    <body>
        <referenceBlock name="menu">
            <action method="setActive">
                <argument name="itemId" xsi:type="string">
Magento_Catalog::catalog_products
</argument>
            </action>
        </referenceBlock>
        <referenceBlock name="page.title">
            <action method="setTitleClass">
             <argument name="class" xsi:type="string">
complex
</argument>
            </action>
        </referenceBlock>
        <referenceContainer name="content">
            <uiComponent name="product_listing"/>
            <block class="Magento\Catalog\Block\Adminhtml\Product" name="products_list"/>
        </referenceContainer>
    </body>
</page>
```

## Layout file types: by role  
For a particular page, its layout is defined by two major layout components: **page layout file** and **page configuration file** (or **generic layout** for pages returned in AJAX requests, emails, and so on).

- **Page layout**  
An XML file declaring a page wireframe inside the <body> section of the HTML page markup, for example, two-column page layout.
- **Page configuration**
An XML file declaring detailed structure, contents and meta-information of a page (includes the <html>, <head>, and <body> sections of the HTML page markup).
- **Generic layout**  
An XML file declaring page detailed structure and contents inside the body section of the HTML page markup. Used for pages returned by AJAX requests, emails, HTML snippets, and so on.


## Module and theme layout files  
The following terms are used to distinguish layouts provided by different application components:
- Base layouts: Layout files provided by modules.
  - Page configuration and generic layout files: `<module_dir>/view/frontend/layout`
  - Page layout files: `<module_dir>/view/frontend/page_layout`
- Theme layouts: Layout files provided by themes.
  - Page configuration and generic layout files: `<theme_dir>/<Namespace>_<Module>/layout`
  - Page layout files: `<theme_dir>/<Namespace>_<Module>/page_layout`


## Customize layout  
To make the necessary changes, create **extending** and **overriding** layout files in your custom theme.

###
http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/layouts/layout-extend.html  
When seeking punctual change, it is highly recommended to use the extend technique.  

### Override a layout
http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/layouts/layout-override.html  
When the changes are much deeper both in layout behavior and parameters, it is recommended to adopt the override technique.  


## Layout files processing
- Collects all layout files from modules. The order is determined by the modules order in the module list from `app/etc/config.php`.
- Determines the sequence of inherited themes `[<parent_theme>, ..., <parent1_theme>] <current_theme>`
- Iterates the sequence of themes from last ancestor to current
- Merges all layout files from the list

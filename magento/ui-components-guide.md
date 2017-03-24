## Overview
http://devdocs.magento.com/guides/v2.1/ui_comp_guide/bk-ui_comps.html

- Magento UI components are implemented as a standard module named `Magento_UI`.
- To use UI components in your custom module, add a dependency for the `Magento_UI` module in your component’s `composer.json` file.
- This XSD file contains rules and limitations shared between all components (both definitions and instance configurations):  
`<your module root dir>/Magento/Ui/etc/ui_definition.xsd`

### General structure
In Magento 2 there are basic and secondary UI components:
- Basic components (declared in the page layout files)
  - Listing component
  - Form component
- All other UI components are secondary (declared in the top-level components’ instances configuration files)

### What is a UI component?
UI component is a combination of:  
- **XML declaration** that specifies the component’s configuration settings and inner structure.
- **JavaScript** class inherited from one of the Magento JavaScript framework UI components base classes (such as UIElement, UIClass or UICollection).
- **Related template(s)**

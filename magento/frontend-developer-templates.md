http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/templates/template-overview.html

Templates define exactly how the content of layout blocks is presented on a page: order, CSS classes, elements grouping, and so on.

Default Magento templates are **PHTML** files. Also HTML templates are used for **Knockout JS scripts**.


## Template customization walkthrough

To customize a template:  
- Locate the template which is associated with the page/block you want to change using template hints.
- Copy the template to your theme folder according to the template storing convention.

To add a new template in a theme:
- Add a template in your theme directory according to the template storing convention.  
- Assign your template to a block in the corresponding layout file   
If you add a new .html template, and then edit it, the changes will not apply until you do the following: delete all files in the `pub/static/frontend` and `var/view_preprocessing` directories, then reload the pages. You can delete the files manually or run the `grunt clean:<theme_name>` command in CLI.


## How templates are initiated
Templates are usually initiated in layout files. Each layout block has an associated template.The template is specified in the `template` attribute of the layout instruction. For example:
```
<block class="Magento\Catalog\Block\Category\View" name="category.image" template="Magento_Catalog::category/image.phtml"/>
```

## Conventional templates location
- Module templates:   
`<module_dir>/view/frontend/templates/<path_to_templates>`
- Theme templates:   
`<theme_dir>/<Namespace>_<Module>/templates/<path_to_templates>`

Examples:
```
<Magento_Catalog_module_dir>/view/frontend/templates/product/widget/new/content/new_grid.phtml
<Magento_Checkout_module_dir>/view/frontend/templates/cart.phtml
```

## Root template
In Magento there’s a special template which serves as root template for all storefront pages in the application: `<Magento_Theme_module_dir>/view/base/templates/root.phtml`.


## Set a block’s template
http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/layouts/xml-manage.html#set_template

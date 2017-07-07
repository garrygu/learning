http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/bk-frontend-dev-guide.html

## Themes
http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/themes/theme-create.html

### Create a storefront theme
- Create a directory for the theme under `app/design/frontend/<your_vendor_name>/<your_theme_name>`.
- Add a declaration file `theme.xml` and optionally create `etc` directory and create a file named `view.xml` to the theme directory.
- Add a `composer.json` file.
- Add `registration.php`.
- Create directories for CSS, JavaScript, images, and fonts.
- Configure your theme in the Admin panel.

### Declare your theme `theme.xml`
```
<theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
     <title>New theme</title> <!-- your theme's name -->
     <parent>Magento/blank</parent> <!-- the parent theme, in case your theme inherits from an existing theme -->
     <media>
         <preview_image>media/preview.jpg</preview_image> <!-- the path to your theme's preview image -->
     </media>
 </theme>
```

### Make your theme a Composer package (optional)  
- Magento default themes are distributed as Composer packages.
- A default public packaging server is https://packagist.org/.
- Example of a theme `composer.json`:
```
{
    "name": "magento/theme-frontend-luma",
    "description": "N/A",
    "require": {
        "php": "~5.5.0|~5.6.0|~7.0.0",
        "magento/theme-frontend-blank": "100.0.*",
        "magento/framework": "100.0.*"
    },
    "type": "magento2-theme",
    "version": "100.0.1",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "autoload": {
        "files": [
            "registration.php"
        ]
    }
}
```

### Add registration.php
```
<?php
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::THEME,
    'frontend/<Vendor>/<theme>',
    __DIR__
);

# Example: Magento Luma theme
<?php
/**
 * Copyright © 2013-2017 Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::THEME,
    'frontend/Magento/luma',
    __DIR__
);
```

### Configure images
http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/themes/theme-images.html
- Product image sizes and other properties used on the storefront are configured in a `view.xml` configuration file. It is required for a theme, but is optional if exists in the parent theme.
- Create the `etc` directory in your theme folder
- Copy view.xml from the etc directory of an existing theme (for example, from the Blank theme) to your theme’s etc directory.
- Configure all storefront product image sizes in `view.xml`. For example:
```
...
    <image id="category_page_grid" type="small_image">
        <width>250</width>
        <height>250</height>
    </image>
...
```

### Create directories for static files
- Each type of static files should be stored in a separate sub-directory of `web` in your theme folder:
```
|-- web
| |-- css/
| | |-- source/
| |-- fonts/
| |-- images/
| |-- js/
```
- During theme development, when you change any files stored here, you need to clear `pub/static` and `var/view_preprocessed` directories, and then reload the pages

### Theme directory structure
```
app/design/frontend/<Vendor>/
├── <theme>/
│   ├── etc/
│   │   ├── view.xml
│   ├── web/
│   │   ├── images
│   │   │   ├── logo.svg
│   ├── registration.php
│   ├── theme.xml
│   ├── composer.json
```

### Declaring theme logo
- The default format and name of a logo image is `logo.svg`. When you put a logo.svg image in the `<theme_dir>/web/images` directory, it is automatically recognized as theme logo.
- You can use a logo file with a different name and format, but you might need to declare it.
- To declare a theme logo, add an extending `<theme_dir>/Magento_Theme/layout/default.xml` layout
```
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="logo">
            <arguments>
                <argument name="logo_file" xsi:type="string">images/my_logo.png</argument>
                <argument name="logo_img_width" xsi:type="number">300</argument>
                <argument name="logo_img_height" xsi:type="number">300</argument>
            </arguments>
        </referenceBlock>
    </body>
</page>
```

### Theme registration
Once you open the Magento Admin (or reload any Magento Admin page) having added the theme files to the files system, your theme gets registered and added to the database.

### Applying a theme
http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/themes/theme-apply.html

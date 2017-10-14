https://help.shopify.com/themes

A theme is a collection of files that control the layout and appearance of your website.

All themes come with build-in settings that include the ability to make changes to the font, the colors, the layout, and the content of your online store.

All themes include a `settings_schema.json` file in the theme's `config` directory that specifies the theme settings that appear on the `Customize theme` page.


# Theme file categories
- Layout  
These files control the layout of your theme.
- Templates  
These files control layouts for specific parts of your theme such as product or blog pages.
- Snippets
- Assets  
These files are images, fonts, scripts, and stylesheets.
- Config  
These files contain settings and configuration data for your theme.
- Locales  
These files contain language-specific pieces of text for your theme.
- Sections  
These files control the layouts of sections in your theme.


# Building Themes
## Getting Started
### Development store
### Tools
- [Slate](https://shopify.github.io/slate/)
- [Shopify Theme Kit](https://shopify.github.io/themekit/)

### Skillshare tutorials
- [Shopify Essentials for Web Developers](https://www.skillshare.com/classes/Shopify-Essentials-for-Web-Developers-From-Store-Setup-to-Custom-Themes/1070001866)
- [Advanced Shopify Theme Development](https://www.skillshare.com/classes/Advanced-Shopify-Theme-Development/708093439?via=similar-classes)

### Shopify Ajax API
- https://help.shopify.com/themes/development/getting-started/using-ajax-api  
- [Laboratory page](https://mayert-douglas4935.myshopify.com/pages/api)

### Shopify TextMate bundle
- https://github.com/Shopify/liquid-tmbundle


## Theme Editor
### Theme Sections
Sections support three new Liquid tags. These new tags are not usable outside of sections:  
{% schema %}  
{% javascript %}  
{% stylesheet %}  

#### Static and dynamic sections
When Shopify renders sections, it wraps each section in a <div> element with a unique id attribute:  
```
<div id="shopify-section-[id]" class="shopify-section">
  [output of the section template]
</div>
```

- Static sections  
`{% section 'header' %}` will include the section located at `sections/header.liquid`.  
A section can be included in multiple templates, but there exists only one instance of that section.   
Sections cannot include other sections.  

- Dynamic sections  
Sections can be dynamically added to the theme's homepage if they have `presets` defined in their respective `{% schema %}` tags.  
Shopify will render any sections configured in the theme editor in the `content_for_index` object that must be included in your theme's `index.liquid` template.  


#### Using section schema tags
Sections have a schema defined in the Liquid `{% schema %}` tag. Like a `comment` tag, `schema` does not output its content and Liquid code inside a `schema` tag is not executed.  

`schema` tags can be placed anywhere within a section file but cannot be nested inside another Liquid tag.  

**Properties:**  
- name  

- class  
You can specify additional classes for the section `<div>` in the schema:  
```
{% schema %}
  {
    "name": "Slideshow",
    "class": "slideshow"
  }
{% endschema %}
==>
<div id="shopify-section-[id]" class="shopify-section slideshow">
  [output of the section template]
</div>
```

- settings  
You can access setting values through the section object:  
```
{{ section.settings.header }}  
```
The global `settings` object (which contains the values of settings defined in `settings_schema.json`) is available within sections.  

  * To explicitly set which input will control the value of the section title, assign an input's id to title in the settings array.
  ```
  {% schema %}
    {
      "name": "Gallery",
      "settings": [
        {
          "id": "title",
          "type": "text",
          "label": "Title",
          "default": "Hello world"
        }
      ]
    }
  {% endschema %}
  ```

- blocks  
Blocks are containers of settings and content which can be added, removed, and reordered within a section.  
A block must have a `name` and a `type`. A block's `type` may be any value set by the theme developer. A block has `settings` in the same format as `settings_schema.json`.   

  * limit  
  The same block can be added to a section multiple times. `limit` is used to defined how many times it can be added.
  ```
  {% schema %}
    {
      "blocks": [
        {
          "type": "payment_icons",
          "name": "Payment Icons",
          "limit": 1
        }
      ]
    }
  {% endschema %}
  ```

- max_blocks  
Can specify a maximum number of blocks in the section schema:
```
{% schema %}
  {
    "name": "Slideshow",
    "max_blocks": 2
  }
{% endschema %}
```

- presets  
Section presets are default configurations of a section.  
When a section has one or more presets, each preset becomes a dynamic section a merchant can add to their theme homepage if the `content_for_index` object has been included in `index.liquid`.  
Presets must have a `name` and a `category`. Section presets will be grouped by their category in the theme editor.  
```
{% schema %}
  {
    "presets": [
      {
        "category": "Custom Content",
        "name": "Text",
        "settings": {
          "heading": "Hello World"
        },
        "blocks": [
          {
            "type": "text",
            "settings": {
              "content": "Once upon a time..."
            }
          }
        ]
      }
    ]
  }
{% endschema %}
```

- default  
Sections that are statically included in the theme's templates can define their default configuration.  
```
{% schema %}
  {
    "default": {
      "settings": {
        "heading": "Hello World"
      },
      "blocks": [
        {
          "type": "text",
          "settings": {
            "content": "Once upon a time..."
          }
        }
      ]
    }
  }
{% endschema %}
```

- locales  
Sections can use global translations defined in the locales directory.  
Sections can also define their own translations in their schema.  
```
{% schema %}
  {
    "locales": {
      "en": {
        "title": "Welcome"
      },
      "fr": {
        "title": "Bienvenue"
      }
    }
  }
{% endschema %}
```
When you update a translation in the language editor, your translations get saved to the applicable locale file and leave the schema unchanged. Translations inside a section's schema act as default values.  
In sections, the translate and localize filters will try to find a translation in the context of the current section, then in the section's schema, then in the global context.  


#### Rendering section blocks
Sections are responsible for rendering their blocks by looping over `section.blocks`.  
```
{% for block in section.blocks %}
  <div class="grid-item" {{ block.shopify_attributes }}>
    {% case block.type %}
    {% when 'text' %}
      {{ block.settings.content }}
    {% when 'image' %}
      <img src="{{ block.settings.image | img_url }}">
    {% endcase %}
  </div>
{% endfor %}
```


#### JavaScript and stylesheet tags
Sections can bundle their own script and style assets using the `javascript` and `stylesheet` tags.   
Each section can have one `javascript` tag and one `stylesheet` tag.   
The scripts for all sections are concatenated into a single file by Shopify and injected into `content_for_header`.


### Configuring theme settings
#### settings_schema.json
`settings_schema.json` controls the organization and content of the menu on the theme's Customize theme page.  

`settings_schema.json` lists all settings that are available for your theme, grouped into sections. The grouping of the settings in `settings_schema.json` is reflected in the `Customize theme` page menu.   

There are two categories of theme setting:  
- Input settings  
These control the settings that can be configured by merchants.  

- Sidebar settings  
They control informational elements (headers and paragraphs), which you can use to add detail and clarity to the `Customize theme` page's sidebar.  

##### Defining a theme setting
Each `settings` entry contains definitions for individual settings.  
Each individual setting has a few attributes, which vary depending on whether the setting is for `merchant input` or `sidebar styling`.  

##### Attributes for input settings
- type*		
Name of the type of settings.
- id*		
The unique name for this setting. The id is exposed to the liquid templates via the settings object. It must only contain alphanumeric characters, underscores, and dashes.
- label*		
You can create links in labels using the pattern `[link text](link URL)`.  
- placeholder		
A value for the input's placeholder text. This is for text-based setting types only.
- default		
- info  
Additional information about the setting. Use sparingly, as it's better to use only informative labels whenever you can.

##### Input setting types
**Basic input setting types**
- text
Single-line text fields
- textarea
Multi-line text areas
- image_picker
Image uploads
- radio
Radio buttons
- select
Selection drop-downs
- checkbox
- range
Range sliders

**Specialized input setting types**
- color
input:
```
{
   "type":      "color",
   "id":        "id",
   "label":     "Text",
   "default":   "value",
   "info":      "Text"
}
```
Example:
```
{
   "type": "color",
   "id": "background_color",
   "label": "Background color",
   "default": "#ffffff"
}
```
- font
- collection
Collections drop-down  
- product
Products drop-down  
- blog
- page
- link_list
"Link lists" are called "menus" in the Navigation page of the Shopify admin.  
- snippet
- url
- video_url
- richtext
- html
- article


##### Sidebar settings
Use these settings to add organizational information to your sidebar so that merchants can easily find the input settings they require.  
- type*		
- content*
- info


### Add theme info
#### Required theme info
The `theme_info` object must include the following:  
- "name": "theme_info"
- theme_name
- theme_author
- theme_version
- theme_documentation_url
- theme_support_email


#### Adding theme info to settings_schema.json
Theme info is stored in `settings_schema.json` as an object named `theme_info`:  
```
{
  "name": "theme_info",
  "theme_name": "Debut",
  "theme_author": "Shopify",
  "theme_version": "1.0.0",
  "theme_documentation_url": "https://help.shopify.com/manual/using-themes/themes-by-shopify/debut",
  "theme_support_email": "theme-support@shopify.com"
}
```

### Working with other theme files  
If you want to use theme setting values in  Liquid templates, CSS, and JavaScript files, you must:
- Append the appropriate `id` attribute to the settings object.
- Amend the filenames.

#### Appending the id attribute to the settings object
```
# A text input in settings_schema.json
{
  "type":      "text",
  "id":        "tagline",
  ...
},
# The value of this text input can be output using the following format in your Liquid templates:
{{ settings.tagline }}
```

#### Amending filenames of CSS or JavaScript files
If you want your CSS or JavaScript files to access the global `settings` object, you must append a `.liquid` extension to their file names.


####  Special-case user selections
- None
```
# in config/settings_data.json file:
"fp_page": "",
# To determine if the theme setting is empty, compare the appropriate settings attribute to blank as follows:
{% if settings.fp_page == blank %}
  <!-- The merchant does not want to add a page here. His fp_page theme setting was set to None. -->
{% endif %}
```
- Non-existent or hidden  
 A theme can't tell the difference between a hidden resource and one that does not exist.  
 ```
 {% assign front_page = settings.fp_page %}
{% unless pages[front_page] == empty %}
  <h1>{{ page.title }}</h1>
  <div>{{ page.content }}</div>
{% endunless %}
 ```


## Theme layouts
There is only ever one active layout on your store at any given time.  The active layout changes when your online visitors begin the checkout process.  

### theme.liquid
The `theme.liquid` file is the default active layout. It usually renders the header content, footer content, navigation, and other global variables.  

The `theme.liquid` can be thought of as the master template; all other templates are rendered inside of `theme.liquid`. Any elements that are repeated in a theme (ex: site navigations, header, footer, etc.) should be placed inside `theme.liquid`.

#### Template Considerations
##### content_for_header and content_for_layout
- Required
- The `{{ content_for_header }}` variable must be placed between the opening and closing `<head>` tag. It inserts the necessary Shopify scripts into the `<head>`.
- The `{{ content_for_layout }}` variable must be placed between the opening and closing `<body>` tag. It outputs dynamic content generated by all of the other templates (index.liquid, product.liquid, and so on).


### checkout.liquid (SHOPIFY PLUS)
This becomes the active layout during the checkout process.  


## Theme templates
### Theme structure
- assets
- config
- layout
- locales
- sections  
Reusable modules of content that can be customized and re-ordered by users of the theme.
- snippets
- templates

### Alternate templates
For example, you might want a sidebar on one product page but not in another. The workaround for this is to create alternate templates.  

There might be cases where you have several versions of a template and want to write an if statement that applies to all of them. The `template` object has several convenient attributes to help with this.  
```
{% if template.name == 'product' %}
We are on a product page!
{% endif %}

{% if template.suffix != blank %}
We are on an alternate template!
{% endif %}
```

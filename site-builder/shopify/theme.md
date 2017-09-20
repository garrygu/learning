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

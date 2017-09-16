https://help.shopify.com/themes/liquid

## Basics
### Tags
Tags are wrapped in: `{% %}` characters:
```
{% if user.name == 'elvis' %}
  Hey Elvis
{% endif %}
```

#### Control flow
- if  
```
{% if product.title == 'Awesome Shoes' %}
  You are buying some awesome shoes!
{% endif %}
```
- unless  
Executes a block of code only if a certain condition is not met (that is, if the result is false).
```
{% unless product.title == 'Awesome Shoes' %}
  You are not buying awesome shoes.
{% endunless %}
```
- else/elseif  

- case/when  
Creates a switch statement to execute a particular block of code when a variable has a specified value. case initializes the switch statement, and when statements define the various conditions.
```
{% case shipping_method.title %}
  {% when 'International Shipping' %}
     You're shipping internationally. Your order should arrive in 2–3 weeks.
  {% when 'Domestic Shipping' %}
    Your order should arrive in 3–4 days.
  {% when 'Local Pick-Up' %}
    Your order will be ready for pick-up tomorrow.
  {% else %}
     Thank you for your order!
{% endcase %}
```
- and/or  

#### Iteration
- **for**    
`for` loops can output a maximum of 50 results per page. In cases where there are more than 50 results, use the `paginate` tag to split them across multiple pages.   
https://help.shopify.com/themes/liquid/objects/for-loops  

  else  
  Specifies a fallback case for a `for` loop which will run if the loop has zero length.   
  ```
  {% for product in collection.products %}
    {{ product.title }}
  {% else %}
    The collection is empty.
  {% endfor %}
  ```
  break  
  Causes the loop to stop iterating.  
  ```
  {% for i in (1..5) %}
    {% if i == 4 %}
      {% break %}
    {% else %}
      {{ i }}
    {% endif %}
  {% endfor %}
  ```
  continue  
  Causes the loop to skip the current iteration.  
  ```
  {% for i in (1..5) %}
    {% if i == 4 %}
      {% continue %}
    {% else %}
      {{ i }}
    {% endif %}
  {% endfor %}
  ```

  `for` tag parameters
  - limit  
  Exits the for loop at a specific index.  
  ```
  {% for item in numbers limit:2 %}
    {{ item }}
  {% endfor %}
  ```
  - offset  
  Starts the `for` loop at a specific index.  
  - range  
  Defines a range of numbers to loop through. You can define the range using both literal and variable values.  
  ```
  {% assign my_limit = 4 %}
  {% for i in (1..my_limit) %}
  {{ i }}
  {% endfor %}
  ```
  - reversed  
  Reverses the order of the loop.  
  ```
  {% for item in array reversed %}
    {{ item }}
  {% endfor %}
  ```

- **cycle**  
Loops through a group of strings and outputs them in the order that they were passed as parameters.   

  Uses for `cycle` include:  
  - applying odd/even classes to rows in a table
  - applying a unique class to the last product thumbnail in a row  

  `cycle` tag parameters  
  `cycle` accepts a parameter called `cycle group` in cases where you need multiple cycle blocks in one template.  
  ```
  {% for product in collections.collection-1.products %}
    <li{% cycle 'group1': ' style="clear:both;"', '', '', ' class="last"' %}>
    ...
  ```

- **tablerow**  
Generates rows for an HTML table. Must be wrapped in an opening `<table>` and closing `</table>` HTML tags.  
https://help.shopify.com/themes/liquid/objects/tablerow  
```
<table>
  {% tablerow product in collection.products %}
    {{ product.title }}
  {% endtablerow %}
</table>
```

  `tablerow` tag parameters  
  - cols
  - limit
  - offset
  - range


#### Theme
- comment  
```
My name is Wilson Abercrombie{% comment %}, esquire{% endcomment %}.
```
- include  
Inserts a snippet from the `snippets` folder of a theme.
```
{% include 'my-snippet-file' %}
```

  Including multiple variables in a snippet:  
  ```
  {% assign my_variable = 'apples' %}
  {% assign my_second_variable = 'oranges' %}
  {% include 'snippet' %}
  ```
  or
  ```
  {% include 'snippet', my_variable: 'apples', my_other_variable: 'oranges' %}
  ```
  include tag parameters  
  - with  
  The with parameter assigns a value to a variable inside a snippet that shares the same name as the snippet.  
  ```
  color.liquid:
    color: '{{ color }}'
    shape: '{{ shape }}'
  theme.liquid:
    {% assign shape = 'circle' %}
    {% include 'color' %}
    {% include 'color' with 'red' %}
  ```

- form  
Creates one or more `<input>`s wrapped in a `<form>` element with all the attributes (such as action and id) required to submit the form.  

  form tag parameters
  - activate_customer_password
  - new_comment
  - contact
  - customer
  - create_customer
  - customer_address
  - customer_login
  - guest_login
  - recover_customer_password
  - reset_customer_password
  - storefront_password


- javascript  
Wraps scripts in a self-executing anonymous function and places them in a single file loaded in `content_for_header`. The `javascript` tag can only be used in section files.  

- layout  
Include `{% layout 'alternate' %}` at the beginning of a template file to use an alternate layout file from the `Layout` folder of your theme. If you don't define an alternate layout, the `theme.liquid` template file is used by default.  
```
# Load the layout/full-width.liquid template
{% layout 'full-width' %}
# If you want a template to use no layout file, you can specify none as the layout:
{% layout none %}
```

- paginate
```
{% paginate collection.products by 5 %}
  {% for product in collection.products %}
    <!--show product details here -->
  {% endfor %}
{% endpaginate %}
# The by parameter is followed by an integer between 1 and 50 that tells the paginate tag how many results it should output per page.
```

- raw  
Allows output of Liquid code on a page without being parsed.  

- schema  
The `schema` tag is used in section files to define the settings and properties of that section.  
The `schema` tags must contain valid JSON and cannot be nested inside another theme tag.

- section  
Inserts a section from the sections folder of a theme.  
```
{% section 'header' %}
# Including section files with the {% section %} tag will render a "static" section.
```

- stylesheet  
Concatenates styles into a single stylesheet loaded in content_for_header. The stylesheet tag is only to be used in section files.  

#### Variable
- assign
- capture  
Captures the string inside of the opening and closing tags and assigns it to a variable.

- increment  
Creates a new number variable, and increases its value by 1 every time increment is called on the variable. The counter's initial value is 0.  

- decrement


### Objects
### Filters

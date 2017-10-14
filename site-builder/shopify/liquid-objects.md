Liquid objects contain attributes to output dynamic content on the page.  
Liquid objects are also often referred to as Liquid variables.  

**Object handles**  
Handles are used to access the attributes of Liquid objects. By default, a handle is the object's title in lowercase with any spaces and special characters replaced by hyphens (-).

For example, a page with the title "About Us" can be accessed in Liquid via its handle about-us as shown below:  
```
<!-- the content of the About Us page -->  
{{ pages.about-us.content }}  
```


## Global objects
## Other objects

---
layout: post
title:  "Asset pipeline Rails is simply awesome"
date:   2014-04-24 
desc: "Asset is an additional file that gets called from the browser which includes javascript files,CSS files,images,videos.
"

---

###ASSET:

Asset is an additional file that gets called from the browser which includes javascript files,CSS files,images,videos.

###Why Asset Pipeline ?

e process used to do this is called Asset Pipeline.

Rails concatenates all javascript file and concatenates in to one big giant file using uglifier and minifier it removes all the spaces and push it to browser.

###MANIFEST FILES :

Rails needs to know which file to be included in application .Javascript manifest file is app/assets/javascripts/application.js

line starting with //= tells which file to be included
```sh
//= require jquery
//= require jquery_ujs
//= require turbolinks
//= require_tree 
```
+ require_tree helper method grabs everything in the current directory.

+ Your stylesheet manifest file operates on the same principle -- it's available at app/assets/stylesheets/application.css.scss:

###OVERRIDE CSS:

What if i want to override the behavior of .container class in specific page then
```sh
 <div class="user">
      <div class="container">
        
      </div>
    </div>
```
we can set like 
```sh
.user .container{
      // style stuff
    }
```

### USAGE OF IMAGES:

+ keep all the images under '/assets' directory and use image directory.
 <%=image_tag "xx.jpg" %>

###CONTROLLER SPECIFIC ASSETS:
You can also opt to include controller specific stylesheets and JavaScript files only in their respective controllers using the following:
```sh
<%= javascript_include_tag params[:controller] %> or <%= stylesheet_link_tag
params[:controller] %>
```

When doing this, ensure you are not using the require_tree directive, as that will result in your assets being included more than once.
#Backbone Breakdown

##A step by step guide to creating a backbone app accessing an API
* * *

### Files to Create 

1. Index.html 

   - Reference all backbone files you will use in `<script src='FILE_PATH_HERE'></script>` tags (in this order):
      a. Libraries referencing the proper file path (in this order): backbone-min.js, underscore-min.js, and jQuery-min.js
      b. Models, Collections, and Views
      c. The main app file which instantiates the AppView and binds it to the main app collection

2. App.js 
  
   - Instantiate a `var App = new AppView({ collection: new CollectionName() });` 

3. AppView.js
   - Specify the element to which you want to append the view
   `el: '.app',`



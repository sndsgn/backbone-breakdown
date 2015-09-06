#Backbone.js Breakdown

##A satisfactory step by step guide to creating a Backbone.js app
* * *

### Map out your architecture

1. Place your main app model and view at the top
2. On the left side place your collections and the models to which they are bound as sub elements
3. On the right side place your view and their subviews and controllers as sub elements
4. List your events and functions
5. Make lines between models and views indicating whether it is a binding, a function or an event to show relationships

### Files to Create 

#### Typical approaches to folder structure are by Backbone object type such as models, collections and view or by feature type such as ToDoList, Calendar, and TimeTracker

1. Index.html 

   - Reference all Backbone files you will use in `<script src='FILE_PATH_HERE'></script>` tags (in this order):
      a. Libraries referencing the proper file path (in this order): `backbone-min.js`, `underscore-min.js`, and `jQuery-min.js`
      b. Models, Collections, and Views
      c. The main app file which instantiates the AppView and binds it to the main app collection

2. App.js 
  
   - Instantiate the AppView and initialize and instantiate its collection 
```
         var App = new AppView({ 
           collection: new CollectionName() 
         }); 
```

3. AppView.js
   - Initialize and extend Backbone View object
         var AppView = Backbone.View.extend({  
   - Specify the element to which you want to append the view
         el: '.app',
   - Within an `*initialize*` method definition and assignment, instantiate child views, assign them as properties to the AppView object and pass in their collection to which they will be bound. At the end invoke the `render` method.
```
         initialize: function() {
           this.title = new TitleView();
           this.list = new ListView({collection: this.collection});
           this.otherViewElement = new OtherViewElementView({collection: this.collection});
           this.render();
         });
```





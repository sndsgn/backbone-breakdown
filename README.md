#Backbone.js Breakdown

##A satisfactory step by step guide to creating a Backbone.js app
* * *

### Map out your architecture

1. Place your main app model and view at the top
2. On the left side place your collections and the models to which they are bound as sub elements
3. On the right side place your view and their subviews and controllers as sub elements
4. List your events and functions
5. Make lines between models and views indicating whether it is a binding, a function or an event to show relationships

### Common errors
1. Use commas not `;` semicolons within the object you are passing to the extend method
2. If you don't define your `el` it will default to a `div` element generated by Backbone
3. `get()` and `set()` methods are for models

### Files to Create 

#### Typical approaches to folder structure are by Backbone object type such as models, collections and view or by feature type such as ToDoList, Calendar, and TimeTracker

1. Index.html 

   1. Reference all Backbone files you will use in `<script src='FILEorURL_PATH_HERE'></script>` tags (in this order):
      1. Libraries referencing the proper file path (in this order): `backbone-min.js`, `underscore-min.js`, and `jQuery-min.js`
      2. Models, Collections, and Views
      3. The main app file which instantiates the AppView and binds it to the main app collection

2. App.js 
  
   1. Instantiate the AppView and initialize and instantiate its collection 
   ```
       var App = new AppView({ 
         collection: new CollectionName() 
       }); 
   ```

3. AppView.js
   1. Initialize and extend Backbone View object
   `var AppView = Backbone.View.extend({`  
   2. Specify the element to which you want to append the view
   `el: '.app',`
   3. Within an `initialize` method definition and assignment, instantiate child views, assign them as properties to the AppView object and pass in their collection to which they will be bound. At the end invoke the `render` method.

   ```
       initialize: function() {
         this.title = new TitleView();
         this.list = new ListView({
           collection: this.collection
         });
         this.otherViewElement = new OtherViewElementView({ 
           collection: this.collection
         });
         this.render();
       }),
   ```

   4. Initialize the AppView `render` method, append the `el` defined earlier with each subview and return `this` which will be bound to the html elements representing this view that you will add to the DOM
   ```
       render: function() {
         this.$el.append([
           this.title.$el,
           this.list.$el
         ]);
         return this;
       }
   ```
        
4. ListEntry.js (the model) 
   1. Instantiate and extend the App model object with this object 
    ```
        var ListEntry = Backbone.Model.extend({
    ```
   2. Define your `defaults`
   ```
       defaults: {
         modelDefaultPropertyVal_001 = '',
         modelDefaultPropertyVal_002 = '',
         modelDefaultPropertyVal_003 = ''
         },
   ```
   3. Define your `initiatlize` function
   ```
       initialize: function() {},
   ```
   4. Define any model methods that will be invoked from some action by the user in the model's view or another view
   ```
       updateStatus : function() {
         var taskStatus = this.get('status');
         this.set({
           'taskStatus' : taskStatus
         });
       }
   ```
   5. Close your model initialization and `extend` method invocation
   ```
      });
   ```
      
           
 




   
        









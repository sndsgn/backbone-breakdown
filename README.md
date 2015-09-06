#Backbone.js Breakdown

##A satisfactory step by step guide to creating a Backbone.js app
####Based on the [HowsTheWeather App] (https://github.com/DannyDelott/HowsTheWeather) by [Danny Delott] (https://github.com/DannyDelott)
* * *

### Map out your architecture

1. Place your main app model and view at the top
2. Below on the left side place your collections and the models to which they are bound as sub elements
3. Below on the right side place your view and their subviews and controllers as sub elements
4. List your events and functions
5. Make lines between models and views indicating whether it is a binding, a function or an event to show relationships

### Common errors
1. Use commas not `;` semicolons within the object you are passing to the extend method
2. If you don't define your `el` it will default to a `div` element generated by Backbone
3. `get` and `set` methods are for models

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
         collection: new Weather() 
       }); 
   ```

3. AppView.js
   1. Initialize and extend Backbone View object
   `var AppView = Backbone.View.extend({`  
   2. Specify the element to which you want to append the view
   `el: '#app',`
   3. Within an `initialize` method definition and assignment, instantiate child views, assign them as properties to the AppView object and pass in their collection to which they will be bound. At the end invoke the `render` method.

   ```
       initialize: function() {

         this.title = new TitleView();

         this.input = new InputView({ 
           collection: this.collection
         });

         this.list = new ListView({
           collection: this.collection
         });

         this.render();
       }),
   ```
   4. Initialize the AppView `render` method, append the `el` defined earlier with each subview and return `this` which will be bound to the HTML elements representing this view that you will add to the DOM
   ```
       render: function() {

         this.$el.append([
           this.title.$el,
           this.input.$el,
           this.list.$el
         ]);

         return this;
       }
   ```
   5. Close your view initialization and `extend` method invocation
   ```
      });
   ```
        
4. WeatherEntry.js (the model - name the file whatever corresponds to your model) 
   1. Instantiate your model and extend the App's Backbone Model object with this object 
    ```
       var WeatherEntry = Backbone.Model.extend({
    ```
   2. Define your `defaults`
   ```
       defaults: {
         zipcode: '',
         city: '',
         weather: '',
         unit: '°F'
       },
   ```
   3. Define your `initiatlize` function
   ```
       initialize: function() {},
   ```
   4. Define any model methods that will be invoked from some action by the user in the model's view or another view
   ```
       toggleUnit : function() {
         var isImperial = this.get('unit') === '°F';
         if(isImperial) {
           var celcius = (this.get('weather') - 32) * (5/9);
           this.set({
             'unit': '°C',
             'weather' : celcius.toFixed(2)
           });
         } else {
           var fahrenheit = (this.get('weather') / (5 / 9)) + 32;
           this.set({
             'unit': '°F',
             'weather': fahrenheit.toFixed(2)
           });
         }
       }
   ```
   5. Close your model initialization and `extend` method invocation
   ```
      });
   ```
      
5. EntryView.js (the model view - name the file to correspond to your model) 
   1. Instantiate your model's view and extend the App's Backbone View object with this object 
    ```
        var EntryView = Backbone.View.extend({
    ```
   2. Define DOM element that you will bind to `el` and to which you will append HTML elements
   ```
       className: 'entry',
   ```
   3. Initialize template HTML that you will populate with data for each model view 
   ```
      template: _.template('<p>It is currently <%= weather %><%= unit %> in <%= city %>.</p>'),
   ```
   4. Specify which `events` the view should listen to that happen in its view and the name of the functions that should be invoked on the model as a result
   ```
      events: {
         'click' : 'clickAction'
      },
   ```
   5. Define your `initiatlize` function and specify which events the view should listen for on the model and render the view once those events have occurred. Also render the view on instantiation. 
   ```
       initialize: function() {
         this.listenTo(this.model, 'change', this.render);
         this.render();
       },
   ```
   6. Define your `render` method, the HTML it should append to the `el` using the `template` created earlier populating the variables `<%=VARIABLE%>` with data and invoke the `html` method on `el` with the HTML template you created
   ```
       render: function() {
         var entry = this.template({
           weather: this.model.get('weather'),
           unit: this.model.get('unit'),
           city: this.model.get('city')
         });
         this.$el.html(entry);
       },
   ```
   7. Define the function that should be invoked when the defined view event occurs
   ```
      clickAction: function() {
        this.model.toggleUnit();
      }
   ```
   8. Close your view initialization and `extend` method invocation
   ```
      });
   ```
 
6. InputView.js (the view associated with the input not tied to a model) 
   1. Instantiate your model's view and extend the App's Backbone View object with this object 
    ```
        var InputView = Backbone.View.extend({
    ```
   2. Define DOM element that you will bind to `el` and to which you will append HTML elements
   ```
       tagName: 'input',
        // Same as el: '<input>',
   ```
   3. Specify which `events` the view should listen to that happen in its view and the name of the functions that should be invoked on the model as a result
   ```
        events: {
          'keydown': 'keyAction',
        },
   ```
   4. Define your `initiatlize` function and invoke the render method to render the view
   ```
        initialize: function() {
          this.render();
        },
   ```
   5. Initialize the `render` method and invoke other functions required during re-render of the view 
   ```
        render: function() {
          this.resetInput();
          return this;
        },
   ```
   6. Define the functions that should be invoked when the defined view event occurs
   ```
        keyAction: function(e) {

          var isEnterKey = (e.which === 13);

          if(isEnterKey && !this.$el.val().trim().match(/^(?=.*[0-9].*)[0-9]{5}$/)) {

            this.$el.attr({
              placeholder: 'Sorry, zip code invalid.'
            });
            this.clearInput();

          } else if(isEnterKey) {

            this.collection.addWeatherEntry(this.$el.val());
            this.resetInput();

          }

        },

        resetInput: function() {
          this.$el.attr({
            placeholder: 'Enter a zip code'
          });
          this.clearInput();
        },

        clearInput: function() {
          this.$el.val('');
        }

   ```
   8. Close your view initialization and `extend` method invocation
   ```
      });
   ```
7. ListView.js (the view associated with the WeatherEntry model) 
   1. Instantiate your model's view and extend the App's Backbone View object with this object 
    ```
        var ListView = Backbone.View.extend({
    ```
   2. Define DOM element that you will bind to `el` and to which you will append HTML elements
   ```
       id: 'list',
   ```
   3. Define your `initiatlize` function and specify which events the view should listen for on the model and render the view once those events have occurred.
   ```
       initialize: function() {
         this.listenTo(this.model, 'add', this.render);
         //Same as this.collection.on('add', this.render, this);
       },
   ```
   4. Initialize your `render` method, run the `empty` method on the `el` element to clear it of child elements previously appended, create a property that instantiates an entry view for each model object in the collection, for each entry view created for each model create a jQuery `$els` that is an array that contains and initializes a jQuery element `$el` for each EntryView, then then append each of those jQuery DOM element to the DOM element defined as `$el`, and then adds those elements to the DOM
   ```
       render: function() {

        this.$el.empty();

        this.entries = this.collection.models.map(function(model) {
          return new EntryView({
            model: model
          });
        });

        var $els = this.entries.map(function(entry) {
          return entry.$el;
        });

        this.$el.append($els);

        return this;
      }
  ```
   5. Close your view initialization and `extend` method invocation
   ```
      });
   ```




   
        









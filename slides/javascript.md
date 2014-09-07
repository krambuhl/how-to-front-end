Javascript
===

##Logical Layers

It's generally a smart idea to seperate your project's components into different objects and files.  This helps break up complex logic up to manageable pieces, hopefully avoiding the spegetti code world.

- Global (External Libraries)
- System (Application)
- Page (Routes)
- Data (Collections/Models)
- Layouts (Views)
    + The layouts described here will likely be similar to the layouts defined in CSS.  
    + Examples: 
        *  Site Header
        *  Site Footer
        *  Article
- Modules (Views)
    + At this layer we describe the modules of the site, often definfing how individual components should operate.  
    + Examples: 
        * Hero Slider 
        * User Card
        * Dialog

##File Naming

Files should be broken up into small, maintainable sections; 1 component per file.  Use a tool like concat or file_include to combine all individual components into a single file.

In larger projects it is wise to categorize files into folders by type and share a global namespace.  

#####Examples

A model object named __ContactModel__ for storing contact data:
`contact-model.js` or `models/contact.js`

A view object named __UsercardView__ for handling UI events:
`usercard-view.js` or `views/usercard.js`

A general file with utility functions:
`utilities.js` or `shared/utilities.js`


##Variable Naming

Different types of variables should be written differently.

###Object Definitions & Namespaces

Object definitions are written in capitalized form, multiple word names have each word Capitalized.  These are usually constructors, any instances created from a definition should be named like any other variable.  Namespaces are also written in capitalized form.

#####Examples

- `MyObjectDefinition`
- `ApplicationView`
- `Namespace.AppRouter`
- `Namespace.ProfileView`


###Variables, Methods & Object Instances

Variables, Methods (both static and instance), and Object Instances should all be named in camelCase form.  

#####Examples

- `index`
- `tailsCount`
- `myObjectInstance`
- `myObjectInstance.navigate()`
- `MyDefinition.staticMethodCall()`


##Message Passing

As applications get bigger there offten is a need to communicate between the various logical layers and components.  It's bad practice to talk directly between modules; this is where message passing comes in.  

The jQuery on/off event listener api can work as a simple event event bus, but a seperate service on the Application layer is usually more portable.  

```js
    function HeaderView() {
        bus.trigger('header-loaded');
    }
    
    function ApplicationView() {
        bus.on('header-loaded', function() {
            // do some header logic now
        });

        this.header = new HeaderView();
    }

    var bus = new EventApi();
    var app = new ApplicationView();
```


- Jquery intro to Monads

`'superstring'.substr(0, 5).indexOf('per')` is not that different from `$('.articles').addClass('red').removeClass('blue')`

- Functional Programming
    - Seperate Procedure from Transformation
- Func. Programming Examples: 
    - Increment
    - Map/Reduce 
- Underscore's Additional Functional Specs
    - Where
    - Filter
    - Every
- Underscore or ES5?
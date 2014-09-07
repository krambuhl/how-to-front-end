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

####Examples

A model object named __ContactModel__ for storing contact data:
`contact-model.js` or `models/contact.js`

A view object named __UsercardView__ for handling UI events:
`usercard-view.js` or `views/usercard.js`

A general file with utility functions:
`utilities.js` or `shared/utilities.js`


##Variable Naming



- Variable Naming Conventions
    - Variables
    - Objects
    - Methods
    - Static Methods


- Message Passing
- Jquery intro to Monads
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
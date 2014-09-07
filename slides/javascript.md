Javascript
===

##Logical Layers

- Global
- Library
- System (Application)
- Page (Routes)
- Data (Collections/Models)
- Layout (View)
    + The layouts described here will likely be similar to the layouts defined in CSS.  
    + Examples: 
        *  Site Header
        *  Site Footer
        *  Article
- Modules (View)
    + At this layer we describe the modules of the site, often definfing how individual components should operate.  
    + Examples: 
        * Hero Slider 
        * User Card
        * Dialog

##File Naming

Files should be broken up into small, maintainable sections; 1 component to a file.  Use a tool like concat or file_include to combine all individual components into a single file.

In larger projects it is wise to categorize files into folders based on the logical layers defined above.

####Examples

- __contact-model.js__ or __models/contact.js__ -- A model object named `ContactModel` for storing contact data
- __usercard-view.js__ or __views/usercard.js__ -- A view object named `UsercardView` for handling UI events



- Variable Naming Conventions
    - Variables
    - Objects
    - Methods
    - Static Methods
- Logical Layering
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
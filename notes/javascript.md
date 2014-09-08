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

#####Javascript Example

```js
function Person(age) {
    this.age = age;
    this.firstName = 'JarJar';
    this.lastName = 'Binks';
}

Person.prototype.fullName = function() {
    return [this.firstName, this.lastName].join(' ');
};

var aPerson = new Person(19);
aPerson.age; // ==> 19
aPerson.fullName(); // ==> JarJar Binks
```


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


##Jquery intro to Chaining

`'astring'.substr(0, 5).indexOf('per')` is not that different from `$('.articles').addClass('is-important').attr('important', true)`.  This works by having each method return the related object instance.  Each method operates on wrapped datum, in the first case `substr` and `indexOf` are methods of the String prototype, in the second `addClass` and `attr` are methods of the jquery prototype.

#####Example

```js
function Calculator(number) {
    this.number = number;
}

Calculator.prototype.increment = function() {
    this.number++;
    return this;
};

Calculator.prototype.add = function(num) {
    this.number += num;
    return this;
};

Calculator.prototype.result = function() {
    return this.number;
}

var myCalc = new Calculator(20);

myCalc.result() // ==> 20
myCalc.increment().result() // ==> 21
myCalc.increment().add(8).result() // ==> 30
```


##Functional Programming

This type of chain usually takes form of a basic monadic form.  Monads are, at their simplest form, are a way to transform data.  The monadic form seperates transformational logic from procedural logic.

###Func. Programming Examples

All these examples will use this `Monad` class to demonstrate.  It takes data as argument to the constructor, then provides a `pipe` method to process data, and `result` to extract the transformed data.  

The `pipe` method accepts an function argument.  Each function passed to pipe should take data as an argument and return data on execution.

```js
function Monad(data) {
    this.startData = data;
    this.data = data;
}

Monad.prototype.pipe = function(process) {
    if (process) this.data = process(data);
    return this;
};

Monad.protype.result = function() {
    return this.data;
};
```

####Increment

```js
function increment(number) {
    return number + 1;
}

var myData = new Monad(10);

myData.pipe(increment).result(); // ==> 11

```

#####A Better Pipe

The monad pipe at this point is pretty simple, it could be vastly improved by accepting additional arguments..

```js
Monad.prototype.pipe = function(process) {
    var args = Array.prototype.slice.call(arguments, 1);
    if (process) this.data = process.apply(this, data.concat(args));
    return this;
};
```

####Increment 2.0

With the addition of arguments to the pipe we can make a single-use increment function work as an addition/subtraction function.  Multi-use: yumm.

```js
function increment(number, inc) {
    return number + (inc || 1);
}

var myData = new Monad(10);
myData.pipe(increment, 10).result(); // ==> 11

```

####Map/Reduce 

```js
// map
function mapAges(data) {
    data.map(function(person) {
        return person.age
    });
}

//reduce
function averageAges(data) {
    var total = data.reduce(function(age, total) {
        total += age;
        return total;
    }, 0);

    return total / data.length;
}

var people = [
    { name: 'Bob', age: 63, retired: true },
    { name: 'Scott', age: 63, retired: false },
    { name: 'Annie', age: 27, retired: false },
    { name: 'Barb', age: 60, retired: false }
];

var peopleData = new Monad(people);
var peopleData = new Monad(people);
myData.pipe(mapAges).pipe(averageAges).result(); // ==> 11

```

- Underscore's Additional Functional Specs
    - Where
    - Filter
    - Every
- Underscore or ES5?






















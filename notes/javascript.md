Javascript
===

Javascript is an easy language to jump into but as projects grow in size it can be a very unwieldy language.  My goal with this document is to outline best practices in writing reuseable, maintainable code that can simplify working in large teams.

##Conceptual Layers

It's a smart idea to seperate your project's components into different objects and files.  This helps break up complex logic up to manageable pieces, hopefully avoiding the spegetti code world.

Layer | Components | Examples
--- | --- | ---
Global | External Libraries | bower_components, jquery, underscore
System | Application, Namespace | AppView, Name.AppView
Page | Routes | AppRouter, ArticleRouter
Data | Collections, Models | ArticleCollection, ArticleModel, UserModel
Layouts | Views | SiteHeaderView, SiteFooterView, ArticleView
Modules | Views | HeroSliderView, UserCardView, DialogView

#####Global

Vendor libraries belong in the global namespace.  Store vendor libraries seperately from your application files.  Bower is a great way to do this and easily manage dependencies, but even just a seperate `vendor` folder is good.

#####System

Namespaces and top-level functions should be defined at the System level.  This is generally where the application or page startup should happen.

#####Page

Routers and content yielding are defined at the Page level.  They handle the URL and routing structure of your system/application.  Routes define what data and layouts should be yielded (displayed) on screen at any time.

#####Data

Data can just be json files, but this layer data is usually defined as Collections and Models.  Data is often used to fill out templates at the Layout/Modules level.

#####Layout

Define large sections of website's visual layout.  Layouts usually define how groups of individual modules are added to a section/

#####Modules

Describes individual components of a website.  Modules should be self-contained, any external logic should handled at a higher conceptual level.


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

##Object Oriented Concepts

Basic object oriented concepts can be seen in a seed [object] and tree [instance].  Objects should all have these properties:

- The type of the seed determines what type of tree will grow. [constructor]
- Each tree has an independent lifetime. [instance]
- Trees don't know about other trees. [information hiding]
- Trees can can drop seeds that are slightly different. [inheritence] 

#####Seed Example

```js
function Seed(type) {
    this.type = type;
    this.isGrown = false;

    this.grow = function() {
        this.isGrown = true;
    };
}

var maple = new Seed('maple');
var birch = new Seed('birch');
// maple.isGrown ==> false
// birch.isGrown ==> false

maple.grow();
// maple.isGrown ==> true
// birch.isGrown ==> false
```

Using protoype

```js
function GenericSeed(type) {
    this.type = type;
    this.isGrown = false;
}

GenericSeed.prototype.grow = function() { this.isGrown = true; };

function MapleSeed() {
    GenericSeed.prototype.constructor.apply(this, arguments);
    this.type = 'maple';
}

MapleSeed.prototype = GenericSeed;

var maple = new MapleSeed();
// maple.isGrown ==> false
maple.grow();
// maple.isGrown ==> true
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

`'astring'.substr(0, 5).indexOf('per')` is not that different from `$('.articles').addClass('is-important').attr('important', true)`.  

This works because each method returns the calling object instance.  Each method operates on wrapped datum. In the first case `substr` and `indexOf` are methods of the String prototype, in the second `addClass` and `attr` are methods of the jquery prototype.

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
};

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
    { name: 'Jeremy', age: 63, retired: true },
    { name: 'Scott', age: 63, retired: false },
    { name: 'Annie', age: 27, retired: false },
    { name: 'Barb', age: 60, retired: false }
];

var peopleData = new Monad(people);
peopleData.pipe(mapAges).pipe(averageAges).result(); // ==> 11

```

- Underscore's Additional Functional Specs
    - Where
    - Filter
    - Every
- Underscore or ES5?






















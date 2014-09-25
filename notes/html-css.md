CSS
===

CSS is a wonderful, flexible language for styling the web.  With all the tools CSS gives you, the need for structure your thoughts effectively becomes important.  This document will outline how to write and organize css in a modular, reusable fashion with an ephasis on naming wisely.

##Conceptual Layers

As your projects grow in size, seperating concerns and regonizing reusable patterns becomes paramount.  Breaking your CSS into modular chunks is a good idea, each module should serve a single purpose (similar to Object Oriented Programming).  A preprocessor like sass helps to concreate this pattern by confining each module to a partial file.

Layer  | Components | Names
--- | --- | ---
Includes | reset, variables, mixins
Standard Modules | list, link, input | .list, .link, .input
Component Modules | usercard, navigation, logo | .usercard, .nav, .logo
Layouts | profile, article, header | .l-profile, .l-article, .l-header
States | header-expanded | .is-header-expanded

####Includes

Includes are where global variables, mixins and configuration should be placed. This is where your css reset or normalize type styles should go as well.  Mixins should be written one to a file like modules.

####Standard Modules

Standard modules are the most reuseable; they define low-level elments and behavior like input fields and headings.  They are called standard modules because they are the starting point for any website.  If you find yourself creating the same component module for every project, it's aviseable to generalize said module and move it to standard modules.

#####Example

```scss
// filename: standard/_input.scss

.input {
    color: grey;
    &:focus { color: black; }
}
```

The big thing to notice is we are not overwriting the base `input` tag.  We are defining a class that can be used anywhere, later it can be extended more stragically.  It would make sense to extend the `input` tag in the `.form` standard class or another module instead.

```scss
// filename: standard/_form.scss

.form {
    input[type="text"] { @extend .input; }
    input[type="submit"] { @extend .buttom; }
}
```

On small websites it might make be reasonable to extend some base tags immediately, but the basic idea is to lower your impact.  An anchor `a` tag can be used for much more than a `.link`  The noise created through extending base tags and writing deep selectors makes large applications harder to maintain.

####Component Modules

Modules are the core of most appications, descibing custom components to be structured by layouts.  Modules should be writen one to file, so the `_nav.scss` file would contain the `.nav` module.  Modules should only care about what's inside it's root element.  A perfect module can work inside of a container, no matter it's size. 

#####Example

```scss
// filename: modules/_usercard.scss

.usercard {}
.usercard-image { @extend .image; }
.usercard-name {}
```

```html
<div class="usercard">
    <img class="usercard-image" />
    <h2 class="usercard-name">Old Gregg</h2>
</div>
```

Here we are defining the css for a navigation bar for the HTML below.  Notice we are writing many selectors over nesting selectors, this reduces complexity and improves reuse.  A media query on `.usercard img` is harder to overwrite than `.usercard-image` because it is more specific.  

####Layout Modules

While Component modules define smaller more reuseable components, Layout modules define more one-off behavior.  Generally layouts define how multiple components should be positioned inside a html section.  Layout module selectors should be prefixed with `.layout-` or `.l-` to mark them seperately of component modules.

#####Example

```scss
// filename: layouts/_profile.scss

.l-profile {
    .usercard { float: left; width: 30%; }
    .preferences { float: left; width: 70%; }
}
```

```html
<section class="l-profile">
    <div class="usercard"> ... </div>
    <div class="preferences"> ... </div>
</section>
```

Since our `.usercard` and `.preferences` modules are capable of streching size, we can set define how multiple modules are laid out inside our `.l-profile` element.

####States

##File Naming

#####Example File Structure

```
`-- sass
    |-- includes
        |-- _reset.scss
        |-- _variables.scss
        `-- mixins
            |-- _respond.scss
    |-- standard
        |-- _list.scss
        |-- _link.scss
        |-- _input.scss
    |-- modules
        |-- _usercard.scss
        |-- _nav.scss
        |-- _logo.scss
    |-- layouts
        |-- _header.scss
        |-- _profile.scss
        |-- _article.scss
    |-- states
        |-- _is-header-expanded.scss
```

##Selector Naming
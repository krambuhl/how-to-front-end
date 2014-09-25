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

The big thing to notice is we are not overwriting the base `input` tag.  We are defining a class that can be used anywhere, later we can extend this class more stragically.  It would make sense to extend the `input` tag in the `.form` standard class instead.  

```scss
// filename: standard/_form.scss

.form {
    input[type="text"] { @extend .input; }
    input[type="submit"] { @extend .buttom; }
}
```

####Component Modules


####Layouts
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
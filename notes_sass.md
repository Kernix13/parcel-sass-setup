# SASS Notes

> NEED TO WORK ON MIXINS AND FUNCTIONS

Have done: 1. variables, 2. Nesting, 3. Modules and namespacing, 4. Extends, 

Not done yet: 1. Mixins, 2. Functions, 3. Conditionals, 4. @content

Links

- [SASS-LANG](https://sass-lang.com/)
- [CSS Tricks: Mixin to Manage Breakpoints](https://css-tricks.com/snippets/sass/mixin-manage-breakpoints/)
- [Bulma: Responsive Mixins](https://bulma.io/documentation/utilities/responsive-mixins/)

<div id="back-to-top"></div>

## Table of Contents

1. [NPM Packages](#npm-packages)
1. [General Notes](#general-notes)
1. [Variables](#variables)
1. [Nesting](#nesting)
1. [Modules](#modules)
   1. [Namespaces and folders](#namespaces-and-folders)
1. [Inheritance and Extends](#inheritance-and-extends)
1. [Mixins](#mixins)
   1. [at content](#at-content)
1. [Functions](#functions)
1. [Operators](#operators)
1. [Conditionals](#conditionals)
1. [Sass Maps](#sass-maps)


## NPM Packages

- [package.json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)
- [scripts](https://docs.npmjs.com/cli/v8/using-npm/scripts)
- [npm-run-script](https://docs.npmjs.com/cli/v8/commands/npm-run-script)
- [Using Npm Scripts as a Build Tool](https://deliciousbrains.com/npm-build-script/): This is the best one I found for the basics

```sh
npm init -y
npm install sass --save-dev
sass src/scss/style.scss dist/css/style.csss
npm run scss
```

That installs SASS as a dev dependency and creates the `dist` folder with the CSS stylesheet, but it does not watch and update your changes.

- npm package, extensions don't work for this, setting it up with Parcel

```bash
npm install -D sass
sass --watchscss:css
```

## General Notes

Terms:

|           |          |          |          |
| :-------- | :------- | :------- | :------- |
| @import   | $varName | @extend  | @each    |
| @include  | &--class | @use     | @if      |
| @else     | @forward | @else if | @content |
| map-get() | #{$name} | -        | -        |

Check out [Sass Guidelines](https://sass-guidelin.es/) for project folder guidelines:

- `base/`
- `components/`
- `layout/`
- `pages/`
- `themes/`
- `abstracts/`
- `vendors/`

## Variables

- Unlike with CSS that has `var()` and `--color`, you just use `$varName`
- In CSS variables are called Custom Properties but it's nicer in SASS
- All variables are prefixed with `$`

```scss
// Declaration
$primaryBtn: rgb(18, 224, 224);
$textClr: #333;

// Rules
.contact button {
  background-color: $primaryBtn;
  color: $textClr;
}
```

## Nesting

- CSS can not nest rules within rules (`nav` > `ul` > `li` > `a`) 
- Nesting has 2 primary benefits: 1) helps you stay organized conceptually like in the html code, 2) it saves time typing
- See below as an example for using the `&` symbol (`&:hover` or `&::before`) for pseudo classes and elements - you use `&` as shorthand for the parent tag or class
- Instead of `nav ul`, `nav li`, `nav a`, you can nest `li`, `a`, `a:hover` withing a `nav` rule 

```scss
* {
  box-sizing: border-box;
  &::before {
    box-sizing: border-box;
  }
  &::after {
    box-sizing: border-box;
  }
}
```

## Modules

- Modules are for splitting up code into smaller files
- Organized code should have it's own file, similar to JavaScript
- Break your large CSS file into different files
- You need an underscore (`_`) as a prefix in the name so it won't be compiled 
- Then just import it into another file
- For example, cut all your header specific styles, create a file called `_header.scss`, and in the same spot in the old file add `@use 'header';` 
- There will be no change to the output in the css file
- **NOTE**: don't add `.scss` - it's not needed plus it throws an error
- **Abstracts** -> file names: __variables_, _fonts_, _colors_, _mixins_, _components_, _breakpoints_, _fuctions_
- Don't use `@import` and instead use `@use` and `@forward`
- Everything must ve namespaced to prevent problems with overwriting variables :

```scss
// Old way
@import "./abstracts/font";
@import "./abstracts/colors";
// New way
@use "./abstracts/font";
@use "./abstracts/colors";

// Examples: old
body {
  font-size: $font-size;
  color: $red;
}
// Examples: new - namespaced
body {
  font-size: font.$font-size;
  color: colors.$red;
}

// use `as` to change the name space:
@use "./abstracts/colors as c";
// thn in a rule use:
body {
  color: c.$red;
}
@use "./abstracts/colors as *";
// or use * to tak away the namespace:
```

> But you have to bring them into every file that will use them. Something about abstracts not needing to be brought in your main file

- To get around that, in any folder create `_index.scss`
- Use `@forward` to bring in your modules, then use `@use` to bring in `_index`

```scss
// add these in _index.scss
@forward "./abstracts/font";
@forward "./abstracts/colors";

// then in any file just add
@use "./abstracts";
```

### Namespaces and folders

- Using `@use` and `@forward` to get your partials working but you must use a name space
- Suggested folder names:
- abstracts: not actually complied into CSS: mixins, maps, breakpoints,
- base: general global styles
  - `_font-face.scss`
  - also `_index`, `_reset`, and `_typography.scss`
- components
  - `_article.scss`
  - `_buttons.scss`
  - also `_index`, `_label`, and `_nav.scss`
- layouts: 
- utilities: `_container.scss`, screen-reader only

## Inheritance and Extends

> For base styles and then extend them to more specific classes 

- `@extends` is for nheriting styles from one declaration to another
- Example: `btn-a`, and `btn-b` can use ALL of `btn-a` stying using `@extend btn-a` - `@extend` inherits the properties of whatever it references
- Common for things like buttons where you use base styles to style all your buttons, or cards, or other common components
- use `@extends name` where `name` is the element or class you want to inherit
- also use `%` to create a name that you can extend

```scss
%btn {
  display: inline-block;
  border-radius: 5px;
  padding: 8px 20px;
  margin: 3px;

  &:hover {
    transform: scale(.98);
  }
}

.btn-primary {
  @extend %btn;
  @include set-background(lighten($primary-color, 10%));
}

.btn-secondary {
  @extend %btn;
  @include set-background($secondary-color);
}
```

## Mixins

- They are similar to a function in JS
- Mixins can take parameters like functions - you need to use `@mixin` in conjuction with `@include`
- `@mixin name( ) { }` - don't hardcode your values in the mixin because you want it to be as flexible as possible
- Use `@include` to call the mixin

```scss
@mixin nameOfMixin($optionalParam) {
  // CSS properties here
}
@mixin otherName {
  // CSS properties here
}
.box { @include nameOfMixin(margin(30px)); }
nav ul { @include otherName; }
```

- Use `@mixin mixinName {}` to declare it, and use `@include mixinName()`
- Make sure to use parentheses `()` to call the mixin
- You can call it in the file the mixin is declared or in any file that imports the file that has the mixin declaration
- Any name for your param is okay but prefix it with `$`
- **NOTE**: You also have to declare something or you get an error - defaults not assumed
- You can add as many parameters as you want/need

```scss
// Declaration
@mixin flexCenter($direction) {
  display: flex;
  flex-direction: $direction;
  justify-content: center;
  align-items: center;
}

// Rules
@include flexCenter(column);
```

### at content

- `@content`: Something with media queries I think - see notes for `Lesson 67 Sass cont`

## Functions 

- Example: `lighten()` and there are many more
- Functions and mixins allow you to make blocks of CSS which you can run wherever you want
- Functions are similar to mixins but they actually return something
- Also with functions you don't need `@include`
- also use `@function` for conditionals

## Conditionals

- Logic via if/else statements - since these are in functions, add them to your utilities file
- Example is to use them in a mixin with parameters 
- `@if`, `@else`, and `@else if` and `@return`
- example: look at the background color and return a text color based on that - 
- his functions and mixins are not working

## Operators

> Skip for now 

- Mathematical operators - you don't want to have a value for something and not remember how you got to that value 
- Using `/ * - +` in your values
- Example: `100px + 50px`, or for division and mult which you need to wrap in parens like `(500px / 2)` or `(300px * 2)` - I guess `calc()` wasn't a thing back then

## Sass Maps

- Custom props and utility classes
- Example: `shade` is actually a map
- Traversy says he never uses source maps so he disables them from being created

>  What is interpolation? `#{$name}`

```scss
:root {
  @each $color, $shades in $colors {
    @each $shade, $value in $shades {
      --clr-#{$color}-#{$shade}: #{$value};
    }
  }

  // example with @if
  @each $screen-size, $font-size in $type-scale {
    @if $screen-size == small {
      @each $size, $value in $font-size {
        --fs-#{$size}: #{$value};
      }
    } @else {
      @include mq(medium) {
        @each $size, $value in $font-size {
          --fs-#{$size}: #{$value};
        }
      }
    }
  }
}
```

**NOTE** : Custom properties can be changed from within a media query.

Standard media query mixin:

```scss
$breakpoints: (
    small: 40em; medium: 65em;
  )
  @mixin mq($key) {
  $size: map-get($breakpoints, $key);

  @media only screen and (min-width: $size) {
    @content;
  }
}
```
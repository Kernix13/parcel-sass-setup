# SASS Notes

Links:

- [SASS-LANG](https://sass-lang.com/)
- [CSS Tricks: Mixin to Manage Breakpoints](https://css-tricks.com/snippets/sass/mixin-manage-breakpoints/)
- [Bulma: Responsive Mixins](https://bulma.io/documentation/utilities/responsive-mixins/)

# LearnWebCode

`LearnWebCode/docs/LWC-CSS.docx` - Section 19 Sass, Lesson 65 geting started..., Lesson 66 Sass basics..., Lesson 67 Sass cont.


**Variables**: define a color scheme for the page and apply to all links on the page - at the top of the scss file undo `$primaryColor: #ec7705;`

**Nesting**: `nav` > `ul` > `li` > `a` all in `header.site-header` - css can not nest rules within rules - nesting has 2 primary benefits:

1. helps you stay organized conceptually like in the html code,
2. it saves time typing

**inheritance / @extend**: style btn-a and btn-b - can use ALL of btn-a stying for btn-b using `@extend btn-a` - `@extend` inherits the properties of whatever it references

**splitting up code into smaller files / @import**: incredibly organized css code like any code related to the header is to have it's own file 

- create a file called `_header.scss`, and in the same spot in the old file add `@import 'header';` 
- there will be no change to the output in the css file - it helps you stay organized: `_header.scss` then `@import 'header';`

**another sass feature: functions**: `lighten()` and many more

**mixins**: alternate button to have a gradient bg - code to create a gradient - mixins use the 2 colors then output all the code - `@mixin name( ) { }` - don't hardcode your color values in the mixin b\c you want it to be as flexible as possible - look at the code but in the button rule use `@include` to call the mixin
- `_mixins.scss` - then use `@import`

**@content** - ??? something with media queries I think - see notes for `Lesson 67 Sass cont`

**operators**: mathematical operators - you don't want to have a value for something and not remember how you got to that value 

- example: `100px + 50px`, or for division and mult which you need to wrap in parens like `(500px / 2)` or `(300px * 2)` - I guess `calc()` wasn't a thing back then

> use @forward instead of @import

## Sass basics

> I think these notes are also from LearnWebCode

Basic notes on working with SASS files and syntax.

Syntax:

|           |          |          |          |
| :-------- | :------- | :------- | :------- |
| @import   | $varName | @extend  | @each    |
| @include  | &--class | @use     | @if      |
| @else     | @forward | @else if | @content |
| map-get() | #{$name} | -        | -        |

## Variables

- unlike with CSS that has `var()` and `--color`, you just use `$varName `

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

- See below but as an example use the `&` symbol (`&:hover` or `&::before`) for pseudo classes and elements

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

## Mixins

- Similar to a function in JS
- use `@mixin mixinName {}` to declare it, and use `@include mixinName()`
- make sure to use parentheses `()` to call the mixin
- you can call it in the file the mixin is declared or in any file that that imports the file that has the mixin declaration
- any name for your param is okay but prefix it with `$`
- NOTE: You also have to declare something or you get an error - defaults not assumed
- you can add as many parameters as you want/need

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

## Customixing your mixins

- change the flex mixin above to be column instead of row
- you can add parentheses to the mixin name and add parameters that can be changed each time you use it

## Extends

- inheriting styles from one declaration to another -
- use `@extends name` where `name` is the element or class you want to inherit

```scss
.contact {
  @extend header;
}
```

### Calc

- his example didn't work for me

### Separate files

- create `_header.scss`, paste header styes into there, then back in your main file use `@import "./header";`
- don't add the extension `.scss` - it's not needed plus it throws an error
- also try `_variables.scss` and do the same in the main scss file
- abstracts -> file names: fonts, colors, mixins, breakpoints, fuctions

# Kevin Powell 1

- mentions not using `@import` and instead use `@use` and `@forward`
- has to use an npm package because extensions don't work for this and setting it up with Parcel

```bash
npm install -D sass
sass --watchscss:css
```

- Add you font vars in your font file - he says you should not use `@import` anymore because it is deprecated
- but everything becomes namespaced to prevent problems with overwriting variables 
- here is both an import and use example:

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

But you have to bring them into every file that will use them. Something about abstracts not needing to be brought in your main file -

- to get around that in any folder create `_index.scss` -
- use - you bring it into that file to use - use `@forward` to bring it into the file to send it back out -

```scss
// add these in _index.scss
@forward "./abstracts/font";
@forward "./abstracts/colors";
// then in a file just do
@use "./abstracts";
```

## Partials

What are partials?

> A partial is a Sass file named with a leading underscore. You might name it something like `_partial.scss`. The underscore lets Sass know that the file is only a partial file and that it should not be generated into a CSS file. Sass partials are used with the @use rule

- Stop using `@import` with Sass -> Using `@use` and `@forward` to get your partials working
- Suggested folder names:
- abstracts: not actually complied into CSS: mixins, maps, breakpoints,
- base: general global styles
  - `_font-face.scss`
  - also `_index`, `_reset`, and `_typography.scss`
- components
  - `_article.scss`
  - `_buttons.scss`
  - also `_index`, `_label`, and `_nav.scss`
- layouts
- utilities: `_container.scss`, screen-reader only

Check out [Sass Guidelines](https://sass-guidelin.es/):

- `base/`
- `components/`
- `layout/`
- `pages/`
- `themes/`
- `abstracts/`
- `vendors/`

## Sass Maps

- custom props and utility classes
- example, shades is actually a map
- what is interpolation? `#{$name}`

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
..........................................................

# Traversy Video

From [Sass Crash Course](https://youtu.be/nu5mdN2JIwM) and [Codepen](https://codepen.io/bradtraversy/pen/ExjmGdY?editors=1100).

## Intro

- CSS PreProcessor
- .scss files are compiled to .css

### Variables intro

- in CSS variables are called Custom Properties but it's nicer in SASS
- prefixed with `$`

### Nesting intro

- instead of `nav ul`, `nav li`, `nav a`, you can nest `li`, `a`, `a:hover` withing a `nav` rule -

### Modules intro

- break into different files - organization - you need an underscore in the name so they won't be compiled -just import it into another file

### Mixins and functions intro

- mixins can take parameters like functions - you need to use `@mixin` in conjuction with `@include`
- functions actually returns something - you don't need `@include`

### inheritance intro

- common for things like buttons where you use base styles to styles all your buttons, or cards, or whatevs - use `@extend` for that

### operators intor

- using `/ * - +` in your values

### conditionals intro

- `@if`, `@else`, and `@else if` -

## Landing Page Project

- had to go to lorem picsum
- create a folder called `scss` and inside `style.scss`
- create some styles then we need a compiler:

**NPM module**:

- he wants to use `sudo npm i -g sass` but I don't want it installed globally and sudo puts your computer at risk "...by giving untrusted code administrative privileges"
- so do `npm install sass` -
- but the global way enables him to use cmds like `sass --watch scss/style.scss css/style.css`
- he says he never uses source maps so he disables them from being created -
- have to stop because he is using Live Sass Complier

## Important Links

- [package.json](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)
- [scripts](https://docs.npmjs.com/cli/v8/using-npm/scripts)
- [npm-run-script](https://docs.npmjs.com/cli/v8/commands/npm-run-script)
- [Using Npm Scripts as a Build Tool](https://deliciousbrains.com/npm-build-script/): This is the best one I found for the basics

```nodejs
npm init -y
npm install sass --save-dev
sass src/scss/style.scss dist/css/style.csss
npm run scss
```

That installs SASS as a dev dependency and creates the `dist` folder with the CSS stylesheet, but it does not watch and update your changes.
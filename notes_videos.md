# Notes from the videos on Parcel

> Why is `npm run dev` creating hash files in my dist folder? How do you clean the dist folder with parcel? 

- https://stackoverflow.com/questions/56506191/how-to-wipe-dist-directory-before-a-build-with-parcel
- There's a plugin for that `parcel-plugin-clean-dist` or try `parcel-reporter-clean-dist`

Notes are from videos by:

1. James Q. Quick: [Getting Started with Parcel.js - A Web Application Bundler](https://youtu.be/ONwotPEpinI)
1. Traversy Media: [Exploring The Parcel Application Bundler](https://youtu.be/DYZTFooDB24)
1. Web Dev Simplified: [Must Know JavaScript Bundler - Parcel](https://youtu.be/DblzpCoPakw)
1. Gary Simon (designcourse): [The Parcel Bundler - A SUPER Easy JavaScript Bundler for your Projects](https://youtu.be/OK6akGZCC88)
1. Kevin Powell: [Sass with auto-refresh (and more) made easy](https://youtu.be/wYWf2m_yzBQ)

<div id="back-to-top"></div>

## Table of Contents

1. [Initial Setup](#initial-setup)
   1. [Setup notes](#setup-notes)
1. [CSS and JS and Images](#css-and-js-and-images)
   1. [SASS](#sass)
   1. [Postcss and Tailwind](#postcss-and-tailwind)
   1. [JavaScript](#javascript)
   1. Images
1. [Miscellaneous](#miscellaneous)

## Initial Setup

Run the following npm commands before or after installing Git:

```bash
npm init -y
npm install --save-dev parcel
npm install --save-dev sass
# or
npm i -D parcel
npm i -D sass
```

Then:

1. Create a folder named `src` in the root of your project.
1. Create `index.html` in the `src` folder.
1. Create an `scss` folder inside `src` and inside there create `main.scss`.
1. Create a `js` folder inside `src` and inside there create `main.js`.
1. Add `<link rel="stylesheet" href="./scss/main.scss" />` in the `<head>` tag of the HTML file.
1. Add `<script type="module" src="./js/main.js"></script>` before the `<body>` tag of the HTML file.
1. Then add the following scripts to `package.json` for Parcel:

```json
"scripts": {
  "dev": "parcel src/index.html",
  "build": "parcel build src/index.html"
},
```

### Setup notes

**NOTE #1**: In `package.json`, you may also see people use `prod` instead of `build` - you can name them whatever you want

For the `dev` script add the package name `parcel` and the filename for the entry point, `src/index.html` in this case. Change that if the location and/or filename is different. Use `npm run dev`.

Use `npm run dev` to set up your dev environment, minify your code, set up a localhost at `http://localhost:1234` and create a build folder called `dist`.

Use the `build` script for when you deploy to production and to minify everything - add the same as for `dev` but add the `build` keyword. `npm run build`.

> **QUESTION #1**: How to configure the port number?

**ANSWER #1**: Right at the top of the [Development page](https://parceljs.org/features/development/) under `Dev server` it mentions some flags to use on the CLI, but what about in `package.json`? Search on Google and GitHub for `parcel package.json`. Also do a search on their page for `package.json` which has what I need I think and look for `#source`, `#targets`, and `#main`.

> **QUESTION #2**: How to change the build folder name?

Check out the [Production page](https://parceljs.org/features/production/) but it doesn't say anything about changing the build folder name or `package.json`. It seems `yarn` is the preferred package for using Parcel.

From the _Parcel Get started_ page, here is the `package.json` file:

```json
{
  "name": "my-project",
  "source": "src/index.html",
  "scripts": {
    "start": "parcel",
    "build": "parcel build"
  },
  "devDependencies": {
    "parcel": "latest"
  }
}
```

So here you can change the location and name of the filename and omit that parameter from the scripts by adding the `source` variable: `"source": "src/index.html",`. That works.

**NOTE #2**: You also do not need to run the sass command `sass watch src/scss:dist/css`. Parcel takes care of all that.

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## CSS and JS and Images

> Some of the below I did not do for this project but you can import your CSS into your JS file.

- In the `src` folder create folders called `css` or `scss`, `images` and `js`.
- Then in those create `main.scss` or `main.css` and `main.js`.
- Gary Simon added `index.js` into the `src` folder next to `index.html` instead of in a `js` folder.
- you can change the `source` variable in `package.json`.

### SASS

Normally to use SASS you would need some kind of compiler to compile it to CSS but Parcel does that for you. So add your SASS rules in your `main.scss` file, then in `main.js` add the following:

```js
import "./../scss/main.scss";
```

> DO NOT FORGET TO IMPORT THAT INTO YOUR JS FILE

- also add `main.js` into `index.html` - in the `<head>` tag with `defer` or not?
- For `scss` files, James Q. Quick just changed `main.css` to `main.scss` and Parcel did the rest - he did not have to install a sass package

> Parcel supports Sass files automatically using the `@parcel/transformer-sass` plugin. When a .sass or .scss file is detected, it will be installed into your project automatically.

### Postcss and Tailwind

While Parcel supports equivalent functionality to many common PostCSS plugins such as `autoprefixer` and `postcss-preset-env` out of the box as described above, PostCSS is useful for more custom CSS transformations such as non-standard syntax additions. It is also used by popular CSS frameworks such as Tailwind.

From parcel's docs:

> You can use PostCSS with Parcel by creating a configuration file using one of these names: `.postcssrc`, `.postcssrc.json`, `.postcssrc.js`, or `postcss.config.js`

- To plugins used by `postcss` - `npm instal autoprefixer` - then to use `postcss` you need to add a file in the root named `.postcssrc` and add the plugins

```json
{
  "plugins": {
    "autoprefixer": true,
    "tailwindcss": true
  }
}
```

- Based on your configured browser targets, Parcel automatically adds vendor prefixed fallbacks for many CSS
- if your CSS source code (or more likely a library) includes unnecessary vendor prefixes, Parcel CSS will automatically remove them to reduce bundle sizes
- For tailwind though you will also need `tailwind.config.js`

Other useful [Postcss plugins](https://github.com/postcss/postcss#plugins):

- `postcss-preset-env` allows you to use future CSS features today
- `postcss-nested` unwraps nested rules the way Sass does
- `cssnano` is a modular CSS minifier

Check out [Postcss on Parcel](https://parceljs.org/languages/css/#postcss)

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### JavaScript

Then in `index.html` add `<script src="./js/main.js" defer></script>` in the `<head>` tag.

> Wrong not `defer` need `module` attribute

Then create another JS file called `helper.js` or whatever and do:

```js
export default function helper() {
  // code here
}
// in main.js
import helper from "./helper";
helper();
```

So Parcel does all the stuff Webpack does by running `npm run dev` without the `webpack.config.js` file and `package.json` scripts referencing Webpack.

- make sure to have lots of exports and imports to test the bundling

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Images

- nothing here yet

## Miscellaneous

**Stopping the server**:

You need to do `CTRL+C` to stop the server and then `npm run dev` to restart it for major file structure changes.

**Deleting the dist folder**:

Delete it and then rerun `npm run dev` and it will be rebuilt but check the file size (Why?). Then run `npm run build` to reduce the js file size.

**NOTE**: `export` any functions from a file that you need then `import` it into the files that need them.

**NOTE 2**: `async / await` is not supported in Parcel by default - to be able to use those `import "babel-polyfill"` into any file that is using async/await or importing a function that uses them. Parcel will add it to your dependencies when you save the file - no NPM command needed.

**File structure example**: LATER

## Babel

[Babel Getting Started](https://babeljs.io/docs/en/index.html) and [Parcel/Babel](https://parceljs.org/languages/javascript/#babel)

- Babel with Parcel uses the env preset, `@babel/preset-env` - 

> What javascript features does parcel not support? What babel plugins are needed?

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>
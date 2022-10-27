# Setting up Parcel

Notes for using Parcel instead of Webpack as a bundler:

1. Reduces the size of your app
1. No config code splitting / organization
1. Automatic transforms (SASS, JS modules)
1. Hot module replacement
1. Complile SASS to CSS
1. Working with images
1. Way easier than Webpack!

Check out the [Parcel on GitHub](https://github.com/parcel-bundler/parcel) for folders like `.vscode` and `.husky`, and files like `.editorconfig`, `.prettierignore` and `.prettierrc`.

Notes are from videos by:

1. James Q. Quick: [Getting Started with Parcel.js - A Web Application Bundler](https://youtu.be/ONwotPEpinI)
1. Traversy Media: [Exploring The Parcel Application Bundler](https://youtu.be/DYZTFooDB24)
1. Web Dev Simplified: [Must Know JavaScript Bundler - Parcel](https://youtu.be/DblzpCoPakw)
1. Gary Simon (designcourse): [The Parcel Bundler - A SUPER Easy JavaScript Bundler for your Projects](https://youtu.be/OK6akGZCC88)
1. Kevin Powell: [Sass with auto-refresh (and more) made easy](https://youtu.be/wYWf2m_yzBQ)
1. [Parcel Docs page](https://parceljs.org/docs/)

<div id="back-to-top"></div>

## Table of Contents

1. [Initial Setup](#initial-setup)
1. [CSS and JS and Images](#css-and-js-and-images)
   1. [SASS](#sass)
   1. [JavaScript](#javascript)
   1. Images
1. [Miscellaneous](#miscellaneous)
1. [Parcel Documentation Notes](#parcel-documentation-notes)
   1. [Declaring browser targets](#declaring-browser-targets)
1. [Development](#development)
   1. [Dev server](#dev-server)
   1. [Hot reloading](#hot-reloading)
   1. [Lazy mode](#lazy-mode)
   1. [Caching](#caching)
   1. [HTTPS](#https)
   1. [API proxy](#api-proxy)
   1. [Auto install](#auto-install)
1. [Production](#production)
   1. [Size optimization](#size-optimization)
   1. [Minification](#minification)
   1. [Tree Shaking](#tree-shaking)
   1. [Cache optimization](#cache-optimization)
   1. [Analyzing bundle sizes](#analyzing-bundle-sizes)

## Initial Setup

Run the following npm commands before or after installing Git:

```bash
npm init -y
npm install --save-dev sass
npm install --save-dev parcel
# or
npm i -D sass
npm i -D parcel
```

Then:

1. Create a folder named `src` in the root of your project.
1. Create `index.html` in the `src` folder.
1. Create an `scss` folder inside `src` and inside there create `main.scss`.
1. Add `<link rel="stylesheet" href="./scss/main.scss" />` in the `<head>` tag of the HTML file.
1. Then add the following scripts to `package.json` for Parcel:

```json
"scripts": {
  "dev": "parcel src/index.html",
  "build": "parcel build src/index.html"
},
```

For the `dev` script add the package name `parcel` and the filename for the entry point, `src/index.html` in this case. Change that if the location and/or filename is different. Use `npm run dev`.

Use `npm run dev` to setup your dev environment, minify your code, set up a localhost at `http://localhost:1234` and create a build folder called `dist`.

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

NOTE: You also do not need to run the sass command `sass watch src/scss:dist/css`. Parcel takes care of all that.

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

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### JavaScript

Then in `index.html` add `<script src="./js/main.js" defer></script>` in the `<head>` tag.

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

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Miscellaneous

**Stopping the server**:

You need to do `CTRL+C` to stop the server and then `npm run dev` to restart it for major file structure changes.

**Deleting the dist folder**:

Delete it and then rerun `npm run dev` and it will be rebuilt but check the file size (Why?). Then run `npm run build` to reduce the js file size.

**NOTE**: `export` any functions from a file that you need then `import` it into the files that need them.

**NOTE 2**: `async / await` is not supported in Parcel by default - to be able to use those `import "babel-polyfill"` into any file that is using async/await or importing a function that uses them. PArcel will add it to your dependencies when you save the file - no NPM command needed.

**File structure example**: LATER

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

<br>

# Parcel Documentation Notes

These are notes verbatim from parceljs.org. I only copied that which is most important and pertinent to myself. And these are only a fraction of their docs.

## Declaring browser targets

By default Parcel does not perform any code transpilation. This means that if you write your code using modern language features, that’s what Parcel will output.

You can declare your app’s supported browsers using the `browserslist` (`See below`) field. When this field is declared, Parcel will transpile your code accordingly to ensure compatibility with your supported browsers.

```json
"browserslist": "> 0.5%, last 2 versions, not dead",
"scripts": {
    "start": "parcel",
    "build": "parcel build"
  },
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Development

[Development docs](https://parceljs.org/features/development/)

Parcel includes a development server out of the box supporting hot reloading, HTTPS, an API proxy, and more.

- the default `parcel` command is a shortcut for `parcel serve`
- By default, it starts a server at `http://localhost:1234`. If port `1234` is already in use, then a fallback port will be used.
- After Parcel starts, the location where the dev server is listening will be printed to the terminal

### Dev server

CLI Options:

- `-p`, `--port` – Overrides the default port. The PORT environment variable can also be used to set the port
- `--open` – Automatically opens the entry in your default browser after Parcel starts

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Hot reloading

**Hot Module Replacement**: improves the development experience by updating modules in the browser at runtime without needing a whole page refresh, application state can be retained as you change small things in your code.

- CSS changes are automatically applied via HMR with no page reload necessary
- This is also true when using a framework with HMR support built in
- If you’re not using a framework, you can opt into HMR using the `module.hot` API
- module.hot is only available in development, so you'll need to check that it exists before using it:

```js
if (module.hot) {
  module.hot.accept();
}
```

- Look into `module.hot.dispose(cb)`
- Skip the section _Development target_ about older browsers and `--target`, `--target legacy`, and `--target default`,

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Lazy mode

...waiting for your entire app to build before the dev server starts up. If you’re only working on one feature, you shouldn’t need to wait for all of the others to build unless you navigate to them.

You can use the `--lazy` CLI flag to tell Parcel to defer building files until they are requested in the browser...when you navigate to a page for the first time, Parcel builds only the files necessary for that page. When you navigate to another page, that page will be built on demand. If you navigate back to a page that was previously built, it loads instantly.

```bash
parcel 'pages/*.html' --lazy
```

This also works with dynamic `import()`, not just separate entries. So if you have a page with a dynamically loaded feature, that feature will not be built until it is activated. When it is requested, Parcel eagerly builds all of the dependencies as well, without waiting for them to be requested.

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Caching

If you restart the dev server, Parcel will only rebuild files that have changed since the last time it ran. Parcel automatically tracks all of the files, configuration, plugins, and dev dependencies that are involved in your build, and granularly invalidates the cache when something changes. For example, if you change a configuration file, all of the source files that rely on that configuration will be rebuilt.

By default, the cache is stored in the `.parcel-cache` folder inside your project. You should add this folder to your `.gitignore` file. You can also override the location of the cache using the `--cache-dir` CLI option. Caching can also be disabled using the `--no-cache` flag. Note that this only disables reading from the cache – a `.parcel-cache` folder will still be created.

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### HTTPS

...you may need to use HTTPS during development...you may need to use a certain hostname for authentication cookies, or debug mixed content issues. Parcel’s dev server supports HTTPS out of the box. You can either use an automatically generated certificate, or provide your own.

To use an automatically generated self-signed certificate, use the `--https` CLI flag. The first time you load the page, you may need to manually trust this certificate in your browser.

```bash
parcel src/index.html --https
```

To use a custom certificate, you’ll need to use the `--cert` and `--key` CLI options to specify the certificate file and private key respectively.

```bash
parcel src/index.html --cert certificate.cert --key private.key
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### API proxy

To better emulate the actual production environment when developing web apps, you can specify paths that should be proxied to another server (e.g. your real API server or a local testing server) in a `.proxyrc`, `.proxyrc.json` or `.proxyrc.js` file.

- lots of code - look into this later
- Skip the section `File watcher`

### Auto install

When you use a language or plugin that isn’t included by default, Parcel will automatically install the necessary dependencies into your project for you...if you include a .sass file, Parcel will install the `@parcel/transformer-sass` plugin and it will be added to the devDependencies in your package.json.

## Production

[Production docs](https://parceljs.org/features/production/)

Parcel’s production mode automatically bundles and optimizes your application for production. It can be run using the parcel build command:

```bash
parcel build src/index.html
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Size optimization

Parcel includes many optimizations designed to reduce bundle sizes, including automatic minification, tree shaking, image optimization, and more.

### Minification

Parcel includes minifiers for JavaScript, CSS, HTML, and SVG out of the box. Minification reduces the file size of your output bundles by removing whitespace, renaming variables to shorter names, and many other optimizations.

By default, minification is enabled when using the parcel build command. Parcel uses `terser` to minify JavaScript, `@parcel/css` for CSS, `htmlnano` for HTML, and `svgo` for SVG.

If needed, you can configure these tools using a `.terserrc`, `.htmlnanorc`, or `svgo.config.json` config file. See the docs for [JavaScript](https://parceljs.org/languages/javascript/), [CSS](https://parceljs.org/languages/css/), [HTML](https://parceljs.org/languages/html), and [SVG](https://parceljs.org/languages/svg/) for more details.

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Tree Shaking

In production builds, Parcel statically analyzes the imports and exports of each module, and removes everything that isn't used. This is called "tree shaking" or "dead code elimination".

Parcel also concatenates modules into a single scope when possible, rather than wrapping each module in a separate function. This is called “scope hoisting”. This helps make minification more effective, and also improves runtime performance by making references between modules static rather than dynamic object lookups.

- Skip the sections _ Development branch removal_, _Image optimization_, _Differential bundling_, and _Compression_.

### Cache optimization

Parcel includes several optimizations related to browser and CDN caching, including content hashing, bundle manifests, and shared bundles.

Parcel automatically includes content hashes in the names of all output files, which enables long-term browser caching.

By default, all bundles include a content hash except entries and certain dependency types that require names to be stable. For example, service workers require a stable file name to work properly...

You can also disable content hashing using the `--no-content-hash` CLI flag. Note that the name will still include a hash, but it will not change on each build. You can customize bundle naming completely using Namer plugins.

- Skip the section _Cascading invalidation_ and _Shared bundles_.

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Analyzing bundle sizes

Parcel includes some tools to help you analyze bundle sizes:

- **Detailed report** - To see more details about what files make up each bundle, you can use the `--detailed-report` CLI option
- **Bundle analyzer** - The `@parcel/reporter-bundle-analyzer` plugin can be used to generate an HTML file containing a tree map that shows the relative size of each asset in every bundle visually. You can run it using the `--reporter` CLI option

```bash
parcel build src/index.html --reporter @parcel/reporter-bundle-analyzer
```

- **Bundle Buddy** - skip details

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

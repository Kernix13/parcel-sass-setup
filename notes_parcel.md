# Notes from Parcel Website

[Parcel Docs page](https://parceljs.org/docs/)

<div id="back-to-top"></div>

## Table of Contents

1. [Building a web app with Parcel](#building-a-web-app-with-parcel)
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

<br>

## Building a web app with Parcel

These are notes verbatim from parceljs.org. I only copied that which is most important and pertinent to myself. And these are only a fraction of their docs.

INSTALLATION:

```bash
npm install --save-dev parcel
```

- create `src/index.html` and add 

```html
<link rel="stylesheet" href="styles.css" />
<script type="module" src="app.js"></script>
```

- In `package.json` add:

```bash
"scripts": {
    "start": "parcel",
    "build": "parcel build"
```

- run `npm run dev` to run the dev server and `npm run build` to build the production files

## Declaring browser targets

By default Parcel does not perform any code transpilation. This means that if you write your code using modern language features, that’s what Parcel will output.

You can declare your app’s supported browsers using the `browserslist` (`See below`) field. When this field is declared, Parcel will transpile your code accordingly to ensure compatibility with your supported browsers.

```json
"browserslist": "> 0.5%, last 2 versions, not dead",
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

> What is _HMR_?

**Hot Module Replacement**: improves the development experience by updating modules in the browser at runtime without needing a whole page refresh, application state can be retained as you change small things in your code.

- CSS changes are automatically applied via HMR with no page reload necessary
- This is also true when using a framework with HMR support built in
- If you’re not using a framework, you can opt into HMR using the `module.hot` API
- `module.hot` is only available in development, so you'll need to check that it exists before using it:

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


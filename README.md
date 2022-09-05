# Setting up Parcel

Notes for using Parcel instead of Webpack as a bundler:

1. Reduces the size of your app
1. Code splitting / organization
1. Hot module replacement
1. Complie SASS to CSS
1. Working with images

Check out [Parcel on GitHub](https://github.com/parcel-bundler/parcel) for folders like `.vscode` and `.husky`, and files like `.editorconfig`, `.prettierignore` and `.prettierrc`.

Notes are from videos by Web DEv Simplified, Gary Simon (designcourse), and Kevin Powell.

## Initial Setup

Run the following npm commands after installing Git:

```bash
npm init -y
npm install sass --save-dev
npm install parcel --save-dev
# or
npm i -D sass
npm i -D parcel
```

Then:

1. Create an `src` folder.
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

For the development script add the package name `parcel` and the filename for the entry point, `src/index.html` in this case. Change that if the location and/or filename is different.

For the build script, for when you deploy to production and to minify everything, add the same as for `dev` but add the `build` keyword.

Use `npm run dev` to setup your dev environment, minify your code, set up a localhost at `http://localhost:1234` and create a build folder called `dist`.

> **QUESTION #1**: How to configure the port number?

**ANSWER #1**: Right at the top of the [Development page](https://parceljs.org/features/development/) under `Dev server` it mentions some flags to use on the CLI, but what about in `package.json`? Search on Google and GitHub for `parcel package.json`. Also do a search on their page for `package.json` which has what I need I think ans look for `#source`, `#targets`, and `#main`.

> **QUESTION #2**: How to change the build folder name?

Check out the [Production page](https://parceljs.org/features/production/) but it doesn't say anything about changing the build folder name or package.json. It seems `yarn` is the preferred package for using Parcel.

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

So here you can change the location and name of the filename and omit that parameter from the scripts by adding the `source` property. That works.

NOTE: You also do not need to run the sass command `sass watch src/scss:dist/css`. PArcel takes care of all that.

## CSS and JS and Images

> Some of the below I did not do for this project but you can import your CSS into your JS file.

In the `src` folder create folders called `css` or `scss`, `images` and `js`. Then in those create `main.scss` or `main.css` and `main.js`.

Gary Simon added `index.js` into the `src` folder next to `index.html` instead of in a `js` folder.

### SASS

Normally to use SASS you would need some kind of compiler to compile it to CSS but Parcel does that for you. So add your SASS rules in your `main.scss` file, then in `main.js` add the following:

```js
import "./../scss/main.scss";
```

Then in `index.html` add `<script src="./js/main.js" defer></script>` in the `<head>` tag.

Then create another JS file called helper.js or whatever and do:

```js
export default function helper() {
  // code here
}
// in main.js
import helper from "./helper";
helper();
```

So Parcel does all the stuff Webpack does by running `npm run dev` without the `webpack.config.js` file and `package.json` scripts.

## Miscellaneous

**Stopping the server**:

You may need to do `CTRL+C` to stop the server and then `npm run dev` to restart it for major file structure changes.

**Deleting the dist folder**:

Delete it and then rerun `npm run dev` and it will be rebuilt but check the file size. Then run `npm run build` to reduce the js file size.

**File structure example**: LATER

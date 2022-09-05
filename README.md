# Setting up Parcel

Using Parcel instead of Webpack as a bundler.

1. Reduces the size of your app
1. Code splitting / organization
1. Hot module replacement
1. Complie SASS to CSS
1. Working with images

## Initial Setup

Run the following npm commands after installing Git:

```bash
npm init -y
npm install --save-dev parcel
# or
npm i -D parcel
```

Then add scripts to `package.json` file for Parcel:

```json
"scripts": {
  "dev": "parcel src/index.html",
  "build": "parcel build src/index.html"
},
```

For the development script add the package name `parcel` and the filename for the entry point, `src/index.html` in this case.

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

So here you can change the location and name of the filename and omit that parameter from the scripts by adding the `source` property.

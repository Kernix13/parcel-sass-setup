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

Check the notes in `notes_videos.md` and `notes_parcelDocs.md` for installing and getting started with PArcel.

## Installation and setup

**From Parcel docs**:

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

**From video notes**:

```bash
npm install --save-dev parcel
npm install --save-dev sass
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

Installation and setup is nearly identical for the notes from Parcel and the Video notes with the exception of the `package.json` scripts. I changed `"main": "index.js",` to `"main": "src/index.js",` in my `package.json` file, so I think the Parcel scripts are the correct ones. Just double-check the `"source"` and `"main"` values.
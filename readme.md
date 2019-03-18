# Mostly Static Boilerplate

A boilerplate repo for mostly-static websites.

## Why

If you create a lot of mostly-static websites that based on `less css` and some vanilla JavaScript, it's helpful to have a simple boilerplate repository to do things like:

-   Compile `less` to `css` automatically as you make changes
-   Ensure your JavaScript is compatible with older browsers
-   Prepare your site for production

This repo is my quick and dirty solution. Comments and suggestions welcome.

## Getting Started

If you just want to get started, clone or download the repo.

You can run a simple local server and automatically compile `less` by running: `npm start`.

To build your site for production, run `npm run build`, which outputs files to the `dist` folder.

## Setup

If you want to know how to set this up yourself, here's the setup:

### Set Up the Directory

The project has the following directory structure:

```
project
│   .babelrc
│   .gitattributes
│   .gitignore
│   package.json
│   package-lock.json
│   .gitignore
│   readme.md
│
└───src
│   │   *.html
│   │
│   └───assets
│       └───css
│       └───img
│       └───js
│
└───dist
│   │   *.html
│   │
│   └───assets
│       └───css
│       └───img
│       └───js
│
└───node_modules
```

The `src` folder contains the code you'll write in html, less, and JavaScript. The `dist` folder is for built/clean/minified code that's ready for production.

`mkdir src src/assets src/assets/css src/assets/img src/assets/js dist`

### Node

We're going to use Node packages to automate our build process and serve our site locally. If you don't already have node, you'll need to install it.

`npm init`

### Install Node Modules:

-   Serve: A simple Node Server based on Zeit/Now
-   Less Watch Compiler: Automatically compile less into CSS whenever you make changes
-   Babel + Presets: Transpile ES6 JavaScript to ES5 and Minify It
-   Concurrently: Allows the Above to Run Simultaneously

`npm install --save-dev serve`
`npm install --save-dev less-watch-compiler`
`npm install --save-dev @babel/core @babel/cli @babel/preset-env babel-preset-minify`
`npm install --save-dev concurrently`

### Make a .gitignore file

If you're going to save this project to a GitHub repository (you should), make a `.gitignore` file with the following:

```
node_modules
dist
```

This will keep your repo clean by preventing Git from tracking anything in your `dist` or `node_modules` folders.

### Make a .babelrc file

We'll use Babel to transpile our JavaScript to ES5, so that it's compatible with older browsers. Create a `.babelrc` file and include the following:

```
{
    "presets": ["@babel/preset-env", "minify"]
}
```

### Add Scripts to package.json

We'll want a few npm scripts to automate the build process. Overwrite the `scripts` object with the following:

```
"scripts": {
    "start": "concurrently \"npx serve src\" \"npm run watch-css\"",
    "watch-css": "npx less-watch-compiler src/assets/css src/assets/css style.less",
    "clean": "rm -rf dist && mkdir -p dist/assets/css dist/assets/js dist/assets/img || true",
    "build": "npm run clean && npm run copy-html && npm run copy-css && npm run copy-img && npm run build-js",
    "copy-html": "find src -name \\*.html -exec cp {} dist \\;",
    "copy-css": "cp -a src/assets/css/style.css dist/assets/css",
    "copy-img": "cp -R src/assets/img dist/assets",
    "build-js": "npx babel src/assets/js -d dist/assets/js",
    "serve-dist": "npx serve dist",
    "destroy-dist": "rm -rf dist",
    "test": "echo \"Error: no test specified\" && exit 1"
},
```

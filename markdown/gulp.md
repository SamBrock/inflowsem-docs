# Gulp
Modern web development has many repetitive tasks like running a local server, minifying code, optimizing images, preprocessing CSS and more. Build tools like Gulp help automate these tasks. 

[Gulp](https://gulpjs.com/) is a streaming task runner that lets developers automate many development tasks. It reads files as streams and pipes the streams to different tasks. These tasks are code-based and use plugins. The tasks modify the files, building source files into production files. 

## How to set up gulp
Setting up gulp for the first time requires a few steps.

### Node
Gulp requires [Node](https://nodejs.org/en/), and its packet manager, [npm](https://www.npmjs.com/), which installs the gulp plugins.

To install Node, click [here](https://nodejs.org/en/) to download the latest version - this also installs npm. 

### Gulp command line tool
Gulp's command line tool should also be installed globally so that gulp can be executed from the command line. Do this by running the following from the command line:

```
npm install --global gulp-cli
```

### Creating a new project
Before installing gulp plugins, your application needs to be initialised. Do this by running the following command line command from within your project's working directory:
```
npm init
```
This command begins the generation of a package.json file, prompting you with questions about your application. You can leave these blank, or skip them entirely by doing `npm init -y` instead of the above command.

#### package.json
```json
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
Don't worry if you don't understand what all of these values represent, they are not critical to learning gulp.

The important thing to know is that this file is used to track your project's packages. Tracking packages like this allows for quick reinstallation of all the packages and their dependencies in future builds. The `npm install` command will read package.json and automatically install everything listed. 

### Installing packages
Gulp and Node rely on plugins (packages) for the majority of their functionality. Node plugins can be installed with the following command line command:
```
npm install pluginName
```
This command uses the npm tool to install the `pluginName` plugin. Plugins and their dependencies are installed in a node_modules folder inside the project's working directory. 

The first plugin that you want to install is gulp itself. Do this by running the following command in the command line from within your project's working directory:
```
npm install gulp
```
Gulp and its dependencies are then present in the the node_modules directory (inside the project directory). The package.json file is also updated to the following:
```json
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "gulp": "^3.9.1"
  }
}
```
Note that there is now a `devDependencies` field with gulp and its current version listed.

### Gulpfile
Once packages are installed (in node_modules), you are ready to use them. All gulp code is written in in a gulpfile.js file. To use a package, start by including it in gulpfile.js. The following code in your gulpfile includes the gulp package that was installed in the previous section:
```js
const gulp = require('gulp');
```
You may not have come across `require` before. The basic functionality of `require` is that it reads a JavaScript file, executes the file, and then proceeds to return the `exports` object. In the above case, it finds the the folder called gulp within node_modules, executes it and returns whatever is exported within `gulp/index.js`.

The rules of where `require` finds the files can be a little complex, but a simple rule of thumb is that if the file doesn't start with "./" or "/", then it is considered a dependency in the local node_modules folder. If the file starts with "./" it is considered a relative file to the file that called `require`. If the file starts with "/", it is considered an absolute path. NOTE: you can omit ".js" and `require` will automatically append it if needed. For more detailed information, see [What is require?](https://nodejs.org/en/knowledge/getting-started/what-is-require/).

## Creating tasks
Gulp tasks are defined in the gulpfile.js file using functions which returns a stream. Streams in gulp provide us with a way to convert files into an object that can flow through the pipeline without having to be written to disk after each step. A simple tasks looks like this (taken from the gulpfile for INFLOWSEM):

```js
function videos() {
  return gulp
    .src('./src/videos/**/*')
    .pipe(gulp.dest('./dist/videos'))
}
```
This code defines a task which moves the videos from the `src` folder to the `dist` folder ("dist" is short for distributable, a file which can be used by others without the need to compile - also used in production).

A complete gulpfile might look like this:
#### gulpfile.js
```js
// Load plugins
const gulp = require("gulp");
const fileinclude = require("gulp-file-include");
const autoprefixer = require("gulp-autoprefixer");
const rename = require("gulp-rename");
const cleanCSS = require("gulp-clean-css");

// HTML task
function html() {
  return gulp
    .src('./src/*.{html,php}')
    .pipe(fileinclude({
      prefix: '@@',
      basepath: '@file'
    }))
    .pipe(gulp.dest('./dist'));
}

// CSS task
function css() {
  return gulp
    .src("./src/css/*.css")
    .pipe(autoprefixer({
      cascade: false
    }))
    .pipe(rename({
      suffix: ".min"
    }))
    .pipe(cleanCSS())
    .pipe(gulp.dest("./dist/css"));
}

exports.default = gulp.series(html, css);
```
Where each installed plugin is included with require() and tasks are then defined using functions from the installed plugins. Note that functionality from multiple plugins can exist in a single task.

Tasks should be exported so that they are publicly available to run in a command prompt. The above tasks are exported as a series using the `gulp.series` function. When this is run, it will run each of these tasks one by one. 

The following command will run the exported gulp tasks:
```
gulp default
```
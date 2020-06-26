# INFLOWSEM + Gulp

The site utilises gulp.js and and cannot be properly worked on without some knowledge of how it works. To get a basic understanding of how gulp is used in the development of web applications, see [gulp]() - an earlier section within these docs.

## Set up

The directory for the site is as follows:

```
inflowsem/
├── node_modules/
├── dist/
├── src/
├── gulpfile.js
├── package.json
└── webpack.config.js
```
### node_modules/: 

The node modules folder contains all the site's dependencies, installed through npm. You don't need to create this folder - it automatically generates when you install your first plugin. To use plugins in your code, simply `require('pluginName')` the plugin. Note that you don't have to reference the node_modules folder, i.e. something like `./node_modules/--/--`, you just put the name of the plugin itself. 

### dist/:

Short for 'distributable', this is the version of the site that is used in production. It is created from the src folder, after all the different tasks, defined in `gulpfile.js`, have been executed. Note that if you click on any of the html files within the dist folder, it will show you exactly what the page is supposed to look like. This isn't the same for the files within the src folder.

### src/:

The source folder contains all the raw code for the site, and is the version that is used when you want to edit it. If you open one of the html files within this folder, it unfamiliar and incomplete. This is because none of the tasks defined in `gulpfile.js` have been executed yet, i.e. the `gulp-file-include` plugin hasn't had a chance to run so there's a lot of missing html, like the navigation bar and footer. 

### gulpfile.js

The gulpfile for INFLOWSEM is straight forward and similar to others that you might have come across. The plugins are listed at the top, followed by numerous task functions - these are then exported as a series of tasks so that the `gulp default` command will run them one after another. 

You will notice that a lot of the tasks simply move one folder from `src` to `dist`, like the following code which moves all the files in `./src/videos` to `./dist/videos`:
```js
function videos() {
  return gulp
    .src('./src/videos/**/*')
    .pipe(gulp.dest('./dist/videos'))
}
```
This is a minor problem with this approach; it could be solved by using an assets folder to store all the videos, pdfs, images, tools etc. The important thing to note is that if you create a new folder within the src folder, and you want it's required in production, you'll need to create a task that takes the files from the folder and puts them in the dist folder. You also have to add the function to the following line of code at the bottom of the gulpfile:
```js
exports.default = gulp.series(clean, html, css, images, ...);
``` 

### package.json

See [gulp](NEED TO LINk) to learn a about this - but you can more or less disregard this file, since you don't really need to do anything with it. It's just used to manage your dependencies (which is done automatically).  

### webpack.config.js

Webpack is a JavaScript module bundler. It's main purpose is to take all of your JavaScript modules and bundle them into a single file for usage in a web browser. This config file is used in the gulpfile, and simply states the entry point for the application (`./src/js/main.js`). You shouldn't have to do anything with this file.  

## Utilisation

### Plugins

There are numerous plugins used to transform the source files:

- [gulp-file-include](https://www.npmjs.com/package/gulp-file-include): including files - used for including common html features, like navigation bars and footers, across all the pages. More on this below. 
- [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer): parses the CSS file and adds vendor prefixes (i.e. `-webkit-`, `-moz-`, `-ms-`).
- [gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css): minifies css.
- [del](https://www.npmjs.com/package/del): deletes files and directories - used for deleting `dist` directory after `gulp default` is entered. 
- [gulp-rename](https://www.npmjs.com/package/gulp-rename): rename files - used for renaming `main.css` to `main.min.css`, once minified.
- [gulp-babel](https://www.npmjs.com/package/gulp-babel): converts ES6 + ES7 code into a backwards compatible version of JavaScript used in current and older browsers (mainly Internet Explorer). To leanr more about this, see [Babel](https://babeljs.io/docs/en/).
- [webpack](https://www.npmjs.com/package/webpack): bunldes JavaScript modules into a single file for usage in web browers. More on this below. 
- [webpack-stream](https://www.npmjs.com/package/webpack-stream): allows webpack to be used in gulp. 

### File Include

Since the site doesn't use a framework like React or Laravel, it can be time consuming to maintain or update parts of the site since the pages share a lot of common features. For example, if you want to update a link in the navigation bar, you would have to go into each page and update it, since the navigation bar is present on each page. This is where [gulp-file-include](https://www.npmjs.com/package/gulp-file-include) has been very useful. 

The partial bits of html are in `src/partials`, and are any features that are used across multiple pages. Note that this folder is not included in `gulpfile.js` as it doesn't need to be available in the production version of the site - `gulp-file-include` has taken the source files and included the partial html files.

The following code shows how the partial html file is included in the source html file:
```html
@@include('./partials/_footer.html')
```

You can also pass variables to partial html files. This is useful for `_head.html`, which has a dynamic page title. 
```html
<!-- ./partials/_head.html -->
<title>@@title</title>

<!-- ./about.html -->
@@include('./partials/_head.html', {"title": "About • INFLOWSEM"})
```

This is better explained in the official docs, see [gulp-file-include](https://www.npmjs.com/package/gulp-file-include).

#### JavaScript Modules

You will notice that the JS files in `src/js` are separated into modules, with most of the modules being imported into `main.js`. This, hopefully, makes the site easier to maintain as the JS for specific features is contained in it's own file, making it quicker to access and easier to work on. 

This is achieved through bundling `main.js` using [webpack](https://webpack.js.org/). 

Note that if you go to `dist/js`, the modules are not there. This is because they have been bundled together in `main.js`.

In addition to importing your own modules, you can import other code libraries which can provide extra functionality. E.g. in `js/modules/bookingCalendar.js`, moment, a popular library for parsing and formatting dates, has been imported.

Libraries can be installed using [npm](https://www.npmjs.com/package/moment). Simply find the library you would like to install, and enter the install command, e.g. `npm i moment`. These are installed into the node_modules folder. To reference them in your own code, you just need to include the name and not the file path:
```js
import moment from 'moment';
``` 

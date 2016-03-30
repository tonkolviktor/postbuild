# A command line utility to inject css and js files into an html file or to remove code based on conditions.
Inspired by [gulp-inject](https://www.npmjs.com/package/gulp-inject) and [gulp-remove-code](https://www.npmjs.com/package/gulp-remove-code) and written because I could not find a command-line equivalent that could work with npm scripts instead of gulp.

This module injects css and javascript files between markers placed in an html file or removes code between markers.
It can take a single file or a directory of files which will be transformed to strings and injected.
It also removes code between markers so you can for example remove livereload code which is not needed in a production environment.

**This module was written in ES6 and requires node >= 4.0.0.**

## Usage
Place the markers in your html file to inject or remove code:

```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Home</title>

     <!-- inject:css -->
     <link rel="stylesheet" href="/path/to/styles.css">
     <!-- endinject -->

   </head>
   <body>

     <!-- inject:js -->
     <script src="/path/to/build.js"></script>
     <!-- endinject -->

     <!-- remove:production -->
     <script src="http://localhost:35729/livereload.js?snipver=1"></script>
     <!-- endremove -->

   </body>
   </html>
```
The css and javascript files will be injected between the markers and the scripts and stylesheets that are already there will be removed.
This way you can replace the regular files used in a development environment with the built files (bundled, minified, versioned) in a production environment.

 Options:

    -h, --help             output usage information
    -V, --version          output the version number
    -i, --input <input>    Input file
    -o, --output <output>  Output file
    -c, --css <css>        css file(s) to inject (file or directory)
    -j, --js <js>          js file(s) to inject (file or directory)
    -r, --remove <remove>  Remove condition

### Examples

Given a directory:

```
-- dist
   -- css
      styles.1234abc.css
   -- js
      vendor.js
      build.4567def.js
```
`postbuild -i index.html -o dist/index.html -c dist/css -j dist/js -r production`

will result in the file `dist/index.html`:

```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Home</title>

     <!-- inject:css -->
     <link rel="stylesheet" href="dist/css/styles.1234abc.css">
     <!-- endinject -->

   </head>
   <body>

     <!-- inject:js -->
     <script src="dist/js/vendor.js"></script>
     <script src="dist/js/build.4567def.js"></script>
     <!-- endinject -->

   </body>
   </html>
```

*Notice that the code*
```
     <!-- remove:production -->
     <script src="http://localhost:35729/livereload.js?snipver=1"></script>
     <!-- endremove -->
```
*has been removed.*

Specifying a single file instead of a directory will only inject that single file:

`postbuild -i index.html -o dist/index.html -c dist/css -j dist/js/build.4567def.js -r production`

results in:

```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Home</title>

     <!-- inject:css -->
     <link rel="stylesheet" href="dist/css/styles.1234abc.css">
     <!-- endinject -->

   </head>
   <body>

     <!-- inject:js -->
     <script src="dist/js/build.4567def.js"></script>
     <!-- endinject -->

   </body>
   </html>
```

*Notice that `vendor.js` is not injected.*

### Tests

Run `npm test` to run the tests.

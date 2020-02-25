# @kuzivany/gulp-sass

Sass plugin for [Gulp](https://github.com/gulpjs/gulp).

**_A fork of [Gulp Sass](https://github.com/dlmanning/gulp-sass/tree/7ab018e3678729392978311eef2cb02754650960) using [Dart Sass][] as the default compiler._**

# Install

## yarn

```
yarn add -D sass https://github.com/kuzivany/gulp-sass
```

## npm

```
npm install sass https://github.com/kuzivany/gulp-sass --save-dev
```

# Basic Usage

Something like this will compile your Sass files:

```javascript
'use strict';

var Fiber = require('fibers');
var gulp = require('gulp');
var sass = require('gulp-sass');

sass.compiler = require('sass');

gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass.sync({fiber: Fiber}).on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});

gulp.task('sass:watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
});
```

You can choose whether to use [Dart Sass][] or [Node Sass][] by setting the `sass.compiler` property. Dart Sass will be used by default, but it's strongly recommended that you set it explicitly for forwards-compatibility in case the default ever changes.

[Dart Sass]: http://sass-lang.com/dart-sass
[Node Sass]: https://github.com/sass/node-sass

Note that when using Dart Sass, **synchronous compilation is twice as fast as asynchronous compilation** by default, due to the overhead of asynchronous callbacks. To avoid this overhead, you can use the [`fibers`](https://www.npmjs.com/package/fibers) package to call asynchronous importers from the synchronous code path. To enable this, pass the `Fiber` class to the `fiber` option:

## Options

Pass in options just like you would for [Node Sass](https://github.com/sass/node-sass#options); they will be passed along just as if you were using Node Sass. Except for the `data` option which is used by gulp-sass internally. Using the `file` option is also unsupported and results in undefined behaviour that may change without notice.

For example:

```javascript
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
   .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
   .pipe(gulp.dest('./css'));
});
```

Or this for synchronous code:

```javascript
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
   .pipe(sass.sync({outputStyle: 'compressed'}).on('error', sass.logError))
   .pipe(gulp.dest('./css'));
});
```

## Source Maps

`gulp-sass` can be used in tandem with [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) to generate source maps for the Sass to CSS compilation. You will need to initialize [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) prior to running `gulp-sass` and write the source maps after.

```javascript
var sourcemaps = require('gulp-sourcemaps');

gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
  .pipe(sourcemaps.init())
  .pipe(sass().on('error', sass.logError))
  .pipe(sourcemaps.write())
  .pipe(gulp.dest('./css'));
});
```

By default, [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) writes the source maps inline in the compiled CSS files. To write them to a separate file, specify a path relative to the `gulp.dest()` destination in the `sourcemaps.write()` function.

```javascript
var sourcemaps = require('gulp-sourcemaps');
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
  .pipe(sourcemaps.init())
  .pipe(sass().on('error', sass.logError))
  .pipe(sourcemaps.write('./maps'))
  .pipe(gulp.dest('./css'));
});
```

# Issues

`gulp-sass` is a very light-weight wrapper around either [Dart Sass][] or [Node Sass][] (which in turn is a Node binding for [LibSass][]). Because of this, the issue you're having likely isn't a `gulp-sass` issue, but an issue with one those projects or with [Sass][] as a whole.

[LibSass]: https://sass-lang.com/libsass
[Sass]: https://sass-lang.com

If you have a feature request/question how Sass works/concerns on how your Sass gets compiled/errors in your compiling, it's likely a Dart Sass or LibSass issue and you should file your issue with one of those projects.

If you're having problems with the options you're passing in, it's likely a Dart Sass or Node Sass issue and you should file your issue with one of those projects.

We may, in the course of resolving issues, direct you to one of these other projects. If we do so, please follow up by searching that project's issue queue (both open and closed) for your problem and, if it doesn't exist, filing an issue with them.

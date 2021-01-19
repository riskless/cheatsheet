### GULP at a glance
- Just another Task Runner
	- similar to GRUNT
- Plenty of plugins (for tasks)
- Plugins/tasks are code driven
	- Configuration driven as in GRUNT
- Stream based execution
	- File based as in GRUNT
- Can be used with several Web Development tools
	-  including Visual studio
- Runs on Node JS
- A complete web workflow
	- Develop, "Watch", Build, Deploy etc.
- Open Source
- Cross platform

### What do you need
- Node js / npm (obviously)
- Gulp client (gulp-cli) - node package
	- Usually a global install
- Gulp - node package
	- Can be global or local installation
- Gulp plugins - node packages
	- Download/install as necessary
- Gulp code (for tasks) - JS file
	- gulpfile.js (default file name)

### Task Execution - GRUNT
- GRUNT Task with 3 Targets (or 3 inner tasks)
1. Code Quality Check (say JSHint)
	- source folder -> Read Files -> Process -> Write Files -> result folder
2. Concat files
	- Previous result folder ->  Read Files -> Process -> Write Files
3. Minify files
	- Previous result folder ->  Read Files -> Process -> Write Files -> result folder

### Task Execution - GULP
- GULP Task (Read once, Write once)
1. Code Quality Check (say JSHint)
	- Source Folder -> Read Files (into stream) -> Process(uses stream)
2. Concat files
	- Previous Process ->  Process(uses stream)
3. Minify files
	- Previous Process ->  Process(uses stream) -> Writing files (from stream) -> result folder (destination)

### Overview of simple Gulpfile
```js
var gulp = require('gulp');
var copy = require('gulp-contrib-copy');

gulp.task('copydir1', function() {
	gulp.src('dir1/**')
		.pipe(copy())
		.pipe(gulp.dest('target/'));
});
```

- var gulp = require('gulp');
	- Create gulp instance

- var copy = require('gulp-contrib-copy');
	- Create gulp plugin instance

-  gulp.task('copydir1', function(){});
	- Define gulp task (task name: copydir1)
	- Task name needs to be provided
	- Define function which needs to be executed when the task is invoked.

- gulp.src('dir1/**')
	- Define gulp source (loads to stream)

- pipe(copy())
	- Process stream (using gulp plugin)
	- Add any no. of processes to pipe

- pipe(gulp.dest('target/'))
	- Writes from stream
	- To Destination

### Simple hands-on
- install gulp
	- npm install -g gulp-cli
	- gulp -version
	- md myapp
	- cd myapp
	- npm init
	- npm install gulp --save
	- npm install gulp-contrib-copy --save

- create gulpfile.js
```js
var gulp = require('gulp');
var copy = require('gulp-contrib-copy');

gulp.task('copydir1', function() {
	gulp.src('src/**')
		.pipe(copy())
		.pipe(gulp.dest('target/'));
});
```

- execute gulpfile
	- gulp copydir1

- execute gulpfile without task name
```js
var gulp = require('gulp');
var copy = require('gulp-contrib-copy');

gulp.task('default', function() {
	gulp.src('src/**')
		.pipe(copy())
		.pipe(gulp.dest('target/'));
});
```
- gulp

### to concatenate JS files using Gulp.js (using 'gulp-concat')
- intallation
	- npm install glup --save
	- npm intall gulp-concat --save

- gulpfile.js
```js
var gulp = require('gulp');
var concat = require('gulp-concat');

gulp.task('t1', function() {
	return gulp.src('src/**/*.js')
		.pipe(concat('all.js'))
		.pipe(gulp.dest('dest'));
});
```
- gulp t1


### to monitor JS file changes using Gulp Watch
```js
var gulp = require('gulp');
var concat = require('gulp-concat');

gulp.task('t1', function() {
	return gulp.src('src/**/*.js')
		.pipe(concat('all.js'))
		.pipe(gulp.dest('dest'));
});

gulp.task('w1', function() {
	gulp.watch('src/**/*.js', ['t1']);
});
```
- gulp w1

### to minify (uglify) JS files using Gulp.js (using 'gulp-uglify')
- npm install gulp-uglify --save

```js
var gulp = require('gulp');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');

gulp.task('t1', function() {
	return gulp.src('src/**/*.js')
		.pipe(concat('all.min.js'))
		.pipe(gulp.dest('dest'));
});

gulp.task('w1', function() {
	gulp.watch('src/**/*.js', ['t1']);
});
```
- gulp w1
### References
- [Gulp JS](https://www.youtube.com/watch?v=mbRlB42vQP8)
- [Gulp Watch](https://www.youtube.com/watch?v=xTiHlpuoOd0&list=PLvZkOAgBYrsRmqqKk6W1O4HKVrlaZsPAc&index=5)

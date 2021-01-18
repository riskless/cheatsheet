### General (Web UI) Development Tasks
- Minify CSS files
- Minify JS files
- Minify images
- Compile SASS/LESS to CSS & minify
- Combine all JS files into one
- Combine all CSS files into one
- Create deployable packages of your website
	- Includes copying from source to target folders
- Many others...

### Automating tasks
- Performing tasks - Different ways
	- Batch files (scripts)
	- Shell/Powershell scripts
	- 3rd party tools/utilities
	- Your own developed utilities
- nothing but "task runners"
	- GRUNT - yet another task runner (The JavaScript Task Runner)

### Why GRUNT?
- Geared specially for web development
- Plenty of plugins (for tasks)
- Plugins/tasks are configuration driven
- Invoked from command line (command prompt)
- Can be used with several Web Development tools
	- Including Visual Studio, Sublime, Webstorm ...
- Runs on Node JS
- A complete web workflow
	- Develop, "Watch", Build, Deploy etc.
- Open Source
- Cross platform

### What do you need
- Node js / npm (obviously)
- Grunt client(grunt-cli) - node package
	- Usually a global install
- Grunt - node package
	- Can be global or local installation
- Grunt plug-ins - node packages
	- Download/install as necessary
- Grunt configuration - JS file
	- gruntfile.js (default file name)

### Working with grunt
- install necessary node packages
- Create Grunt (config) file
	- gruntfile.js
		- Entry point for the GRUNT task runner
		- Node modules
		- Loads plug-ins
		- Defines & configures tasks / targets
- Execute grunt file
	- From command prompt
	- Or from within your (supported) web development toos

### Overview of a simple Gruntfile
- gruntfile.js
```js
module.exports = function (grunt) {
	grunt.initConfig({
		pkg: grunt.file.readJSON('package.json'),
		copy:{
			t1: {
				src: 'dir1/*',
				dest: 'dir2/'
			}
		}
		grunt.loadNpmTasks('grunt-contrib-copy');

		// default task
		grunt.registerTask('default', 'copy:t1');

		// default task alias
		grunt.registerTask('t1', 'copy:t1');
	});
};
```

- module.exports = function(grunt){}
	- Node Module defintion
	- "grunt" object is automatically instantiated by "GRUNT" runtime
	- Usually saved as "gurntfile.js"

- pkg: grunt.file.readJSON('package.json')
	- To load all the necessary node/grunt modules to execute the tasks

- copy:{}
	- Grunt task configurations and options
	- In this case, "copy" is a task, loaded through a plugin called "grunt-contrib-copy"
	- Can have multiple tasks listed here

- t1:{}
	- Target of a task
		- A task executed in a context of options
	- At least, one target is usually expected
	- Can have multiple flavors (contexts) of the same task to be expressed in the form of multiple targets.

- grunt.loadNpmTasks('grunt-contrib-copy')
	- Plugins list to be loaded, to help with tasks

### Simple example 
- install grunt
	- node -v
	- npm -v
	- grunt -v
	- npm install -g grunt-cli
	- grunt -v
	- npm init
	- npm install grunt --save
	- npm install grunt-contrib-copy --save
- create gruntfile.js
- execute grunt task
	- grunt --gruntfile gruntfile.js copy
	- grunt copy (default: gruntfile.js)
	- grunt copy:t1 (a specific target)
	- grunt (default task -> grunt.registerTask('default', 'copy:t1');)
	- grunt t1 (default task alias -> grunt.registerTask('t1', 'copy:t1');)

### Minifying (or uglifying) files using Grunt JS
- install grunt
	- npm install grunt --save

- install grunt-contrib-uglify plugin 
	- npm install grunt-contrib-uglify --save

- index.html
```html
<script src="src/app.js"></script>
<script src="src/one.js"></script>
<script src="src/t/two.js"></script>

<input type="button" onclick="app.firstMethod()" value="First Message">
<input type="button" onclick="app.secondMethod()" value="Second Message">
```

- app.js
```js
var app = {};
```

- one.js
```js
app.firstMethod = function() {
	alert("First Method");
}
```

- two.js
```js
app.secondMethod = function() {
	debugger;
	alert("Second Method");
}
```

- to minify JS files (and combine/concat) into a single file using Grunt JS
```js
module.exports = function (grunt) {
	grunt.loadNpmTasks('grunt-contrib-uglify');
	grunt.initConfig({
		pkg: grunt.file.readJSON('package.json'),
		uglify: {
			t1: {
				files:{
					'dest/all.min.js': ['src/app.js','src/one.js', 'src/t/two.js']
				}
			}
		}
	});
};
```
- grunt uglify:t1

- index.html
```html
<script src="dest/all.min.js"></script>

<input type="button" onclick="app.firstMethod()" value="First Message">
<input type="button" onclick="app.secondMethod()" value="Second Message">
```

- to create and use 'sourcemaps' feature, while minifying Javascript files using Grunt JS 
```js
module.exports = function (grunt) {
	grunt.loadNpmTasks('grunt-contrib-uglify');
	grunt.initConfig({
		pkg: grunt.file.readJSON('package.json'),
		uglify: {
			t1: {
				options: {
					sourceMap: true // creating all.min.js.map
				}, 
				files:{
					'dest/all.min.js': ['src/app.js','src/one.js', 'src/t/two.js']
				}
			}
		}
	});
};
```
- grunt uglify:t1

- to minify JS files retaining the folder structure using Grunt JS
```js
module.exports = function (grunt) {
	grunt.loadNpmTasks('grunt-contrib-uglify');
	grunt.initConfig({
		pkg: grunt.file.readJSON('package.json'),
		uglify: {
			t2: {
				files:[{
					cwd: 'src/',	// cwd: Current Working Directory
					src: '**/*.js',
					dest: 'target/',
					expand: true,
					flatten: false,
					ext: '.min.js'
				}]
			}
		}
	});
};
```
- grunt uglify:t2

- index.html
```html
<script src="target/app.min.js"></script>
<script src="target/one.min.js"></script>
<script src="target/t/two.min.js"></script>

<input type="button" onclick="app.firstMethod()" value="First Message">
<input type="button" onclick="app.secondMethod()" value="Second Message">
```

###  Grunt Watch 
- Installing and working with grunt-contrib-watch plug-in 
	- npm install grunt-contrib-watch --save

- Monitor JS files and minify them automatically using Grunt Watch
```js
module.exports = function (grunt) {
	grunt.loadNpmTasks('grunt-contrib-uglify');
	grunt.loadNpmTasks('grunt-contrib-watch');
	grunt.initConfig({
		pkg: grunt.file.readJSON('package.json'),
		uglify: {
			t1: {
				files:{
					'dest/all.min.js': ['src/**/*.js']
				}
			}
		},

		watch: {
			w1: {
				files: ['src/**/*.js'],
				tasks: ['uglify:t1']
			}
		}
	});
};
```

- grunt watch:w1

### References
- [Grunt JS](https://www.youtube.com/watch?v=sS4sTrd57t8 "Grunt JS")
- [How to minify (or uglify) JavaScript files](https://www.youtube.com/watch?v=Gkv7pA0PMJQ&list=PLvZkOAgBYrsRmqqKk6W1O4HKVrlaZsPAc&index=3)
- [Understanding Grunt Watch](https://www.youtube.com/watch?v=dKZOOj6ruiE)

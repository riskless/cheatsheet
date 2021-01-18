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


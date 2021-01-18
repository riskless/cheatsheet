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

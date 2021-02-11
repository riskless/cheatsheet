
### What is Angular CLI
- CLI stands for Command Line Interface. So Angular CLI is command line tool the help us 
- Create Angular applications faster and with great consistency
- Create the boiler plate code for angular features like components, directives, pipes, services etc.
- Create boiler plate code for TypeScript features like classes, interfaces, enums etc.
- It follows angular best practices and conventions out of the box
- Run Unit and End-to-End (e2e) tests
- Create optimised builds for deployment to production

### Installing Angular CLI
- node -v
- npm -v
- npm install -g @angular/cli // npm i -g @angular/cli
- ng -v // To verify the version of Angular CLI installed

### To create a new Angular Project
- ng new your_app_name
- what did this "ng new" command do
	- A new folder with name MyFirstApp is created
	- All the required configuration and source files are created.
	- All the npm dependencies are installed in node_modules folder
	- Unit and end-to-end tests are created
	- The Karma unit test runner is configured
	- The Protractor end-to-end test framework is configured
- To run the project using Angular CLI
	- ng serve --open
- To stop the server
	- press CTRL + C
- To run all the unit tests
	- ng test
- To run all the end-to-end tests
	- ng e2e

### Customize Command Prompt
- displays all the Angular CLI commands
	- ng --help
- To redirect the output of ng --help command to the windows clipboard
	- ng --help | clip
- redirect the output directly to a text document
	- ng --help > file_name // myDoc.txt

### ng new options
1. --dry-run (-d): Run through without making any changes. Just reports the files that will be created
2. --skip-install	(-si): Skip installing packages
3. --skip-tests (-st): Skip creating tests
4. --inline-style (-is): Use inline styles when generating the new application
5. --inline-template (-it):	Use inline templates when generating the new project

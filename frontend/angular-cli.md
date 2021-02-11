
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

### Angular CLI project structure
- node_modules	
	- The packages specified in package.json file are installed into this folder when we run npm install command
- e2e Folder	
	- Contains end-to-end tests and their configuration files. We will discuss end-to-end tests in our upcoming videos
- tsconfig.json
	- This is the TypeScript compiler configuration file. This file has several TypeScript compiler configuration settings. For example, to compile TypeScript to JavaScript on saving a TypeScript file set compileOnSave setting to true. If you do not want .map files to be generated, set sourceMap to false. .map files are used for debugging your application.
- tslint.json
	- Angular has a linting tool that checks our TypeScript code for programmatic and stylistic errors as well as non-adherence to coding standards and conventions. tslint.json is the configuration file for linting. 
- assets
	- As the name implies, the assets folder contains the assets of your application like images and anything else to be copied when you build your application
- environments
	- This folder contains the environment files. By default we have 2 environment files. environment.ts is for for development environment. Notice production property in this file is set to false. environment.prod.ts is for production. Notice in this file production property is set to true as expected. The build system defaults to the dev environment which uses `environment.ts`, but if we do a production build environment.prod.ts will be used.
- favicon.ico
	- This is the favorite icon for your application which is typically displayed in the browser address bar and next to the page name in a list of bookmarks. Angular CLI provides this favorite icon out of the box. You may replace this favicon with your own company favicon
- index.html
	- The main HTML page that is served when someone visits your site
- main.ts	
	- The main entry point for the application. This file contains the code to bootstrap the application root module (AppModule)
- polyfills.ts
	- This is the polyfills file. Angular is built on the latest standards of the web platform. Targeting such a wide range of browsers is challenging because not all browsers support all features of modern browsers. This can be compensated by using polyfill scripts as they implement the missing features in JavaScript. So these polyfills allow us to use an API regardless of whether it is supported by a browser or not

- styles.css	
	- This file contains the global styles of our application. Styles that are local and specific to a component are often defined with in the component itself for easier maintenance
- test.ts	
	- This file is the main entry point for unit tests and loads all the .spec and framework files


### generating components
- ng generate component ComponentName // ng g c ComponentName
- When we execute this command (ng g c abc)
	- A folder with name abc is created
	- The component files (Component class, View template, CSS file and the spec file ) are created and placed inside the folder "abc"
	- The root module file (app.module.ts) is also updated with our new component i.e the required import statement to import the abc component from the component file is included and the component is also declared in the declarations array of the @NgModule() decorator
- Placing the generated component folder in a different folder
	- ng g c abc/xyz
- Generating a new component without a folder
	- ng g c pqr --flat
- Placing the flat component files in a different folder other than app
	- ng g c abc/jkl --flat
- ng g c ghi -d
	- it only reports the files it is going to generate, without actually generating them. 
- ng g c ghi -it -is --spec=false -d
	- If you want an inline template and styles instead of an external template and stylesheet, use -it flag for inline template and -is flag for inline styles. 
	- Along the same lines, if you do not want a spec file use --spec=false.
- ng g c ghi -d  --style=scss
	- To use sass instead of CSS with your component, use the --style=scss flag with ng generate command. If you want less use --style=less


### generating services
- ng generate service serviceName OR ng g s serviceName
- ng generate service employee -module=app.module
	- The above command not only generates employee service, it also registers our service witht the AppModule
- If you do not want the spec file
	- ng g s student -d --spec=false

### generating modules
- To generate a module
	- ng generate module moduleName OR ng g m moduleName
- ng g m students -d -m=app.module
	- The above command not only generates students module, it also imports it into the root module (AppModule)
- ng g m students -d -m=app.module --spec=true
	- By default a spec file is not generated. If you also want a spec file to be generated use the --spec option
- ng g m students -d -m=app.module --spec=true --flat=true
	- When generating a module, Angular CLI by default creates a folder for the module and places the module files in that folder. 
	- If you do not want a dedicated folder for the module you are generating, use --flat option.

### generating directives, pipes and routing guards
- Generating directives
	- ng generate directive directiveName	 or ng g d directiveName
- Generating pipes
	- ng generate pipe pipeName	or ng g p pipeName
- Generating routing guards
	- ng generate guard guardName	or ng g g guardName
- When you try to generate a directive, pipe or a component, and if you have multiple modules in your angular project you may get the following error
	- ng g d directiveName --skip-import -d
		- Use --skip-import option to tell Angular CLI not to import and register the generated component, directive or pipe 
	- ng g d directiveName -m=app.module -d
		- Use --module option or it's alias -m to tell Angular CLI the module with which we want our newly generated component, directive or pipe should be registered.  
	- When genearting certain angular features like services or routing guards, you will not get this error, even when you have multiple modules in your project, because by default, Angular CLI does not try to import and register these features.

### generating class, interface and enum
- To generate a class
	- ng generate class className or ng g cl className
- ng g cl employee/employee
	- The command above creates a folder with name "employee" and then creates the "employee" class in it.
- ng g cl employee/employee --spec=true
	- By default, a spec file is not created for the class. If you want a spec file to be generated set --spec option to true.
- To generate an interface use
	- ng generate interface interfaceName or ng g i interfaceName
- To generate an enum use
	- ng generate enum enumName or ng g e enumName

### Linting TypeScript
- Angular has a linting tool that checks our TypeScript code for programmatic and stylistic errors as well as non-adherence to coding standards and conventions. - tslint.json is the configuration file for linting. This file contains all the default rules for linting our code.
- to lint the code
	- ng lint
- ng lint --type-check
	- if 'no-use-before-declare' rule is enabled we need to use --type-check option with the ng lint command
	- 'no-use-before-declare' rule is enabled out of the box and it disallows usage of variables before their declaration.
- "no-var-keyword" rule is also enabled by default. Turn this rule off by setting it's value to false in tslint.json
	- "no-var-keyword": true
- some of the common linting rules
	- quotemark rule specifies whether you want single or double quotes
	- no-trailing-whitespace rule disallows trailing whitespace at the end of a line
	- semicolon rule specifies that a line should be terminated with a semicolon
	- comment-format rule specifies that all single-line comments must begin with a space
	- component-class-suffix rule enforces that a component class should end with the suffix Component
	- use-life-cycle-interface rule enforces that you add the implements keyword for every lifecycle hook you use
- ng lint --fix
	- Some of the linting errors support automatic fix. To have these linting errors fixed automatically, run ng lint command with the --fix option.
- ng lint --help
	- To see the options that can be used with ng lint command


### generating routing module
- ng new app_name --routing
	- To make Angular CLI generate a routing module

### ng serve options
- ng serve --help
	- To see the list of all options that we can use with "ng serve" command
- ng serve --open
	- builds and launches the application in your default browser
- options
1. --watch (-w):	Run build when files change
2. --live-reload (-lr): Whether to reload the page on change
3. --open	(-o):	Opens the url in default browser
4. --port	(-p): The port on which the server is listening
5. --extract-css (-ec):	Extract css from global styles onto css files instead of js ones

### Compile angular app
- ng serve
	- Compiles and serves the application from memory
	- Does not write the build files to the disk
	- Typically used to run the application on local development machine
	- Cannot be used for deploying the build to another server (Ex. Testing, Staging or Production server)
- ng build
	- Compiles the application to the "dist" folder
	- Can be used to produce both development & production builds
	- Typically used to deploy the application on another server

### dev build vs prod build
- To generate a development build
	- ng build OR ng build --dev
- To generate a production build
	- ng build --prod
- Source Maps: Development build generate Source Maps where as production build does not. 
	- ng build --dev -sm false // --sourcemaps
		- The above command produces a development build without source maps
	- ng build --prod -sm true
		- if you want source maps along with your production build
- Extracts CSS: With the development build global styles are extracted to .js files where as with the production build they are extracted to .css files.
	- ng build --dev -ec true // --extract-css
		- The above command produces a development build with global styles extracted to .css file(s) instead of .js ones.
- Minification & Uglification: A Prod Build is both minified and uglified, where as a Dev Build is not.
	- The process of removing excess whitespace, comments, and optional tokens like curly brackets and semicolons is called Minification.
	- The process of transforming code to use short variable and function names is called uglification.
- Tree Shaking : A Prod build is Tree Shaked, where as a Dev build is not.
	- Tree shaking is the process of removing any code that we are not actually using in our application from the final bundle. It's one of the most effective techniques to reduce the application size.
- Ahead-of-Time (AOT) Compilation
	- With a production build we get AOT (Ahead-of-Time) compilation, i.e the Angular component templates are pre-compiled, where as with a development build they are not. 

### Ahead-of-Time compilation and Just-in-Time compilation
- JIT: JIT compilation as the name implies, compiles the application Just-in-Time in the browser at runtime.
- AOT: AOT compilation compiles the application at build time.
- to inspect the JavaScript bundles
	- npm install source-map-explorer --save-dev
	- node_modules\.bin\source-map-explorer dist\vendor.bundle.js
- By default, the following 2 commands use JIT compilation 
	- ng build
	- ng serve
- to turn on Ahead-of-Time compilation
	- ng build --aot
	- ng serve --aot
- to turn off AOT for the production build
	- ng build --prod --aot false

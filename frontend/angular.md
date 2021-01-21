### SPA (Single Page Application)
- A web application that fits in a single page and dynamically update that page as the user interacts with the app!
- Advantages of SPA
	- Saves bandwidth as you are only sending data without decoration.
	- Better user experience as it doesn't require a page reload.
	- Client and server logic can evolve independently.

### Angular
- Angular is a platform and framework for building client applications in HTML and TypeScript.
- 'AngularJS' was first released in 2010.
- Developed and maintained by Google.
- 'Angular' is successor of 'AngularJS' and was released in 2016.
- 'Angular' is not an enhancement of 'AngularJS'. It is a complete rewirte of a new framework with a host of features.
- What we need to install
	1. Intall Node.js (comes with NPM Command Interface)
		- Node.js is a JavaScript runtime environment that executes JavaScript code outside of a browser. We need Node.js to use our JavaScript Libraries.
	1. Install Angular CLI
		- Generate angular applications
		- Generate components, services, etc.
		- Build applications.
	1. Install Visul Studio Code (IDE)

### Setup and getting started steps
1. Install Node.js (From www.nodejs.org)
2. Open terminal and install 'Anuglar CLI' using the command
- npm install -g @angular/cli
3. Go to your favourite directory and create a new Angular workspace using Angular CLI command
- ng new app-name
	- Creates a new workspace and an initial skeleton application
	- Installs necessary npm packages and other dependencies
	- Creates end-to-end tests for initial application
4. Install 'Visual Studio Code' and import workspace
5. Launch the app using Angular CLI
- ng serve --open

### NPM (Node Package Manager)
- Easily manage project dependencies
- NPM Repository hosts libraries that are useful for development
	- TypeScript to JavaScript Transpiler
	- Angular CLI module
	- Minification library
### Webpack
- Manages the dependencies
- Reduce the number of round trips required to run the app.
- Doesn't include the code that is not used.
- Increase performance
- Improves load time

### JIT (Just In Time Compiler)
- Loads the application slower as it compiles the application when running for the first time.
- Requires additional libraries to compile and hence more bundle size.
- Suitable for local development not for production.
- Less secure as original code can be seen.

### AOT (Ahead of Time Compiler)
- Compiled while building the app
- No need to deploy the compiler reducing the bundle size and load time.
- Suitable for production environment
- More secure as the orginal source code is not visible.
- ng serve --aot=true

### data-binding 
1. One way data-binding
- From Component to View Template
- With interpolation, we place the component property name in the view template, enclosed in double curly braces: {{propertyName}}
- The expression that is enclosed in double curly braces is commonly called as Template Expression.
- Interpolation is a special syntax that Angular converts into a property binding. Interpolation is just a convenient alternative to property binding. 
- Example
```js
<h1>{{'Name = ' + firstName}}</h1>
<h1>{{'Name = ' + firstName}}</h1>
<h1>{{ 10 + 20 + 30 }}</h1>
<h1>{{firstName ? firstName : 'No name specified'}}</h1>
<img src='{{imagePath}}'/>
<img src='http://haroldkim.site/{{imagePath}}' />
<h1>{{'Full Name = ' + getFullName()}}</h1>

// property binding
<img [src]='imagePath'/>
<button [disabled]='isDisabled'>Click me</button>

// We can also use the alternate syntax with bind- prefix. This is known as canonical form
<button bind-disabled='isDisabled'>Click me</button>
```

2. One way data-binding
	- From View Template to Component
3. Two way data-binding	
	- From Component to View Template & From View template to Component

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

### Data Binding 
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

### HTML element attribute and DOM (Document Object Model) property
- HTML attributes and the DOM properties are different things.
- When a browser loads a web page, the browser creates a Document Object Model of that page.
- DOM contains the HTML elements as objects, their properties, methods and events and it is a standard for accessing, modifying, adding or deleting HTML elements.
- Attributes are defined by HTML, where as properties are defined by the DOM.
- Attributes initialize DOM properties. Once the initialization complete, the attributes job is done.
- Property values can change, where as attribute values can't.
- Angular binding works with properties and events, and not attributes. Angular data-binding is all about binding to DOM object properties and not HTML element attributes.
- The role of attributes is to initialize element properties and their job is done.

### Angular attribute binding
- In some situations we want to be able to bind to HTML element attributes.For example, colspan and aria attributes does not have corresponding DOM properties.
```js
columnSpan: number = 2;

// property binding
// error: Can't bind to 'colspan' since it isn't a known property of 'th'
<th [colspan]="columnSpan">

// attribute binding
<th [attr.colspan]="columnSpan">
<th attr.colspan="{{columnSpan}}">
```

### CSS Class binding
```js
// style
.boldClass{
    font-weight:bold;
}

.italicsClass{
    font-style:italic;
}

.colorClass{
    color:red;
}

// template
classesToApply: string = 'italicsClass boldClass';

// html
// 'colorClass' is removed and these classes (italicsClass & boldClass) are added.
<button class='colorClass' [class]='classesToApply'>My Button</button>


/* Adding or removing a single class */
// component
applyBoldClass: boolean = false;

// template
// it does not remove the existing colorClass already added using the class attribute.
<button class='colorClass' [class.boldClass]='!applyBoldClass'>My Button</button>

// You can also removed an existing class that is already applied. The class binding removes the boldClass.
applyBoldClass: boolean = false;

<button class='colorClass boldClass italicsClass' [class.boldClass]='applyBoldClass'>My Button</button>

/* To add or remove multiple classes use ngClass directive  */
// Since both the keys (boldClass & italicsClass) are set to true, both classes will be added to the button element
<button class='colorClass' [ngClass]='addClasses()'>My Button</button>

// component
applyBoldClass: boolean = true;
applyItalicsClass: boolean = true;
addClasses() {
		let classes = {
				boldClass: this.applyBoldClass,
				italicsClass: this.applyItalicsClass
		};
		return classes;
}
```

### Style binding 
```js
// component
isBold: boolean = true;
fontSize: number = 30;
isItalic: boolean = true;

addStyles() {
	let styles = {
		'font-weight': this.isBold ? 'bold' : 'normal',
		'font-style': this.isItalic ? 'italic' : 'normal',
		'font-size.px': this.fontSize
	};

	return styles;
}

// If the property 'isBold' is true, then font-weight style is set to bold else normal.
<button style='color:red' [style.font-weight]="isBold ? 'bold' : 'normal'">My Button</button>

// style property name can be written in either dash-case or camelCase. (font-size or fontWeight)

// unit extension
<button style='color:red' [style.font-size.px]="fontSize">My Button</button>

/* To set multiple inline styles use NgStyle directive */
<button style='color:red' [ngStyle]="addStyles()">My Button</button>

```

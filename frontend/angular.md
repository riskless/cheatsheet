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

### event binding
```html
<button (click)="onClick()">Click me</button>
<button on-click="onClick()">Click me</button>
```

```html
<table>
	<thead>
		<tr>
			<th attr.colspan="{{columnSpan}}">Employee Details</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>First Name</td>
			<td>{{firstName}}</td>
		</tr>
		<tr *ngIf='showDetails'>
			<td>Gender</td>
			<td>{{gender}}</td>
		</tr>
	</tbody>
</table>
<button (click)='toggleDetails()'>{{showDetails ? 'Hide' : 'Show'}} Details</button>
```

```js
export class EmployeeComponent {
	columnSpan: number = 2;
	firstName: string = 'Tom';
	gender: string = 'Male';
	showDetails: boolean = false;

	toggleDetails(): void {
		this.showDetails = !this.showDetails;
	}
}
```

### Two way data binding
- a combination of both Property Binding and Event Binding.
```html
Name : <input [value]='name' (input)='name = $event.target.value'>
Name : <input [(ngModel)]='name'>
```

```js
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [],
  imports: [
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

### ngFor
```html
<tr *ngFor='let employee of employees'>
	<td>{{employee.code}}</td>
	<td>{{employee.name}}</td>
	<td>{{employee.gender}}</td>
	<td>{{employee.annualSalary}}</td>
	<td>{{employee.dateOfBirth}}</td>
</tr>
<tr *ngIf="!employees || employees.length==0">
	<td colspan="5">
		No employees to display
	</td>
</tr>
```

```js
export class EmployeeListComponent {
	employees: any[] = [
		{
			code: 'emp101', name: 'Tom', gender: 'Male',
			annualSalary: 5500, dateOfBirth: '25/6/1988'
		},
		{
			code: 'emp102', name: 'Alex', gender: 'Male',
			annualSalary: 5700.95, dateOfBirth: '9/6/1982'
		}
	];
}
```

### ngFor trackBy
```js
/* trackBy */
// html
<tr *ngFor='let employee of employees; trackBy:trackByEmpCode'>

// component
trackByEmpCode(index: number, employee: any): string {
	return employee.code;
}

/* How to get the index of an item in a collection */
<tr *ngFor='let employee of employees; let i=index'>
	<td>{{employee.dateOfBirth}}</td>
	<td>{{i}}</td>
</tr>

/* Identifying the first and the last element in a collection */
<tr *ngFor='let employee of employees; let isFirst = first; let isLast = last'>
	<td>{{employee.dateOfBirth}}</td>
	<td>{{isFirst}}</td>
	<td>{{isLast}}</td>
</tr>

/* Identifying even and odd element in a collection */
<tr *ngFor='let employee of employees; let isEven = even; let isOdd = odd'>
	<td>{{employee.dateOfBirth}}</td>
	<td>{{isEven}}</td>
	<td>{{isOdd}}</td>
</tr>
```

### Pipes
- Transform data before display
- Built in pipes include lowercase, uppercase, decimal, date, percent, currency etc
```js
/* To apply a pipe on a bound property use the pipe character "|" */
<td>{{employee.code | uppercase}}</td>

/* We can also chain pipes */
<td>{{employee.dateOfBirth | date:'fullDate' | uppercase }}</td>

/* Pass parameters to pipe using colon ":" */
<td>{{employee.dateOfBirth | date:'dd/MM/y'}}</td>
```

### custom pipe
```js
/* EmployeeTitlePipe */
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
	name: 'employeeTitle'
})
export class EmployeeTitlePipe implements PipeTransform {
	transform(value: string, gender: string): string {
		if (gender.toLowerCase() == "male")
			return "Mr." + value;
		else
			return "Miss." + value;
	}
}

/* AppModule */
import { EmployeeTitlePipe } from './employee/employeeTitle.pipe'

@NgModule({
	imports: [BrowserModule],
	declarations: [EmployeeTitlePipe],
	bootstrap: [AppComponent]
})

export class AppModule { }

/* html */
// Employee name gets passed automatically.
<tr *ngFor='let employee of employees;'>
	<td>{{employee.code}}</td>
	<td>{{employee.name | employeeTitle:employee.gender}}</td>
</tr>
```

### component input properties
- Passing data from the parent component to the child component 
```js
/* child component = nested component */
import { Component, Input } from '@angular/core';
@Component({
	selector: 'employee-count',
	templateUrl: 'app/employee/employeeCount.component.html',
	styleUrls: ['app/employee/employeeCount.component.css']
})
export class EmployeeCountComponent {
	@Input()
	all: number;

	@Input()
	male: number;

	@Input()
	female: number;
}

/* parent component = container component */
import { Component } from '@angular/core';

@Component({
    selector: 'list-employee',
    templateUrl: 'app/employee/employeeList.component.html',
    styleUrls: ['app/employee/employeeList.component.css']
})
export class EmployeeListComponent {
	employees: any[];

	constructor() {
		this.employees = [
			{
				code: 'emp101', name: 'Tom', gender: 'Male',
				annualSalary: 5500, dateOfBirth: '6/25/1988'
			},
			{
				code: 'emp102', name: 'Alex', gender: 'Male',
				annualSalary: 5700.95, dateOfBirth: '9/6/1982'
			},
			{
				code: 'emp103', name: 'Mike', gender: 'Male',
				annualSalary: 5900, dateOfBirth: '12/8/1979'
			}
		];
	}

	getTotalEmployeesCount(): number {
		return this.employees.length;
	}

	getMaleEmployeesCount(): number {
		return this.employees.filter(e => e.gender === 'Male').length;
	}

	getFemaleEmployeesCount(): number {
		return this.employees.filter(e => e.gender === 'Female').length;
	}
}

/* html (parent) */
<employee-count 
	[all]="getTotalEmployeesCount()"
	[male]="getMaleEmployeesCount()"
	[female]="getFemaleEmployeesCount()">
</employee-count>
```

### component output properties
- how to pass data from the child component to the parent component
```js
/* child component */
// Import Output and EventEmitter
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
	selector: 'employee-count',
	templateUrl: 'app/employee/employeeCount.component.html',
	styleUrls: ['app/employee/employeeCount.component.css']
})
export class EmployeeCountComponent {
	@Input()
	all: number;

	@Input()
	male: number;

	@Input()
	female: number;

	// Holds the selected value of the radio button
	selectedRadioButtonValue: string = 'All';

	// The Output decorator makes the property an Output property
	// EventEmitter class is used to create the custom event
	// When the radio button selection changes, the selected
	// radio button value which is a string gets passed to the
	// event handler method. Hence, the event payload is string.
	@Output()
	countRadioButtonSelectionChanged: EventEmitter<string> = new EventEmitter<string>();

	// This method raises the custom event. We will bind this
	// method to the change event of all the 3 radio buttons
	onRadioButtonSelectionChange() {
		this.countRadioButtonSelectionChanged.emit(this.selectedRadioButtonValue);
	}
}

/* child html */
<span class="radioClass">Show : </span>
<input name='options' type='radio' value="All"
       [(ngModel)]="selectedRadioButtonValue"
       (change)="onRadioButtonSelectionChange()">
<span class="radioClass">{{'All(' + all + ')'}}</span>
<input name="options" type="radio" value="Male"
       [(ngModel)]="selectedRadioButtonValue"
       (change)="onRadioButtonSelectionChange()">
<span class="radioClass">{{"Male(" + male + ")"}}</span>
<input name="options" type="radio" value="Female"
       [(ngModel)]="selectedRadioButtonValue"
       (change)="onRadioButtonSelectionChange()">
<span class="radioClass">{{"Female(" + female + ")"}}</span>

/* parent component */
// This property keeps track of which radio button is selected
// We have set the default value to All, so all the employees
// are displayed in the table by default
selectedEmployeeCountRadioButton: string = 'All';

// Depending on which radio button is selected, this method updates
// selectedEmployeeCountRadioButton property declared above
// This method is called when the child component (EmployeeCountComponent)
// raises the custom event - countRadioButtonSelectionChanged
// The event binding is specified in employeeList.component.html
onEmployeeCountRadioButtonChange(selectedRadioButtonValue: string): void {
		this.selectedEmployeeCountRadioButton = selectedRadioButtonValue;
}

/* parent html */
<employee-count [all]="getTotalEmployeesCount()"
	[male]="getMaleEmployeesCount()"
	[female]="getFemaleEmployeesCount()"
	(countRadioButtonSelectionChanged)="onEmployeeCountRadioButtonChange($event)">

// Angular does not allow multiple structural directives to be placed on one element
<ng-container *ngFor="let employee of employees;">
	<tr *ngIf="selectedEmployeeCountRadioButton=='All' || selectedEmployeeCountRadioButton==employee.gender">
		<td>{{employee.code | uppercase}}</td>
		<td>{{employee.name | employeeTitle:employee.gender }}</td>
		<td>{{employee.gender}}</td>
		<td>{{employee.annualSalary | currency:'USD':true:'1.3-3'}}</td>
		<td>{{employee.dateOfBirth | date:'dd/MM/y'}}</td>
	</tr>
</ng-container>
```

### interface
- To create a custom type for our business object
- TypeScript interfaces exist for developer convenience and are not used by Angular at runtime. During transpilation, no JavaScript code is generated for an interface. It is only used by Typescript for type checking during development.
- To reduce the amount of code you have to write, consider using short-hand syntax to initialise class properties with constructor parameters

```js
/* IEmployee */
export interface IEmployee {
	code: string;
	name: string;
	gender: string;
	annualSalary: number;
	dateOfBirth: string;
	
	// To make a property optional use a ?
	// A class that implements this interface need not provide implementation for this property
	department?: string;

	computeMonthlySalary(annualSalary: number): number;
}

/* Employee */
export class Employee implements IEmployee {
	// All the interface mandatory properties are defined  
	public code: string;
	public name: string;
	public gender: string;
	public annualSalary: number;
	public dateOfBirth: string;

	// The above class properties are then initialized
	// using the constructor parameters. To do something
	// like this, TypeScript has a shorthand syntax which
	// reduces the amount of code we have to write
	constructor(code: string, name: string, gender: string,
		annualSalary: number, dateOfBirth: string) {
		this.code = code;
		this.name = name;
		this.gender = gender;
		this.annualSalary = annualSalary;
		this.dateOfBirth = dateOfBirth;
	}

	// Implementation of the interface method
	computeMonthlySalary(annualSalary: number): number {
		return annualSalary / 12;
	}
}

// Here is the shorthand syntax to initialise class properties with constructor parameters
export class Employee implements IEmployee {
	constructor(public code: string, public name: string, public gender: string,
		public annualSalary: number, public dateOfBirth: string) {
	}
}
```

### component lifecycle hooks
- A component has a lifecycle managed by Angular.
	- Creates the component
	- Renders the component
	- Creates and renders the component children
	- Checks when the component data-bound properties change, and 
	- Destroys the component before removing it from the DOM
- The 3 most commonly used hooks
	- ngOnChanges: Executes, every time the value of an input property changes. The hook method receives a SimpleChanges object containing current and previous property values. This is called before ngOnInit
	- ngOnInit: Executes after the constructor and after ngOnChange hook for the first time. It is most commonly used for component initialisation and retrieving data from a database
	- ngOnDestroy: Executes just before angular destroys the component and generally used for performing cleanup

```js
/* SimpleComponent */ 
// Step 1 : Import OnChanges and SimpleChanges
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

// The selector "simple" will be used as the directive
// where we want to use this component. Notice we are
// also using the simpleInput property with interpolation
// to display the value it receives from the parent
// component
@Component({
	selector: 'simple',
	template: `You entered : {{simpleInput}}`
})
// Step 2 : Implement OnChanges Life Cycle Hook interface
export class SimpleComponent implements OnChanges {
	// Input property. As and when this property changes
	// ngOnChanges life cycle hook method is called
	@Input() simpleInput: string;

	// Step 3 : Implementation for the hook method
	// This code logs the current and previous value to the console.
	ngOnChanges(changes: SimpleChanges) {
		for (let propertyName in changes) {
			let change = changes[propertyName];
			let current = JSON.stringify(change.currentValue);
			let previous = JSON.stringify(change.previousValue);
			console.log(propertyName + ': currentValue = '
					+ current + ', previousValue = ' + previous);
			// The above line can be rewritten using
			// placeholder syntax as shown below
			// console.log(`${propertyName}: currentValue
			// = ${current }, previousValue = ${previous }`);
		}
	}
}

/* AppComponent */
import { Component } from '@angular/core';

// Notice we have placed the text box in this root component
// To keep the value in the textbox and the component property
// value "userText" in sync we are using 2 way data binding
// We have also bound userText property of this component
// to the input property of the SimpleComponent
@Component({
	selector: 'my-app',
	template: `Your Text : <input type='text' [(ngModel)]='userText'/>
						 <br/><br/>
						 <simple [simpleInput]='userText'></simple>
						`
})
export class AppComponent {
	userText: string = 'Pragim';
}
```

### Service
- A service in Angular is generally used when you need to reuse data or logic across multiple components.
```js
/* EmployeeService */
import { Injectable } from '@angular/core';
import { IEmployee } from './employee';

// The @Injectable() decorator is used to inject other dependencies
// into this service. As our service does not have any dependencies
// at the moment, we may remove the @Injectable() decorator and the
// service works exactly the same way. However, Angular recomends
// to always use @Injectable() decorator to ensures consistency
@Injectable()
export class EmployeeService {
	getEmployees(): IEmployee[] {
		return [
			{
				code: 'emp101', name: 'Tom', gender: 'Male',
				annualSalary: 5500, dateOfBirth: '6/25/1988'
			},
			{
				code: 'emp102', name: 'Alex', gender: 'Male',
				annualSalary: 5700.95, dateOfBirth: '9/6/1982'
			}
		];
	}
}

/* EmployeeListComponent */
@Component({
	// Register EmployeeService in this component by
	// declaring it in the providers array
	providers: [EmployeeService]
})
// Make the class implement OnInit interface
export class EmployeeListComponent implements OnInit {
    employees: IEmployee[];

    selectedEmployeeCountRadioButton: string = 'All';

    // Inject EmployeeService using the constructor
    // The private variable _employeeService which points to
    // EmployeeService singelton instance is then available
    // throughout this class
    constructor(private _employeeService: EmployeeService) {
    }

    // In ngOnInit() life cycle hook call the getEmployees()
    // service method of EmployeeService using the private
    // variable _employeeService
    ngOnInit() {
        this.employees = this._employeeService.getEmployees();
    }

    getTotalEmployeesCount(): number {
        return this.employees.length;
    }

    getTotalMaleEmployeesCount(): number {
        return this.employees.filter(e => e.gender === 'Male').length;
    }

    getTotalFemaleEmployeesCount(): number {
        return this.employees.filter(e => e.gender === 'Female').length;
    }

    onEmployeeCountRadioButtonChange(selectedRadioButtonValue: string): void {
        this.selectedEmployeeCountRadioButton = selectedRadioButtonValue;
    }
}
```

### http service
```js
/* */
import { HttpModule } from '@angular/http';

@NgModule({
	imports: [HttpModule],
	declarations: [],
	bootstrap: [AppComponent]
})

export class AppModule { }

/* EmployeeService */
@Injectable()
export class EmployeeService {

	// Inject Angular http service
	constructor(private _http: Http) { }

	// Notice the method return type is Observable<IEmployee[]>
	getEmployees(): Observable<IEmployee[]> {
		// To convert Observable<Response> to Observable<IEmployee[]>
		// we are using the map operator
		return this._http.get('http://localhost:24535/api/employees')
			.map((response: Response) => <IEmployee[]>response.json());
	}
}

/* employeeList.component.ts */
ngOnInit() {
	this._employeeService.getEmployees()
		.subscribe(employeesData => this.employees = employeesData);
}

```
- What is an Observable 
	- Observable is an asynchronous pattern. In the Observable pattern we have an Observable and an Observer. Observer observes the Observable. In many implementations an Observer is also called as a Subscriber.
	- An Observable can have many Observers (also called Subscribers).
	- Observable emits items or notifications over time to which an Observer (also called Subscriber) can subscribe.
	- When a subscriber subscribes to an Observable, the subscriber also specifies a callback function.
	- This subscriber callback function is notified as and when the Observable emits items or notifications.
	- Within this callback function we write code to handle data itmes or notifications received from the Observable.
	- 3 callback functions 
		- onNext: The Observable calls this method whenever the Observable emits an item. The emitted item is passed as a parameter to this method
		- onError: The Observable calls this method if there is an error
		- onCompleted: The Observable calls this method after it has emitted all items, i.e after it has called onNext for the final time

### http error handling
```js
/* EmployeeService  */
getEmployees(): Observable<IEmployee[]> {
	return this._http.get('http://localhost:24535/api/employees')
		.map((response: Response) => <IEmployee[]>response.json())
		.catch(this.handleError);
}

handleError(error: Response) {
	console.error(error);
	return Observable.throw(error);
}

/* EmployeeListComponent */
ngOnInit() {
	// The second arrow function sets the statusMessage property
	// to a meaningful message that can be displayed to the user
	this._employeeService.getEmployees()
		.subscribe(
			employeesData => this.employees = employeesData,
			error => {this.statusMessage = 'Problem with the service. Please try again after sometime';}
		);
}
```

### routing
```js
/* AppModule */
import { RouterModule, Routes } from '@angular/router';

// Routes is an array of Route objects
// Each route maps a URL path to a component
// The 3rd route specifies the route to redirect to if the path
// is empty. In our case we are redirecting to /home
// The 4th route (**) is the wildcard route. This route is used
// if the requested URL doesn't match any other routes already defined
const appRoutes: Routes = [
	{ path: 'home', component: HomeComponent },
	{ path: 'employees', component: EmployeeListComponent },
	{ path: '', redirectTo: '/home', pathMatch: 'full' },
	{ path: '**', component: PageNotFoundComponent }
];

// To let the router know about the routes defined above,
// pass "appRoutes" constant to forRoot(appRoutes) method
@NgModule({
	imports: [
		BrowserModule, FormsModule, HttpModule,
		RouterModule.forRoot(appRoutes)
	],
	declarations: [AppComponent, HomeComponent],
	bootstrap: [AppComponent]
})
export class AppModule { }

/* html */
//The routerLink directive tells the router where to navigate when the user clicks the link.
//The routerLinkActive directive is used to add the active bootstrap class to the HTML navigation element whose route matches the active route.
//The router-outlet directive is used to specify the location where we want the routed component's view template to be displayed.
<div style="padding:5px">
	<ul class="nav nav-tabs">
		<li routerLinkActive="active">
			<a routerLink="home">Home</a>
		</li>
		<li routerLinkActive="active">
			<a routerLink="employees">Employees</a>
		</li>
	</ul>
	<br/>
	<router-outlet></router-outlet>
</div>
```

### route parameters
```js
/* AppModule */
const appRoutes: Routes = [
	{ path: 'employees/:code', component: EmployeeComponent }
];

/* html */
<a [routerLink]="['/employees',employee.code]">
	{{employee.code | uppercase}}
</a>

/* EmployeeService */
getEmployeeByCode(empCode: string): Observable<IEmployee> {
	return this._http.get("http://localhost:31324/api/employees/" + empCode)
		.map((response: Response) => <IEmployee>response.json())
		.catch(this.handleError);
}
/* EmployeeComponent */
ngOnInit() {
	let empCode: string = this._activatedRoute.snapshot.params['code'];
	this._employeeService.getEmployeeByCode(empCode).subscribe(
		(employeeData) => {
			if (employeeData == null) {
				this.statusMessage = 'Employee with the specified Employee Code does not exist';
			}
			else {
				this.employee = employeeData;
			}
		},
		(error) => {
			this.statusMessage = 'Problem with the service. Please try again after sometime';
			console.error(error);
		});
}

```


### router navigate method
```js
/* html */
<input type="button" class="btn btn-primary" value="Back to Employees List" (click)="onBackButtonClick()"/>

/* component */
// import { Router } from '@angular/router';
constructor(private _router: Router) {}

onBackButtonClick() :void {
	this._router.navigate(['/employees']);
}
```

### Promises
```js
/* service */
@Injectable()
export class EmployeeService {
	constructor(private _http: Http) { }

	getEmployees(): Observable<IEmployee[]> {
		return this._http.get('http://localhost:24535/api/employees')
			.map((response: Response) => <IEmployee[]>response.json())
			.catch(this.handleError);
	}

	// Notice we changed the return type of the method to Promise<IEmployee>
	// from Observable<IEmployee>. We are using toPromise() operator to
	// return a Promise. When an exception is thrown handlePromiseError()
	// logs the error to the console and throws the exception again
	getEmployeeByCode(empCode: string): Promise<IEmployee> {
		return this._http.get("http://localhost:24535/api/employees/" + empCode)
			.map((response: Response) => <IEmployee>response.json())
			.toPromise()
			.catch(this.handlePromiseError);
	}

	// This method is introduced to handle exceptions
	handlePromiseError(error: Response) {
		console.error(error);
		throw (error);
	}

	handleError(error: Response) {
		console.error(error);
		return Observable.throw(error);
	}
}

/* component */
ngOnInit() {
	let empCode: string = this._activatedRoute.snapshot.params['code'];
	// The only change that we need to make here is use
	// then() method instead of subscribe() method
	this._employeeService.getEmployeeByCode(empCode).then(
		(employeeData) => {
			if (employeeData == null) {
				this.statusMessage = 'Employee with the specified Employee Code does not exist';
			} else {
				this.employee = employeeData;
			}
		},
		(error) => {
			this.statusMessage = 'Problem with the service. Please try again after sometime';
			console.error(error);
		});
}
```
- Promises
	- Emits a single value
	- Not Lazy
	- Cannot be cancelled
- Observable
Promise	
	- Emits multiple values over a period of time
	- Lazy. An Observable is not called until we subscribe to the Observable
	- Can be cancelled using the unsubscribe() method
	- Observable provides operators like map, forEach, filter, reduce, retry, retryWhen etc.

- Observable retry on error
```js
/* component */
ngOnInit() {
let empCode: string = this._activatedRoute.snapshot.params['code'];

this._employeeService.getEmployeeByCode(empCode)
	// Chain the retry operator to retry on error.
	//.retry()

	// Retry only 3 times if there is an error
	//.retry(3)

	// Retry with a delay of 1000 milliseconds (i.e 1 second)
	.retryWhen((err) => err.delay(1000))

	// The delay operator will not work with retry
	//.retry().delay(5000)
	.subscribe(
		(employeeData) => {
			if (employeeData == null) {
				this.statusMessage = 'Employee with the specified Employee Code does not exist';
			} else {
				this.employee = employeeData;
			}
		},
		(error) => {
			this.statusMessage ='Problem with the service. Please try again after sometime';
			console.error(error);
	});
}

/* If you want to retry every 1000 milli-seconds only for a miximum of 5 times */
ngOnInit() {
	let empCode: string = this._activatedRoute.snapshot.params['code'];

	this._employeeService.getEmployeeByCode(empCode)
		// Retry 5 times maximum with a delay of 1 second
		// between each retry attempt
		.retryWhen(
			(err) => {
				return err.scan(
					(retryCount, val) => {
						retryCount += 1;
						if (retryCount < 6) {
							this.statusMessage = 'Retrying...Attempt #' + retryCount;
							return retryCount;
						} else {
							throw (err);
						}
					}, 0).delay(1000)
			}
		)
		.subscribe(
			(employeeData) => {
				if (employeeData == null) {
					this.statusMessage = 'Employee with the specified Employee Code does not exist';
				}	else {
					this.employee = employeeData;
				}
			},
			(error) => {
				this.statusMessage = 'Problem with the service. Please try again after sometime';
				console.error(error);
			}
		);
}

```

### observable unsubscribe
```js
/* html */
<div style="margin-top:5px" *ngIf="!subscription.closed">
	<input type="button" class="btn btn-primary" value="Cancel Request" (click)="onCancelButtonClick()" />
</div>

/* component */
// Create a class property of type ISubscription
// The ISubscription interface has closed property
// The ngIf directive in the HTML binds to this property
// Go to the difinition of ISubscription interface to
// see the closed property
subscription: ISubscription;

ngOnInit() {
	let empCode: string = this._activatedRoute.snapshot.params['code'];

	// Use the subscription property created above to hold on to the
	// subscription.We use this object in the onCancelButtonClick()
	// method to unsubscribe and cancel the request
	this.subscription = this._employeeService.getEmployeeByCode(empCode)
			.retryWhen((err) => {
					return err.scan((retryCount, val) => {
							retryCount += 1;
							if (retryCount < 4) {
									this.statusMessage = 'Retrying...Attempt #' + retryCount;
									return retryCount;
							}
							else {
									throw (err);
							}
					}, 0).delay(1000)
			})
			.subscribe((employeeData) => {
					if (employeeData == null) {
							this.statusMessage =
									'Employee with the specified Employee Code does not exist';
					}
					else {
							this.employee = employeeData;
					}
			},
			(error) => {
					this.statusMessage =
							'Problem with the service. Please try again after sometime';
					console.error(error);
		});
}

// This method is bound to the click event of the "Cancel Request" button
// Notice we are using the unsubscribe() method of the subscription object
// to unsubscribe from the observable to cancel the request. We are also
// setting the status message property of the class to "Request Cancelled"
// This message is displayed to the user to indicate that the request is cancelled
onCancelButtonClick(): void {
		this.statusMessage = 'Request cancelled';
		this.subscription.unsubscribe();
}
```

### Modules
- In simple terms an Angular Module is a class decorated with @NgModule decorator.
- An Angular Module is a mechanism to group components, directives, pipes and services that are related to a feature area of an angular application.
- Module Type
	- Root Module
	- Feature Module
	- Core Module
	- Shared Module
	- Routing Module

### Root Module
- Root Module	Every Angular application has at least one module, the root module. By default, this root application module is called AppModule. We bootstrap this root module to launch the application. If the application that you are building is a simple application with a few components, then all you need is the root module. As the application starts to grow and become complex, in addition to the root module, we may add several feature modules. We then import these feature modules into the root module.

### Feature Module
- To group the components, directives, pipes and services related to a specific feature area, we create a module for each feature area. These modules are called feature modules.

### Core Module
- The most important use of this module is to include the providers of http services. Services in Angular are usually singletons. So to ensure that, only one instance of a given service is created across the entire application, we include all our singleton service providers in the core module. In most cases, a CoreModule is a pure services module with no declarations. The core module is then imported into the root module (AppModule) only. CoreModule should never be imported in any other module.

### Shared Module
- This module contains reusable components, directives, and pipes that we want to use across our application. The Shared module is then imported into specific Feature Modules as needed. The Shared module might also export the commonly used Angular modules like CommonModule, FormsModule etc. so they can be easily used across your application, without importing them in every Feature Module.

### Routing Module
- An angular application may also have one or more routing modules for application level routes and feature module routes

### Creating feature module
```js
/* employee.module.ts */
// employee.module.ts
import { NgModule } from '@angular/core';
// Exports all the basic Angular directives and pipes
// such as NgIf, NgFor, DecimalPipe etc.
import { CommonModule } from '@angular/common';
// CreateEmployeeComponent uses ReactiveFormsModule directives such as
// formGroup so ReactiveFormsModule needs to be imported into this Module
// An alternative approach would be to create a Shared module and export
// the ReactiveFormsModule from it, so any other module that needs
// ReactiveFormsModule can import it from the SharedModule.
import { ReactiveFormsModule } from '@angular/forms';

// Import and declare the components that belong to this Employee Module
import { CreateEmployeeComponent } from './create-employee.component';
import { ListEmployeesComponent } from './list-employees.component';

@NgModule({
  imports: [
    CommonModule,
    ReactiveFormsModule
  ],
  declarations: [
    CreateEmployeeComponent,
    ListEmployeesComponent
  ],
  // If you want the components that belong to this module, available to
  // other modules, that import this module, then include all those
  // components in the exports array. Similarly you can also export the
  // imported Angular Modules
  // exports: [
  //   CreateEmployeeComponent,
  //   ReactiveFormsModule
  // ]
})
export class EmployeeModule { }

/* AppModule  */
// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';

import { AppRoutingModule } from './app-routing.module';
import { EmployeeModule } from './employee/employee.module';

import { EmployeeService } from './employee/employee.service';

import { AppComponent } from './app.component';
import { HomeComponent } from './home.component';
import { PageNotFoundComponent } from './page-not-found.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    PageNotFoundComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    EmployeeModule
  ],
  providers: [EmployeeService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Creating a separate routing module for a feature module
- forRoot() method registers the specified routes. It also creates an instance of the Router service and registers it with the angular's dependency injector.
- forChild() method on the other hand only registers the additional specified routes and tells angular to reuse the Router service instance that forRoot has created.
- To ensure that, there is only one instance of Router service, forRoot() method should be called only once in the main application routing module.
```js
/* EmployeeRoutingModule */
import { NgModule } from '@angular/core';
// Import RouterModule & Routes type
import { RouterModule, Routes } from '@angular/router';

// Import all the components that we will be referencing in the route definitions
import { CreateEmployeeComponent } from './create-employee.component';
import { ListEmployeesComponent } from './list-employees.component';

// Define the routes
const appRoutes: Routes = [
  { path: 'list', component: ListEmployeesComponent },
  { path: 'create', component: CreateEmployeeComponent },
  { path: 'edit/:id', component: CreateEmployeeComponent },
];

// In a feature module forChild() method must be used to register routes
// Export RouterModule, so the it's directives like RouterLink, RouterOutlet
// are available to the EmployeeModule that imports this module
@NgModule({
  imports: [ RouterModule.forChild(appRoutes) ],
  exports: [ RouterModule ]
})
export class EmployeeRoutingModule { }

/* EmployeeModule */
// Import EmployeeRoutingModule into EmployeeModule
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ReactiveFormsModule } from '@angular/forms';

// Import the EmployeeRoutingModule
import { EmployeeRoutingModule } from './employee-routing.module';

import { CreateEmployeeComponent } from './create-employee.component';

 
import { ListEmployeesComponent } from './list-employees.component';

@NgModule({
  imports: [
    CommonModule,
    ReactiveFormsModule,
    // Add EmployeeRoutingModule to the imports array
    EmployeeRoutingModule
  ],
  declarations: [
    CreateEmployeeComponent,
    ListEmployeesComponent
  ]
})
export class EmployeeModule { }

/* AppRoutingModule */
// Remove the EMPLOYEE feature module routes from AppRoutingModule
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { HomeComponent } from './home.component';
import { PageNotFoundComponent } from './page-not-found.component';

const appRoutes: Routes = [
  { path: 'home', component: HomeComponent },
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: '**', component: PageNotFoundComponent }
];

@NgModule({
  imports: [ RouterModule.forRoot(appRoutes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }

/* AppModule */
// all feature modules should be imported before AppRoutingModule
@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    PageNotFoundComponent
  ],
  imports: [
    BrowserModule,
    EmployeeModule,
    AppRoutingModule,
    HttpClientModule,
  ],
  providers: [EmployeeService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Creating shared module
- The SharedModule may re-export other common angular modules, such as CommonModule, FormsModule, ReactiveFormsModule etc. Instead of writing the same code in every feature module to import these commonly used Angular modules we can re-export them from a SharedModule, so these commonly used Angular Modules are available to all the feature modules that import this SharedModule.
- The SharedModule should not have providers. This is because, lazy loaded modules create their own branch on the Dependency Injection tree. As a result of this, if a lazy loaded module imports the shared module, we end up with more than one instance of the service provided by the shared module. For this same reason, the SharedModule should not import or re-export modules that have providers.
- The SharedModule is then imported by all the FeatureModules where we need the shared functionality. The SharedModule can be imported by both - eager loaded FeatureModules as well as lazy loaded FeatureModules.

```js
/* SharedModule */
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    // At the moment we do not have any components in this SharedModule
    // that require directives of CommonModule or ReactiveFormsModule
    // So we did not include them here in the imports array. We can
    // export a module without importing it first
  ],
  declarations: [],
  // Our Employee FeatureModule requires the CommonModule directives
  // such as ngIf, ngFor etc. Similarly, Employee FeatureModule also
  // requires ReactiveFormsModule directives. So export CommonModule
  // and ReactiveFormsModule.
  exports: [
    CommonModule,
    ReactiveFormsModule
  ]
})
export class SharedModule { }

/* EmployeeModule */
import { NgModule } from '@angular/core';

import { EmployeeRoutingModule } from './employee-routing.module';

import { CreateEmployeeComponent } from './create-employee.component';
import { ListEmployeesComponent } from './list-employees.component';
import { SharedModule } from '../shared/shared.module';

@NgModule({
  imports: [
    EmployeeRoutingModule,
    SharedModule,
  ],
  declarations: [
    CreateEmployeeComponent,
    ListEmployeesComponent
  ]
})

export class EmployeeModule { }
```

### Grouping routes and creating component less route 
- All the routes in an angular module that you want to lazy load should have the same route prefix.
```js
/* employee-routing.module.ts */
// Notice the parent route(employees) does not have a component associated with it. That is why this route is called a component less route.
const appRoutes: Routes = [
  {
    path: 'employees',
    children: [
      { path: '', component: ListEmployeesComponent },	//-- /employees
      { path: 'create', component: CreateEmployeeComponent },	//-- /employees/create
      { path: 'edit/:id', component: CreateEmployeeComponent },	//-- /employees/edit/1
    ]
  }
];

/* app.component.html */
<li>
    <a routerLinkActive="active" routerLink="employees">List</a>
</li>
<li>
    <a routerLinkActive="active" routerLink="employees/create">Create</a>
</li>

/*  list-employees.component.ts */
editButtonClick(employeeId: number) {
  this._router.navigate(['/employees/edit', employeeId]);
}

/* create-employee.component.ts */
onSubmit(): void {
  this.mapFormValuesToEmployeeModel();
  if (this.employee.id) {
    this.employeeService.updateEmployee(this.employee).subscribe(
      () => this.router.navigate(['employees']),
      (err: any) => console.log(err)
    );
  } else {
    this.employeeService.addEmployee(this.employee).subscribe(
      () => this.router.navigate(['employees']),
      (err: any) => console.log(err)
    );
  }
}
```

### Lazy loading angular modules
- As you add more features to your application you will have more feature modules like ReportsModule, AdminModule etc. As you add more feature modules, the overall application size will continue to grow. At some point you'll reach a tipping point where the application takes a very long time to load. 
- To lazy load a module, it has to meet 2 requirements.
	- All the routes in the angular module that you want to lazy load should have the same route prefix
	- The module should not be referenced in any other module. If it is referenced, the module loader will eagerly load it instead of lazily loading it.

```js
/* app-routing.module.ts */
{ path: 'employees', loadChildren: './employee/employee.module#EmployeeModule' }

/* employee-routing.module.ts */
const appRoutes: Routes = [
  { path: '', component: ListEmployeesComponent },
  { path: 'create', component: CreateEmployeeComponent },
  { path: 'edit/:id', component: CreateEmployeeComponent },
];
```
### Preloading angular modules
- Module loading strategies
	- Eager Loading
		- With Eager Loading all the modules must be downloaded onto the client machine before the application starts.
		- There is nothing special that we have to do, for an Angular module to be eager loaded. It just needs to be referenced in the application using imports metadata of @NgModule decorator.
	- Lazy Loading
		- Lazy loaded modules are loaded on demand when the user navigates to the routes in those respective modules.
	- Preloading
		- With Preloading, lazy loaded modules are downloaded in the background after the initial start up bundle is downloaded. 
		- Preloading is also often called Eager Lazy Loading

```js
/* AppRoutingModule */
import { PreloadAllModules } from '@angular/router';

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes, { preloadingStrategy: PreloadAllModules })
  ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

- The value for preloadingStrategy property
	- NoPreloading:	This is the default and does not preload any modules
	- PreloadAllModules:	Preloads all modules as quickly as possible in the background
	- Custom Preload Strategy:	We can also specify our own custom preloading strategy. 

### Custom preloading strategy
```js
/* CustomPreloadingService  */
// To create a Custom Preloading Strategy, create a service that implements Angular's built-in PreloadingStrategy abstract class
import { Injectable } from '@angular/core';
import { PreloadingStrategy, Route } from '@angular/router';
import { Observable, of } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
// Since we are creating a Custom Preloading Strategy, this service
// class must implement PreloadingStrategy abstract class
export class CustomPreloadingService implements PreloadingStrategy {

  constructor() { }

  // PreloadingStrategy abstract class has the following preload()
  // abstract method for which we need to provide implementation
  preload(route: Route, fn: () => Observable<any>): Observable<any> {
    // If data property exists on the route of the lazy loaded module
    // and if that data property also has preload property set to
    // true, then return the fn() which preloads the module
    if (route.data && route.data['preload']) {
      return fn();
    // If data property does not exist or preload property is set to
    // false, then return Observable of null, so the module is not
    // preloaded in the background
    } else {
      return of(null);
    }
  }
}

/* app-routing.module.ts */
// Import CustomPreloadingService and set it as the Preloading Strategy
import { CustomPreloadingService } from './custom-preloading.service';

RouterModule.forRoot(appRoutes, {
  preloadingStrategy: CustomPreloadingService
})

// Modify the 'employees' route
const appRoutes: Routes = [
  { path: 'home', component: HomeComponent },
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  {
    path: 'employees',
    // set the preload property to true, using the route data property
    // If you do not want the module to be preloaded set it to false
    data: { preload: true },
    loadChildren: './employee/employee.module#EmployeeModule'
  },
  { path: '**', component: PageNotFoundComponent }
];

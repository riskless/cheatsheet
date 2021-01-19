### NodeJS
- Server side environment to create and run server side programs.
- NodeJS is built based on JavaScript language.
- NodeJS supports asynchronous I/O operations. (database/file operations)
- NodeJS internally uses a JavaScript Engine (JavaScript Interpreter) called V8 Engine, which is developed by Google, and which is already being used in Google Chorme.
- NodeJS was developed by Ryan Dahl, under the administration of Linux foundation.
- NodeJS is open source.
- NodeJS is free to use.
- NodeJS is cross-platform.
- Not a programming language. It is an infrastructure to build and run software apps in JS.
- Used for Full Stack JS.
- To show current ES6 support in Node.js (http://node.green/)
- Actions NOT supported in JS but supported in NODE.JS
	- File Actions (Create, Read, Update, Delete)
	- OS Actions
	- These actions are supported through additional modules.
- practical uses
	- Web Applications
	- Backend API
	- Chat / Messaging (Real time apps)
	- Command Line Programs
	- Scheduled Jobs


### Execution Flow of PHP/Java/.NET
- Broswer sends request to server.
- Server send request to database or file.
- While the database or file is being read/written, the server simply waits until the operation is completed.
- After completion of database/file operation, the content is sent to broswer.
- The server is ready to receive next request.

### Execution Flow of NodeJS
- Broswer sends request to server.
- Server send request to database or file.
- Server will not wait until the completion of database/file operation, simply it receives another request starts its processing.
- After completion of database/file operation, the content is sent to broswer.

### Non-blocking
- Node.js operates on a single thread, using non-blocking I/O calls.
- Non-blocking commands execute in parallel, and use callbacks to signal completion or failure. In other words, Node.js uses asynchronous programming techniques.
- This is the biggest difference between Node.js and other server side programming languages like PHP, JSP, & ASP.net.

### REPL
- Node.js ships with a REPL.
- REPL stands for Read-Eval-Print-Loop.
- It is a language shell or interactive command line environment.
- open REPL
	- node

### Modules
- Modules are JS libraries.
- It is a file in NodeJS.
- A module can share its variables/objects/functions to other modules.
- They are used for:
	- Code reuse
	- Abstraction (Data Privacy)
	- Modularity

- There are 3 kinds of modules in Node.js:
1. In-Built or Core Modules
2. NPM or Installed Modules
3. Custom Modules
	- Node.js does not support ES6 import statement. Modules follow commonJS specification.

### An example of custom modules
- app.js
```js
console.log(module);

const greeting = require('./lib/greeting.js');
greeting();

const average = require('./lib/school.js').average;
console.log(average([70, 55, 90, 89.144]));

console.log(require('./lib/school.js').grades);
```
- lib/greeting.js
```js
//exporting one function
const greet = () => {console.log('hello');};
module.exports = greet;
console.log(module);
```

- lib/math.js
```js
module.exports.average = array => {
    let sum = 0;
    array.forEach(element => {sum += element;});
    return (sum/array.length).toFixed(2);
};

module.exports.grades = {
    best: 97,
    average: 56
}

console.log(module);
```

### Built-in modules
- Node.js has a set of core modules which can be used without any further installation.
	- events
	- fs
	- path
	- http
	- util
	- os

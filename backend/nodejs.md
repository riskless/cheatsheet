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
### FS Module
- The fs module provides access to the file system.
- The fs module has functions for creating, reading, updating and deleting files and directories.
- It has asynchronous as well as synchronous functions.
- It has streaming as well as non-streaming functions.
- Always prefer asynchronous functions.
- Always prefer streaming functions for large files.
- FS modules methods
	- readFile() 
		- Reads the content of a file asynchronously.
	- readFileSync() 
		- Same as readFile(), but synchronous.
	- writeFile() 
		- Writes data to a file asynchronously.
	- writeFileSync() 
		- Same as writeFile(), but synchronous.
	- unlink() 
		- Deletes a file asynchronously.
	- unlinkSync() 
		- Deletes a file, but synchronous.
	- mkdir()
		- Makes a new directory asynchronously.
	- mkdirSync()
		- Same as mkdir(), but synchronous.
	- rmdir()
		- Removes a directory asynchronously.
	- rmdirSync()
		- Same as rmdir(), but synchronous.
	- createReadStream()
		- Returns a new readable stream object asynchronously.
	- createWriteStream()
		- Returns a new writeable stream object asynchronously.
	- rename()
		- Renames a file asynchronously.
	- renameSync()
		- Same as rename(), but synchronous.

- Test code
```js
const fs = require('fs');

// Synchronous
const filecontent = fs.readFileSync('./readme.txt', 'utf-8');
fs.writeFileSync('./writeme.txt', filecontent, 'utf-8');
console.log('Sync file read/write');

// Asynchronous file read/write
fs.readFile('./readme.txt', 'utf-8', (err, data) => {
  fs.writeFile('./writemeasync.txt', data, 'utf-8', () => {
    console.log('Async file read/write');
  });
});

```

### ExpressJS
- It is possible to create a simple web server using the http module. But other common web development tasks need a lot of custom code to be written.
- Web Frameworks provide a easy way of implementing common web development requirements.
- Express is the most popular Node.js web framework, and is the underlying library for a number of other popular Node web frameworks like MEAN/MERN.
- Express is minimalist & unopinionated. But, Express JS community has created packages to address almost any web development problem. There are libraries to work with cookies, sessions, user logins, URL parameters, POST data, security headers, and many more.
- ExpressJS Features
	- Write handlers for different HTTP verbs at different URL paths (routes).
	- Integrate with "view" rendering templates. Express has support for a number of template engines. Some popular template engines that work with Express are Pug (Jade), Mustache, Handlebars, and EJS.
	- Set common web application settings (like location of static content).
	- Getting query, route and post parameters.
	- Allow usage of additional request processing "middleware" for requirements like cookies, sessions, emails, file upload etc. 

### An example (express)
- app.js
```js
const express = require('express');
const bp = require('body-parser');
const app = express();
app.use(bp.json());

const skillsData = {
  skills: ['Node.js'],
};

app.get('/api/skills', (req, res) => {
  res.json(skillsData);
});

app.post('/api/skills', (req, res) => {
  skillsData.skills.push(req.body.skill);
  res.json(skillsData);
});

app.delete('/api/skills', (req, res) => {
  skillsData.skills.pop(req.body.skill);
  res.json(skillsData);
});

app.listen(8080, () => {
  console.log('App started and listening at port 8080');
});
```

### An example (express + ejs)
- ejs install
	- npm install ejs --save

- app.js
```js
const express = require('express');
const ejs = require('ejs');

const bp = require('body-parser');
const urlencoded = bp.urlencoded({ extended: false });

const app = express();
app.set('view engine', 'ejs');

app.get('/', (req, res) => {
  res.send('Home | <a href="aboutus">About Us</a> | <a href="contactus">Contact Us</a><h1>Hello World!</h1>');
});

app.get('/aboutus', (req, res) => {
  res.sendFile(`${__dirname}/aboutus.html`);
});

app.get('/input/:name/:location', (req, res) => {
  inputData = {
    rp: req.params,
    qs: req.query,
    skills: [
      'Node.js', 'React.js', 'JavaScript',
    ],
    nav: ['Home', 'About Us', 'Contact Us'],
  };

  res.render('details', inputData);
});

app.get('/contactus', (req, res) => {
  res.sendFile(`${__dirname}/contactus.html`);
});

app.post('/postdata', urlencoded, (req, res) => {
  res.json(req.body);
});

app.get('*', (req, res) => {
  res.send('<h2>404 - Page Not Found</h2>');
});

app.listen(8080, () => {
  console.log('App started and listening at port 8080');
});
```

- aboutus.html
```html
<body>
  <a href="/">Home</a> | About Us | <a href="/contactus">Contact Us</a>
  <h1>About Us</h1>
  <a href="/input/harold/seoul?web=howtostudyit.com&profession=Engineer">Test Query & Route Parameters</a>
</body>
```

- conatact.html
```html
<body>
  <a href="/">Home</a> | <a href="/aboutus">About Us</a> | Contact Us
  <h1>Contact Us</h1>
  <form action="/postdata" method="post">
    Name:
    <input name="name" type="text"><br/><br/> Email:
    <input name="email" type="email"><br/><br/> Comments:<br/>
    <textarea name="comments" id="" cols="30" rows="10"></textarea><br/><br/>
    <input type="submit" value="Submit">
  </form>
</body>
```

- views/details.ejs
```html
<body>
  <!--Include Partial-->
  <%-include menu.ejs%>
    <p>
      Name: <%=rp.name%><br>
      Location: <%=rp.location%><br> 
      Website: <%=qs.web%><br> 
      Profession: <%=qs.profession%><br>
      <br>
    </p>

    <span>Skills:</span>
    <ul>
      <% skills.forEach(function(data){%>
        <li>
          <%=data%>
        </li>
        <%});%>
    </ul>
</body>
```
- views/menu.ejs
```html
<ul>
  <% nav.forEach(function(data){%>
    <li>
      <a href="#">
        <%=data%>
      </a>
    </li>
    <%});%>
</ul>
```

### References
- [Node.js Tutorial](https://www.w3schools.com/nodejs/default.asp)
- [Node.JS Tutorial](https://www.youtube.com/watch?v=_9tcueGoDW8&list=PLVz1UWMtyw51n7FLxcSbh9S23oDUiisc0&index=17)
- [NODE JS Online Training](https://www.youtube.com/watch?v=YcLFmi7nYog)

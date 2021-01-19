### NPM (Node Package Manager)
- NPM is a package manager for JS that ships with Node.js. 
- It helps to discover packages of reusable code - assemble them in powerful new ways.
- Use NPM to install, share, and distribute code; manage dependencies in your projects; and share & receive feedback with others.
- The NPM registry hosts almost half a million packages of free, reusable code - the largest software registry in the world.
- NPM is automatically installed when Node.js is installed.
- NPM takes care of dependency management through a file named package.json.
- In the package.json file, each dependent module along with its version number is listed.
- NPM creates a folder named node_modules, where all the installed packages will be placed. 

### Commands
- search for NPM packages
	- npm search jquery
	- official site: https://www.npmjs.com/

- view more information about a particular library
	- npm view jquery
	- npm view jquery version
	- npm view jquery versions

- install npm packages only for a particular applicaton
	- npm install jquery

- intall a particular version
	- install npm underscore@1.5.3
	- npm list

- view pakages installed
	- npm list
	- npm ls

- uninstall packages
	- npm uninstall express

- create package.json for a particular application
	- npm init
	- npm init --force
	- npm init -f

- include dependencies in the package.json
	- npm install express --save

- remove packages and exclude dependencies in the package.json
	- npm uninstall express --save

- show you all the list of libraries which are currently outdated
	- npm outdated

- update all the packages to the latest version for your entire application
	- npm update

- update a specific pacage
	- npm update express --save

- install packages globally
	- npm install express --global
	- npm install express -g
	- npm list -g
	- npm uninstall express --global

- use packages
```js
var o = require("undserscore");
o.each([1,2,3], function(m){
	console.log(m);
});
```

### References
- [Quick introduction to NPM (Node Package Manager)](https://www.youtube.com/watch?v=uwivwx7YC-0&list=PLvZkOAgBYrsQVc9PFn8mQ-xXef9zmy3kC&index=3)

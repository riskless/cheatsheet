### Task Runners
- JS tasks (regular development tasks at client-side)
	- Minify JS
	- Concat Files into one
	- Transpile (compile/build)
	- etc.
- Tasks performed usually through plug-ins
	- Plenty of plug-ins
- Popular Task Runners:
	- Grunt
	- Gulp

### Bundlers
- Compiles JS written using module system
	- AMD, CommonJS, ES6
- Bundle = Compile, Concat (files into one) and/or minify
- Not designed to be a task runner
- Module bundler at its core
- Can be used as a replacement of Task Runner
	- Only in some scenarios
		- Few tasks of Task Runner can be achieved using Bundler
	- Not a complete replacement though
- Can still integrate with a task runner
- Popular Bundlers:
	- Browserify
	- Webpack

### Webpack
- Webpack is module bundling system (bundler)
- Open Source & Platform independent
- Works with Node/NPM
- Mainly used to transpile (compile) JS modules
	- so that every brower can understand JS, without worrying about JS versions/features supported
	- Various "loaders" need to be involved for specific transilations
		- babel: ReactJS -> Plain JS
- Can be configured through "webpack.config.js"
- Highly Customizable and Extensible with loaders and plug-ins
- Supports CLI
- Can integrate with Grunt/Gulp
	- But not needed, in some scenarios
- Has a built-in watcher
	- watcher: a file monitoring tool

### Simple Demo
- Settings up basic ES6 (or ES2015) development environment
- Uses
	- Webpack
	- Babel (for ES6/ES2015 module tranpilations)
	- jQuery (not necessary, just an example)

- installation
	- npm install --save-dev webpack
	- npm install --save-dev babel-core babel-loader babel-preset-es2015
	- npm install --save jquery

- index.html
```html
<script src="build/app.all.js"></script>
<button id="showBtn">Show</button>
```

- src/Message.js
```js
export default class Message {
	show() {
		alert("Hello world!");
	}
}
```

- src/app.js
```js
import msg from './Message.js'
import $ from 'jquery'

$(function(){
	$(#showBtn").on("click", function(){
		var o = new msg();
		o.show();
	}
});
```

- webpack configuration (webpack.config.js)
```js
var path = require('path');
var webpack = require("webpack');

module.exports = {
	entry: ['./src/app.js'],
	output: {
		path: './build',
		filename: 'app.all.js'
	},
	module: {
		loaders: [{
			test:/\.js$/, 
			include:path.resolve(__dirname, "src"),
			loader: 'babel-loader',
			query: {
				presets:['es2015']
		}]
	},
	watch:true
};
```

- tranpile all the js files within the src foler to es2015 using babel-loader
- execute webpack (using webpack.cmd)
	- webpack // creating app.all.js
- execute webpack with watcher
	- webpack --watch // watch: true

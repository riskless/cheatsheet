### ReactJS
- ReactJS (aka React.js or React) is an User Interface library for writing SPA.
- It provides just the V of MVC.
- React with Reduc & React-router together behave like a framework.
- A React SPA is built out of building blocks called Components.
- State and Props are the core concepts of ReactJS.
- In React, all visual elements are defined only in JavaScript (JSX).
- No need for HTML.
- Even CSS can be written as JavaScript.
- JSX stands for JavaScript XML.
- JSX is essentially JS but with a HTML-ish (XML-ish) syntax.
- Browsers cannot understand JSX or ES6/7.
- So React takes the help of Babel to tranpile JSX & ES6/7 to plain JavaScript.
- Babel convers ES6/ES7 and JSX to ES5.
- Uses Webpack & Babel internally.
- Create React App is the best way to start building a new React SPA.

### Install and Run
- npm install -g create-react-app
- create-react-app myapp
- cd my app
- npm start

```js
/* package.json */
{
  "name": "myapp",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^16.2.0",
    "react-dom": "^16.2.0",
    "react-scripts": "1.1.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

### example (React Directly in HTML)
```html
<!DOCTYPE html>
<html>
  <script src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
  <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
  <body>
    <div id="mydiv"></div>

    <script type="text/babel">
      class Hello extends React.Component {
        render() {
          return <h1>Hello World!</h1>
        }
      }

      ReactDOM.render(<Hello />, document.getElementById('mydiv'))
    </script>
  </body>
</html>
```

### example (React environment)
```js
/* index.html */
<div id="root"></div>

/* index.js */
import React from "react";
import ReactDOM from "react-dom";

ReactDOM.render(
	<h1>Hello World</h1>, document.getElementById("root"),
);
```

### example
```js
/* index.html */
<div id="root"></div>

/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App/>
		<App/>
	</div>,
	document.getElementById("root")
);

/* App.js */
import React from "react";

class App extends React.Component {
  render() {
    return (
      <h1>Hello World</h1>;
    )
  }
}
export default App;
```

### example (nested component)
```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App/>
		<App/>
	</div>,
	document.getElementById("root"),
);

/* App.js */
import React from "react";
import Para from "./Para";


class App extends React.Component {
  render() {
    return (
			<div>
				<h1>Hello World</h1>
				<hr/>
				<Para/>
			</div>
    );
  }
}
export default App;

/* Para.js */
import React from "react";

class Para extends React.Component {
  render() {
    return (
			<div>
				<p>test para</p>
			</div>
    )
  }
}
export default Para;
```

### example (property)
- Props are arguments passed into React components.
- Props are passed to components via HTML attributes.
- If you have a variable to send, and not a string as in the example above, you just put the variable name inside curly brackets
- If your component has a constructor function, the props should always be passed to the constructor and also to the React.Component via the super() method.
- React Props are read-only! You will get an error if you try to change their value.
```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App greet="Hello" who="Harold"/>
		<App greet="Hi" who="Crystal"/>
	</div>,
	document.getElementById("root"),
);

/* App.js */
import React from "react";
import Para from "./Para";

class App extends React.Component {
  render() {
    return (
			<div>
				<h1>{this.props.greet} {this.props.who}</h1>
				<hr/>
				<Para/>
			</div>
    );
  }
}
export default App;

/* Para.js */
import React from "react";

class Para extends React.Component {
  render() {
    return (
			<div>
				<p>test para</p>
			</div>
    )
  }
}
export default Para;
```

### example (pass a property to the child component)
```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App greet="Hello" who="Harold"/>
		<App greet="Hi" who="Crystal"/>
	</div>,
	document.getElementById("root")
);

/* App.js */
import React from "react";
import Para from "./Para";

class App extends React.Component {
  render() {
    return (
			<div>
				<h1>{this.props.greet} {this.props.who}</h1>
				<hr/>
				<Para name={this.props.who}/>
			</div>
    );
  }
}
export default App;

/* Para.js */
import React from "react";

class Para extends React.Component {
  render() {
    return (
			<div>
				<p>{this.props.who}</p>
			</div>
    )
  }
}
export default Para;


### React State
- React components has a built-in state object.
- The state object is where you store property values that belongs to the component.
- When the state object changes, the component re-renders.
- To change a value in the state object, use the this.setState() method. Always use the setState() method to change the state object, it will ensure that the component knows its been updated and calls the render() method (and all the other lifecycle methods).
```js
// Refer to the state object in the render() method
class Car extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      brand: "Ford",
      model: "Mustang",
      color: "red",
      year: 1964
    };
  }
  render() {
    return (
      <div>
        <h1>My {this.state.brand}</h1>
        <p>
          It is a {this.state.color}
          {this.state.model}
          from {this.state.year}.
        </p>
      </div>
    );
  }
}

// Add a button with an onClick event that will change the color property
class Car extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      brand: "Ford",
      model: "Mustang",
      color: "red",
      year: 1964
    };
  }
  changeColor = () => {
    this.setState({color: "blue"});
  }
  render() {
    return (
      <div>
        <h1>My {this.state.brand}</h1>
        <p>
          It is a {this.state.color}
          {this.state.model}
          from {this.state.year}.
        </p>
        <button
          type="button"
          onClick={this.changeColor}
        >Change color</button>
      </div>
    );
  }
}
```

### example (static state)
```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App />
	</div>,
	document.getElementById("root"),
);

/* App.js */
import React from "react";

class App extends React.Component {
  
	render() {
		const ts = new Date().toLocalTimeString();
		// static
    return <h1>{ts}{</h1>;
  }
}
export default App;
```

### example (dynamic state)
```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App />
	</div>,
	document.getElementById("root")
);

/* App.js */
import React from "react";

class App extends React.Component {
	constructor() {
		super();
		this.state = {
			ts: new Date().toLocalTimeString()
		};
	}
  
	render() {
    return <h1>{this.state.ts}{</h1>;
  }
}
export default App;
```

### example (dynamic state)
```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App />
	</div>,
	document.getElementById("root")
);

/* App.js */
import React from "react";

class App extends React.Component {
	constructor() {
		super();
		this.state = {
			ts: new Date().toLocalTimeString()
		};
	}

	componentDidMount() {
		setInterval(()=>{
			this.setState({ts: new Date().toLocalTimeString()});
		}, 1000);
	}
  
	render() {
		// automatically called whenever state value is changed.
    return <h1>{this.state.ts}{</h1>;
  }
}
export default App;
```

### Lifecycle of Components
- Each component in React has a lifecycle which you can monitor and manipulate during its three main phases. 
- The three phases are: Mounting, Updating, and Unmounting.
- Mounting
	- constructor()
		- The constructor() method is called before anything else, when the component is initiated, and it is the natural place to set up the initial state and other initial values.
	- getDerivedStateFromProps()
		- The getDerivedStateFromProps() method is called right before rendering the element(s) in the DOM.
	- render()
		- The render() method is required, and is the method that actually outputs the HTML to the DOM.
	- componentDidMount()
		- The componentDidMount() method is called after the component is rendered.
	- The render() method is required and will always be called, the others are optional and will be called if you define them.
- Updating
	- getDerivedStateFromProps()
	- shouldComponentUpdate()
	- render()
	- getSnapshotBeforeUpdate()
	- componentDidUpdate()
	- The render() method is required and will always be called, the others are optional and will be called if you define them.
- Unmounting
	- componentWillUnmount()

### Events
- Just like HTML, React can perform actions based on user events.
- React events are written in camelCase syntax: onClick instead of onclick.
- React event handlers are written inside curly braces: onClick={shoot}  instead of onClick="shoot()"
```js
/* Bind this */
class Football extends React.Component {
  shoot = () => {
    alert(this);
    // The 'this' keyword refers to the component object
		// With arrow functions, this will always represent the object that defined the arrow function.
  }
  render() {
    return (
      <button onClick={this.shoot}>Take the shot!</button>
    );
  }
}

ReactDOM.render(<Football />, document.getElementById('root'));


/* Passing Arguments: Make an anonymous arrow function */
class Football extends React.Component {
  shoot = (a) => {
    alert(a);
  }
  render() {
    return (
      <button onClick={() => this.shoot("Goal")}>Take the shot!</button>
    );
  }
}

ReactDOM.render(<Football />, document.getElementById('root'));

/* Passing Arguments: Bind the event handler to this */
// If you must use regular functions instead of arrow functions you have to bind this to the component instance using the bind() method
class Football extends React.Component {
  shoot(a) {
    alert(a);
  }
  render() {
    return (
      <button onClick={this.shoot.bind(this, "Goal")}>Take the shot!</button>
    );
  }
}

ReactDOM.render(<Football />, document.getElementById('root'));

/* React Event Object */
class Football extends React.Component {
  shoot = (a, b) => {
    alert(b.type);
    // 'b' represents the React event that triggered the function, in this case the 'click' event
  }
  render() {
    return (
      <button onClick={(ev) => this.shoot("Goal", ev)}>Take the shot!</button>
    );
  }
}
ReactDOM.render(<Football />, document.getElementById('root'));

/* Without arrow function */
// Without arrow function, the React event object is sent automatically as the last argument when using the bind() method
class Football extends React.Component {
  shoot = (a, b) => {
    alert(b.type);
    // 'b' represents the React event that triggered the function, in this case the 'click' event
  }
  render() {
    return (
      <button onClick={this.shoot.bind(this, "Goal")}>Take the shot!</button>
    );
  }
}

ReactDOM.render(<Football />, document.getElementById('root'));
```
### example (event)
```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App />
	</div>,
	document.getElementById("root"),
);

/* App.js */
import React from "react";
import Form from "./Form";

class App extends React.Component {
	constructor() {
		super();
	}
  
	render() {
    return (
			<div>
				<Form />
			</div>
		);
  }
}
export default App;

/* Form.js */
import React from 'react';
import Display from "./Display";

class Form extends React.Component {
  constructor() {
    super();

    this.state = {
      count: 0,
    };

		this.increment = this.increment.bind(this);
    this.decrement = this.decrement.bind(this);
	}

  increment() {
    console.log("increment");
		this.setState(prevState => ({ count: prevState.count + 1 }));
  }

  decrement() {
    console.log("decrement");
		this.setState(prevState => ({ count: prevState.count - 1 }));
  }

	render() {
		return 
		<div>
			<input type="button" value="Increment" onClick={this.increment} />
			<input type="button" value="Decrement" onClick={this.decrement} />
			<br/></br/>
			<Display value={this.state.count}/>
		</div>;
	}
}
export default Form;


/* Display.js */
import React from 'react';

class Display extends React.Component {
	render() {
		return <div>Count: {this.props.value}</div>;
	}
}
export default Display;
```

### React Sass
- Sass is a CSS pre-processor.
- Sass files are executed on the server and sends CSS to the browser.
- Use Sass
	- npm install node-sass
```js
/* mysass.scss */
$myColor: red;

h1 {
  color: $myColor;
}

/* index.js */
import React from 'react';
import ReactDOM from 'react-dom';
import './mysass.scss';

class MyHeader extends React.Component {
  render() {
    return (
      <div>
      <h1>Hello Style!</h1>
      <p>Add a little style!.</p>
      </div>
    );
   
}
}

ReactDOM.render(<MyHeader />, document.getElementById('root'));
```

### React Forms
- Just like in HTML, React uses forms to allow users to interact with the web page.
- In HTML, form data is usually handled by the DOM. 
- In React, form data is usually handled by the components. When the data is handled by the components, all the data is stored in the component state.

```js
/* Add an event handler in the onChange attribute, and let the event handler update the state object */
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { username: '' };
  }
  myChangeHandler = (event) => {
    this.setState({username: event.target.value});
  }
  render() {
    return (
      <form>
      <h1>Hello {this.state.username}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        onChange={this.myChangeHandler}
      />
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Conditional Rendering */
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { username: '' };
  }
  myChangeHandler = (event) => {
    this.setState({username: event.target.value});
  }
  render() {
    let header = '';
    if (this.state.username) {
      header = <h1>Hello {this.state.username}</h1>;
    } else {
      header = '';
    }
    return (
      <form>
      {header}
      <p>Enter your name:</p>
      <input
        type='text'
        onChange={this.myChangeHandler}
      />
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Submitting Forms */
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { username: '' };
  }
  mySubmitHandler = (event) => {
    event.preventDefault();
    alert("You are submitting " + this.state.username);
  }
  myChangeHandler = (event) => {
    this.setState({username: event.target.value});
  }
  render() {
    return (
      <form onSubmit={this.mySubmitHandler}>
      <h1>Hello {this.state.username}</h1>
      <p>Enter your name, and submit:</p>
      <input
        type='text'
        onChange={this.myChangeHandler}
      />
      <input
        type='submit'
      />
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Multiple Input Fields */
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      age: null,
    };
  }
  myChangeHandler = (event) => {
    let nam = event.target.name;
    let val = event.target.value;
    this.setState({[nam]: val});
  }
  render() {
    return (
      <form>
      <h1>Hello {this.state.username} {this.state.age}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        name='username'
        onChange={this.myChangeHandler}
      />
      <p>Enter your age:</p>
      <input
        type='text'
        name='age'
        onChange={this.myChangeHandler}
      />
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Validating Form Input */
// When you fill in your age
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      age: null,
    };
  }
  myChangeHandler = (event) => {
    let nam = event.target.name;
    let val = event.target.value;
    if (nam === "age") {
      if (!Number(val)) {
        alert("Your age must be a number");
      }
    }
    this.setState({[nam]: val});
  }
  render() {
    return (
      <form>
      <h1>Hello {this.state.username} {this.state.age}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        name='username'
        onChange={this.myChangeHandler}
      />
      <p>Enter your age:</p>
      <input
        type='text'
        name='age'
        onChange={this.myChangeHandler}
      />
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Validating Form Input */
// with the validation at form submit
lass MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      age: null,
    };
  }
  mySubmitHandler = (event) => {
    event.preventDefault();
    let age = this.state.age;
    if (!Number(age)) {
      alert("Your age must be a number");
    }
  }
  myChangeHandler = (event) => {
    let nam = event.target.name;
    let val = event.target.value;
    this.setState({[nam]: val});
  }
  render() {
    return (
      <form onSubmit={this.mySubmitHandler}>
      <h1>Hello {this.state.username} {this.state.age}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        name='username'
        onChange={this.myChangeHandler}
      />
      <p>Enter your age:</p>
      <input
        type='text'
        name='age'
        onChange={this.myChangeHandler}
      />
      <br/>
      <br/>
      <input type='submit' />
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Adding Error Message */
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      age: null,
      errormessage: ''
    };
  }
  myChangeHandler = (event) => {
    let nam = event.target.name;
    let val = event.target.value;
    let err = '';
    if (nam === "age") {
      if (val !="" && !Number(val)) {
        err = <strong>Your age must be a number</strong>;
      }
    }
    this.setState({errormessage: err});
    this.setState({[nam]: val});
  }
  render() {
    return (
      <form>
      <h1>Hello {this.state.username} {this.state.age}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        name='username'
        onChange={this.myChangeHandler}
      />
      <p>Enter your age:</p>
      <input
        type='text'
        name='age'
        onChange={this.myChangeHandler}
      />
      {this.state.errormessage}
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Textarea */
// In HTML the value of a textarea was the text between the start tag <textarea> and the end tag </textarea>
// In React the value of a textarea is placed in a value attribute
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      description: 'The content of a textarea goes in the value attribute'
    };
  }
  render() {
    return (
      <form>
      <textarea value={this.state.description} />
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));

/* Select */
// In HTML, the selected value in the drop down list was defined with the selected attribute
// In React, the selected value is defined with a value attribute on the select tag
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      mycar: 'Volvo'
    };
  }
  render() {
    return (
      <form>
      <select value={this.state.mycar}>
        <option value="Ford">Ford</option>
        <option value="Volvo">Volvo</option>
        <option value="Fiat">Fiat</option>
      </select>
      </form>
    );
  }
}

ReactDOM.render(<MyForm />, document.getElementById('root'));
```

### References
- [Getting Started](https://reactjs.org/docs/getting-started.html)
- [React Tutorial](https://www.w3schools.com/react/default.asp)

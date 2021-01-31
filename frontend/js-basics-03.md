### Modern JavaScript Development
- NPM (Node Packae Manager)
	- Both repository and software
	- Contains open-source package to include 3rd-party code in our own code (e.g React, jQuery, Leaflet, etc)
- Bundling
	- Join all modules into one file
	- webpack or parcel
- Transpiling/Polyfilling
	- Convert modern Javascript back to ES5
	- Babel

### Module
- Reusable piece of code that encapsulates implementation details.
- Usually a standalone file, but it doesn't have to be.
- Why modules
	- Compose software: Modules are small building blocks that we put together to build complex applications.
	- Isolate components: Modules can be developed in isolation without thinking about the entire codebase.
	- Abstract code: Implement low-level code in modules and import these abstractions into other modules.
	- Organized code: Modules naturally lead to a more organized codebase.
	- Reuse code: Modules allow us to easily reuse the same code, even across multiple projects.

```js
import { rand } from './math.js'; // import (dependency)
const p1 = rand(1,6,2);
const p1 = rand(1,6,2);
const scores = {p1, p2};
export {scores}; // export (public API)
```

```js
/* shoppingCart.js */
// Exporting module
console.log('Exporting module');

const shippingCost = 10;
export const cart = [];

export const addToCart = function (product, quantity) {
  cart.push({ product, quantity });
  console.log(`${quantity} ${product} added to cart`);
};

const totalPrice = 237;
const totalQuantity = 23;

export { totalPrice, totalQuantity as tq };

export default function (product, quantity) {
  cart.push({ product, quantity });
  console.log(`${quantity} ${product} added to cart`);
}

/* test.js */
// Exporting and Importing in ES6 Modules

// Importing module
// import { addToCart, totalPrice as price, tq } from './shoppingCart.js';
// addToCart('bread', 5);
// console.log(price, tq);

console.log('Importing module');
// console.log(shippingCost);

// import * as ShoppingCart from './shoppingCart.js';
// ShoppingCart.addToCart('bread', 5);
// console.log(ShoppingCart.totalPrice);

// import add, { addToCart, totalPrice as price, tq } from './shoppingCart.js';
// console.log(price);

import add, { cart } from './shoppingCart.js';
add('pizza', 2);
add('bread', 5);
add('apples', 4);

console.log(cart);
```

### Module Pattern
```js
const ShoppingCart = (function () {
  const cart = [];
  const shippingCost = 10;
  const totalPrice = 237;
  const totalQuantity = 23;

  const addToCart = function (product, quantity) {
    cart.push({ product, quantity });
    console.log(
      `${quantity} ${product} added to cart (sipping cost is ${shippingCost})`
    );
  };

  const orderStock = function (product, quantity) {
    console.log(`${quantity} ${product} ordered from supplier`);
  };

  return {
    addToCart,
    cart,
    totalPrice,
    totalQuantity,
  };
})();

ShoppingCart.addToCart('apple', 4);
ShoppingCart.addToCart('pizza', 2);
console.log(ShoppingCart);
console.log(ShoppingCart.shippingCost);
```

### CommonJS Modules
```js
// Export
export.addTocart = function (product, quantity) {
  cart.push({ product, quantity });
  console.log(
    `${quantity} ${product} added to cart (sipping cost is ${shippingCost})`
  );
};

// Import
const { addTocart } = require('./shoppingCart.js');
```

### Bundling with parcel
- Installation
	- npm i parcel --save-dev // locally
	- npm in parcel -g // globally
- run
	- npx parcel index.html
	- npm run start // "scripts": "parcel index.html"
- build
	- npm run build // "scripts": "parcel build index.html"

```js
/* index.html */
// <script defer src="script.js"></script>

/* script.js */
console.log('Importing module');

import add, { cart } from './shoppingCart.js';
add('pizza', 2);
add('bread', 5);
add('apples', 4);

console.log(cart);

import cloneDeep from 'lodash-es';

const state = {
  cart: [
    { product: 'bread', quantity: 5 },
    { product: 'pizza', quantity: 5 },
  ],
  user: { loggedIn: true },
};
const stateClone = Object.assign({}, state);
const stateDeepClone = cloneDeep(state);

state.user.loggedIn = false;
console.log(stateClone);

console.log(stateDeepClone);

if (module.hot) {
  module.hot.accept();
}
```

### Modern and clean code
- Readable code
	- Write code so that others can understand it
	- Write code so that you can understand it in 1 year
	- Avoid too "clever" and overcomplicated solutions
	- Use descriptive variable names: what they contain
	- Use descriptive function names: what they do
- General
	- Use DRY (Don't Repeat Yourself) principle (refactor your code)
	- Don't pollute global namespce, encapsulate instead
	- Don't use var
	- Use strong type checks (=== and !==)
- Functions
	- Generally, functions should do only one thing
	- Don't use more than 3 function parameters
	- Use default parameters whenever possbile
	- Generally, return same data type as received
	- Use arrow functions when they make code more readable
- OOP
	- Use ES6 classes
	- Encapsulate data and don't mutate it from outside the class
	- Implement method chaining
	- Do not use arrow functions as methods (in regular objects)
- Avoid nested code
	- Use early return (guard clauses)
	- Use ternary (conditional) or logical operators instead of if
	- Use multiple if instead of if/else-if
	- Avoid for loops, use array methods instead
	- Avoid callback-based asynchronous APIs
- Asynchronous code
	- Consume promises with async/await for best readability
	- Whenever possible, run promises in parallel (Promise.all)
	- Handle errors and promise rejections


### Imperative code
- Programmer explains "How to do things"
- We explain the computer every single step it has to follow to achieve a result
- Example: Step-by-step recipe of cake
```js
const arr = [2,4,6];
const doubled = [];
for(let i=0;i<arr.length;i++) {
	doubled[i] = arr[i] * 2;
}
```

### Declarative code
- Programmer tells "What to do"
- We simply describe the way the computer should achieve the result
- The HOW (step-by-step instructions) gets abstracted away
- Example: Description of a cake
```js
const arr = [2,4,6];
const doubled = arr.map(n => n*2);
```

### Functional programming
- Declarative programming paradigm
- Based on the idea of writing software by combining many pure functions, avoiding side effects and mutating data
- Side effect: Modification (mutation) of any data outside of the function (mutating external variables, logging to console, writing to DOM, etc.)
- Pure function: Function without side effects. Does not depend on external variables. Given the same inputs, always returns the same outputs.
- Immutablility: State (data) is never modified. Instead, state is copied and the copy is mutated and returned.
- Examples: React, Redux
- Functional programming techniques
	- Try to avoid data mutations
	- Use built-in methods that don't produce side effects
	- Do data transformations with methods such as .map(), .filter() and .reduce()
	- Try to avoid side effects in functions: this is of course not always possible!
- Declarative syntax
	- Use array and object destructuring
	- Use the spread operator (...)
	- Use the ternary (conditional) operator
	- Use template literals

### References
- [The Complete JavaScript Course](https://www.udemy.com/course/the-complete-javascript-course/)


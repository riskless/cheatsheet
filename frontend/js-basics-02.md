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

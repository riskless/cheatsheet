###  Contents
- Let
- Const
- Data Types
- String
- Function
- Objects
- Classes
- Symbols
- Arrays
- Sets
- Maps
- Promises

### let
- If you use var then the variable is available globally versus let which makes the variable available only in the block
```js
if(true){
  // let x = 10;
  var x = 10;
  document.write("x = " + x + "<br />");
}

// x is undefined here
document.write("x = " + x + "<br />");

// If you use let in a block with the same name
// it won't effect the value outside of the block
var y = 10

if(true){
  let y = 20
}
document.write("y = " + y + "<br />");
```

### const
```js
// You can define constant values that can't change
const PI = 3.14159

// This throws an error
// PI = 2.13

// This won't since the constant is in a block
if(true){
  const PI = 2.13
}
```

### Data Types
```js
// You still don't declare a data type, but instead
// the value defined defines the type being Boolean,Number, String, Object, Symbol
document.write(typeof true + "<br />");
document.write(typeof 3.14 + "<br />");
document.write(typeof "string" + "<br />");
document.write(typeof Symbol() + "<br />");
document.write(typeof {a:1} + "<br />");
document.write(typeof [1,2,3] + "<br />");
document.write(typeof undefined + "<br />");
```

### Strings
```js
// You can use string interpolation using template literals
let fName = "Derek";
let lName = "Banas";
document.write(`${fName} ${lName}` + "<br />");

// Calculation in output using template literals
let num1 = 10
let num2 = 5
document.write(`10 * 5 = ${num1 * num2}` + "<br />");

// You can use tagged template literals to modify output using a function
// The strings array contains the strings and the values array the substitutions
function doMath(strings, ...values) {
  if (strings[0] == 'Add'){
    document.write(`${values[0]} + ${values[1]} = ${values[0] + values[1]} <br />`);
  } else if (strings[0] == 'Sub'){
    document.write(`${values[0]} - ${values[1]} = ${values[0] - values[1]} <br />`);
  }
}
doMath`Add${10} ${20}`;
doMath`Sub${10} ${20}`;

// Iterate over characters
for(let c of fName){
  document.write(`${c} <br />`);
}

// Repeat a string
document.write("Hello ".repeat(3) + "<br />");

// Does a string start with a value
document.write(fName.startsWith("De") + "<br />");

// Does it end with
document.write(fName.endsWith("ek") + "<br />");

// Does it include
document.write(fName.includes("ere") + "<br />");

// Multiline strings
let multilineStr = "This is \
a multiline \
string";

document.write(`${multilineStr} <br />`);

let anothermlstr = `Another
multiline
string`;

document.write(`${anothermlstr} <br />`);
```

### Functions
```js
// Default values are defined next to parameters
function getSum(num1 = 1, num2 = 1){
  document.write(`${num1} + ${num2} = ${num1+num2}<br />`);

  // arguments[] only receives the value passed
  document.write(`${arguments[0]} + ${arguments[1]}<br />`)
}

getSum(3);

// Rest parameters, which are preceeded by ...
// become an array
// You can only have 1 rest parameter and it must be defined last

function getSumMore(...vals){
  let sum = 0;
  for(let i = 0, len = vals.length; i < len; i++){
    sum += vals[i];
  }
  document.write(`The sum is ${sum}<br />`);
}
getSumMore(1,2,3,4);

// You can also pass arrays
let vals = [1,2,3,4,5];
getSumMore(...vals);

// Arrow functions define parameters followed by the body of the function
let difference = (num1, num2) => num1 - num2;
document.write(`5 - 10 = ${difference(5,10)}<br />`);

// They can contain more then 1 statement
let mult = (num1, num2) => {
  let product = num1 * num2;
  document.write(`${num1} * ${num2} = ${product}<br />`);
};
mult(5,50);

// You can use arrow functions with map, filter and map
let valArr = [1,2,3,4,5];

// Reduce applies a function against an accumulator to get a single result
let sumVals = valArr.reduce((a, b) => a + b);
document.write(`Sum : ${sumVals}<br />`);

// Filter returns those values that pass the condition
let evens = valArr.filter(v => v % 2 == 0);
document.write(`Evens : ${evens}<br />`);

// Map performs the given action on every item passed
let doubles = valArr.map(v => v * 2);
document.write(`Doubles : ${doubles}<br />`);
```

### Objects
```js
// You create object literals like this
function createAnimal(name, owner){
  return {
    // Properties
    name,
    owner,
    // Create a method
    getInfo(){
      return `${this.name} is owned by ${this.owner}`
    },
    // Objects can contain other objects
		address: {
			street: '123 Main St',
			city: 'Pittsburgh'
		}
  };
}
var spot = createAnimal("Spot", "Doug");

// Execute method
document.write(`${spot.getInfo()}<br />`);

// Access object in the object
document.write(`${spot.name} is at ${spot.address.street}<br />`);

// Get properties and methods of object
document.write(`${Object.getOwnPropertyNames(spot).join(" ")} <br />`);

// You can store values from Objects with destructoring
let { name, owner } = spot;
document.write(`Name : ${name}<br />`);

// Get the inner class value
let { address } = spot
document.write(`Address : ${address.street}<br />`);

// You can destructor arrays as well
let favNums = [2.718, .5772, 4.6692];
let [,,chaos] = favNums;
document.write(`Chaos : ${chaos}<br />`);

// You can use rest items to grab part of an array
let [, ...last2] = favNums;
document.write(`2nd Num : ${last2[0]}<br />`);

// This can be used to switch values
let val1 = 1, val2 = 2;
[val1,val2] = [val2,val1];
document.write(`Val2 : ${val2}<br />`);
```

*** Classes
```js
// Classes now work much like they do in other languages
class Mammal{
  constructor(name){
    this._name = name;
  }

  // Getter
  get name() {
    return this._name;
  }

  // Setter
  set name(name){
    this._name = name;
  }

  // Static Mammal creator
  static makeMammal(name){
    return new Mammal(name);
  }

  getInfo(){
    return `${this.name} is a mammal`;
  }

}

// Create an object
let monkey = new Mammal("Fred");

// Change name
monkey.name = "Mark";

// Call getter
document.write(`Mammal : ${monkey.name}<br />`);

// Create Mammal using static function
let chipmunk = Mammal.makeMammal("Chipper");
document.write(`Mammal 2 : ${chipmunk.name}<br />`);

// You can inherit properties and methods with extends
class Marsupial extends Mammal{
  constructor(name, hasPouch){
    // Call the super class constructor
    super(name);
    this._hasPouch = hasPouch;
  }

  get hasPouch() {
    return this._hasPouch;
  }

  set hasPouch(hasPouch){
    this._hasPouch = hasPouch;
  }

  // You can override methods
  getInfo(){
    return `${this.name} is a marsupial`;
  }

}

let kangaroo = new Marsupial("Paul", true);
document.write(`It is ${kangaroo.hasPouch} that ${kangaroo.name} has a pouch<br />`);

// Test overridden method
document.write(`${chipmunk.getInfo()}<br />`);
document.write(`${kangaroo.getInfo()}<br />`);

// You can dynamically inherit from Classes
function getClass(classType){
  if (classType == 1) {
    return Mammal;
  } else {
    return Marsupial;
  }
}

class Koala extends getClass(2){
  constructor(name){
    super(name);
  }
}

let carl = new Koala("Carl");
document.write(`${carl.getInfo()}<br />`);
```

### Symbols
- A Symbol is like an enumerated type that can be used as identifiers and they can't be changed (immutable).
```js
// Create a symbol that is used like a label in an array
// You can provide a description in quotes
let capital = Symbol("State Capital");

let pennsylvania = {};
pennsylvania[capital] = "Harrisburg";
document.write(`Capital of PA : ${pennsylvania[capital]}<br />`);

// Get the description
document.write(`Symbol Capital : ${capital.toString()}<br />`);

// You can share symbols by using symbol.for()
let employNum = Symbol.for("Employee Number");

let bobSmith = {};
bobSmith[employNum] = 10;

let sallyMarks = {};
sallyMarks[employNum] = 11;

document.write(`Bob : ${bobSmith[employNum]}<br />`);
document.write(`Sally : ${sallyMarks[employNum]}<br />`);
```

### Arrays
```js
// Array.of() is used to create arrays instead of the array constructor
let array1 = Array.of(1,2,3);

// Create an object into an array
let array2 = Array.from("word");

// You can use Array.from to manipulate values
let array3 = Array.from(array1, (value) => value * 2);

// Iterate over values
for (let val of array3) document.write(`Array Val : ${val}<br />`);
```

### Sets
```js
// A Set is a list of values with no duplicates
var randSet = new Set();
randSet.add(10);
randSet.add("Word");

// Check to see if set contains a value
document.write(`Has 10 : ${randSet.has(10)}<br />`);

// Get size of Set
document.write(`Set Size : ${randSet.size}<br />`);

// Delete item from list
randSet.delete(10);

// Iterate a Set
for (let val of randSet) document.write(`Set Val : ${val}<br />`);
```

### Maps
```js
// A Map is a collection of key/value pairs
var randMap = new Map();
randMap.set("key1", "Random String");
randMap.set("key2", 10);

// Get values
document.write(`key1 : ${randMap.get("key1")}<br />`);
document.write(`key2 : ${randMap.get("key2")}<br />`);

// Get size
document.write(`Map Size : ${randMap.size}<br />`);

// Iterate Map
randMap.forEach(function(value, key){
  document.write(`${key} : ${value}<br />`);
});
```

### Promises
```js
// Promises define code that is to be executed later
// Promises either succeed or fail once
// They either are fulfilled, rejected, pending, or settled

// A Promise that is handled immediately
var p1 = Promise.resolve('Resolve Me');

// then takes 2 optional arguments being first a callback for a success and another for failure
p1.then((res) => document.write(`${res}<br />`));

// Create a promise that executes after 2 seconds
var p2 = new Promise(function(resolve, reject){
  setTimeout(() => resolve('Resolve Me'), 2000);
});

p2.then((res) => document.write(`${res}<br />`));

// Here I demonstrate how then is used if a promise is fulfilled or rejected
let randVal = 18;
var p3 = new Promise(function(resolve, reject){
  if (randVal == 6){
    resolve("Good Value");
  } else {
    reject("Bad Value");
  }
});

p3.then(
	(val) => document.write(`${val}<br />`),
	(err) => document.write(`${err}<br />`)
);

// You should add catch to a chain to handle errors
var p4 = new Promise((resolve, reject) => {
  if (randVal <= 17){
    throw new Error("Can't Vote"); // Same as a Reject
  } else {
    resolve("Can Vote");
  }
});

p4.then((val) => document.write(`${val}<br />`))
	.catch((err) => document.write(`${err.message}<br />`));
```

### References
- [ECMAScript 6 Tutorial](https://www.youtube.com/watch?v=Jakoi0G8lBg&list=PLGLfVvz_LVvSX7fVd4OUFp_ODd86H0ZIY&index=43)

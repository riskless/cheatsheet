### Looping through arrays
```javascript
var shoppingList = ['orange', 'apple', 'banana'];
console.log(shoppingList);

for(var i = 0; i < shoppingList.length; i ++){
  console.log(shoppingList[i]);
}

shoppingList.forEach(function(element){
  console.log(element);
});

//ES6
for (element of shoppingList){
  console.log(element);
}
```
### Function statement/declaration
- Hoisting (O)
```javascript
//function statement
console.log(sum(3,5));
function sum(a, b){
    return a + b;
}
```
### Function expression 
- Hoisting (X)
```javascript
//function expression
var greet = function(name){
    console.log("Hello " + name + "!");
};
```
### IIFE (Immediately Invoked Function Expressions)
```javascript
(function(){
  console.log("Hi!");
})();

(function(){
  console.log("Hi!");
}());
```
### Recursion
```javascript
var factorial = function(n){
    if(n == 1){
        return 1;
    }else{
        return n * factorial(n-1);
    }
};
console.log(factorial(3));//1*2*3
console.log(factorial(4));//1*2*3*4
//factorial(n) = n * factorial (n-1)
```
### Singleton Design Pattern
```javascript
var signupController = window.signupController || {};
signupController.registrationPage = {
  // code
    
  //Initialization method
  init: function(){
    // some code
  },
    
  method1 : function(){
    // some code
  }   
};

//invoke the init method after the page loads
codepion.registrationPage.init();
```

### Module Design Pattern
- UI Module, Events Module, Data Module and more.
```javascript
var myNamespace = window.myNamespace || {};

//module
myNamespace.module = (function(){
    //private members
    var privateProperty1 = false;
    var privateProperty2 = [1, 2, 3];
    function privateMethod1(){
        console.log('Hi');
    }
    function privateMethod2(){
        console.log('Hello');
    }
    
    //return object
    return{
        //public members
        publicProperty1: true,
        publicProperty2: 10,
        publicMethod1: function(){
            console.log(privateProperty1);
            privateMethod1();
        },
        publicMethod2: function(){
            console.log(privateProperty2);
        }
    }
})();

myNamespace.module.publicMethod1();
myNamespace.module.publicMethod2();
```

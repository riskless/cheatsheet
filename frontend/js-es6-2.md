### Variables and Scope
```js
/* var - scoping and hoisting */
//ES5: we declared variables using var
var x = 1; //global scoped

//variables were global scoped or function scoped
function add1(y){
    var x = 2; //function scoped
    return y + x;
}
console.log(add1(3));//5

function add2(y){
    return y + x;
}
console.log(add2(3));

//They were also hoisted
console.log(z);//undefined, no error thanks to hoisting
var z = 1;
console.log(z);

/* let - scoping and hoisting */
//ES6
//let x = 1;
//redeclaration throws an error unlike var

//scope
let x = 1;
function add(y){
    return y + x;
}
console.log(add(3));
//so let variables can be accessed from inside functions if declared in their outer environment just like var

function add2(y){
    let x = 2;
    return y + x;
}
console.log(add2(3));

//hoisting
//console.log(z);
//temporal dead zone
let z;
console.log(z);
z = 1;
console.log(z);

/* Block Scoping */
let y = 2;

{
    let x = 1;
    let z = 2;
    console.log(z);//2
}

{
    //console.log(x);//error
    let z = 4;//no redeclaration error, variables are in different scopes now.
    console.log(z);
    console.log(y);//2
}

//ES5 using IIFE (fake scoping)

//(function(){
//    let x = 1;
//    let z = 2;
//    console.log(z);
//});
//(function(){
//    let z = 4;
//    console.log(z);
//    console.log(y);
//})


//let mark = 80;
//if(mark > 50){
//    let pass = true;
//}else{
//    let pass = false;
//}
//console.log(pass);


//to fix this:

let pass;
let mark = 80;
if(mark > 50){
    pass = true;
}else{
    pass = false;
}
console.log(pass);

/* Functions and Blocking Scoping */
greet();//hi
function greet(){
    console.log('hi');
}
greet();//hi

{
    function greet(){
        console.log('hello');
    }
    greet();//hello
    
    {
        function greet(){
            console.log('hi there');
        }
    }

}
greet();//hi

{
    function greet(){
        console.log('hi hello');
    }
}
greet(); // hi

/* let scope vs closures */
//for(var i = 0; i < 3; i ++){
//    document.getElementById(i).addEventListener('click', function(){
//        console.log(i);
//    })
//}

//Solution in ES5: use a closure
//var f = function(x){
//    return function(){
//        console.log(x);
//    }
//}
//
//for(var i = 0; i < 3; i ++){
//    document.getElementById(i).addEventListener('click', f(i))
//}
//f(0), f(1), ...


//Now try with let:

for(let i = 0; i < 3; i ++){
    document.getElementById(i).addEventListener('click', function(){
        console.log(i);
    })
}
```

### To be continued...

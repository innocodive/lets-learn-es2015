<h1>Block Bindings</h1>

Variable scoping has always been done using `functions` or `IIFE` - Immediately Invoked Function Expressions in Javascript. ES6 (ES2015) introduced <b>Block Scoping</b>. We need `{}` and `let` to create a new scope.
When we declare a variable in Javascript using `var`, the declaration gets hoisted to the top of the function or to the top of the code if it is declared outside the function. ES6 introduced a <b>Block-level</b> Declaration. In block-level decrations variables are not accessible outside the block `{}` or functions. We declare variables using `let` keyword. While hoisting works for `var` declarations it wouldn't work for `let` declarations. That is why it is a good practice to declare variables at the top of the scope or a function, when we use `let`.

```javascript
	var myAge = 36, age = 50;
	if(age > myAge) {
		let myAge = age;
		console.log("now I became older, which is " + Myage);
	} else {
		let age = myAge;
		console.log("we are at the same age, which is " + myAge);
	}
	console.log("myAge outside the scope: " + myAge);
```
We cannot redeclare the already declared variable in a same scope:
```javascript
	var myAge = 20;
	console.log(myAge);
	let myAge = 25;		// this throws a syntax error
	console.log(myAge);
```
If we modify this example:
```javascript
	var myAge = 20;
	console.log(myAge);
	if(myAge > 10) {
		let myAge = 25;
		console.log(myAge);
		}
```
In this case, we create a new variable myAge inside the if scope, that is why myAge in the if scope is different from the myAge in the global scope.
<h4>Declaring Constants in ES6</h4>

constants are declared using `const` keyword. Once they are declared you cannot change their values. Constants have to be initialized when they are declared.
```javascript
	const myAge = 30;
	const yourAge;	// it will throw a syntax error. needs to be initialized when declared
```
Constants are also a block-level declarations and variable hoisting wouldn't work for constants. If we try to redeclare already declared variables using `var` or `let` keywords, constant declaration will throw an error:
```javascript
	var myAge = 20;
	let yourAge = 30;

	const myAge = 25;
	const yourAge = 23;
```
<h4>Declaring Objects as Constants</h4>
If we create an object using a `const` keyword, we cannot change the object name but we can change the object content (variable and method values). Situation is the ame with arrays. 

```javascript
	const newObject = {
		myAge : 25
	};

	newObject.myAge = 30;	//this works

	newObject = {
		myName : "Max"
	};						// will throw an error
```

`newObject` variable doesn't hold the actual object but a reference to that object. That is why the content of the object can be modified, but not the binding/reference to that object.

<h4>Temporal Dead Zone (TDZ)</h4>
When Javascript Engine reads through your code, it would look for the variable declarations. If it finds `var` declarations, it would be hoisted to the top of the global scope or a function scope. Because hoisting for `let` and `const` wouldn't work, they will be put in the Temporal Dead Zone. TDZ is basically <b>accessing-too-early</b> error. We need to also mention the unusual behaviour of `typeof`. We use `typeof` to check if any variable is declared or not. If the variable is declared using `let` after we check its type, it will throw a reference error. Because the variable is in TDZ. To avoid these type error, we should always declare the `let` vars before we access them. Have a look at the example below:

```javascript
	if(typeof x === undefined) {
		console.log("not yet");
	}
	if(typeof y === undefined) {	// this will throw a Reference Error
		console.log("too early");
	}
	let y;
```

<h4>`var` and `let` in Loops</h4>

Have a look at this code snippet:

```javascript
	var numbers = [];

	for(let i = 0; i < 4; i++) {
		numbers.push(function() {
			console.log(i);
			});
	}
	numbers[2]();	// 2
```
Here `let i` first declares/initializes `i = 0`, and then redeclares a `new i` for each iteration of the Loop. To see the difference, replace `let i` with `var i`.

Now have a look at this example: 
```javascript
	for (var i = 0; i < 5; i++) {
  		console.log(calculate(i));
	}
	console.log("this is i outside the for Loop: " + i);	// i is 5 here

	function calculate(i) {
  		return i * 3;
	}
```
Because `i` is declared using `var`, it gets hoisted to the top of the scope. That is why we can access to the value of the variable `i` outside the for loop. Replace the `var` with `let` and see the result. You will get "i is not defined" message.

<h4>`const` in Loops</h4>

Have a look at this example:
```javascript
	for(const i = 0; i < 5; i++) {
		console.log(i);
	}
```

In this example, declaring a `const` and initializing is fine. First iteration will work. When it comes to `i++` stage it will throw an error, as you are attempting to change the value of `const i`. Have a look at another example:

```javascript
	var obj = {
  		name : "Max",
  		surname : "Charyyev",
  		age : 20
		};

		for(const key in obj) {
  			console.log(key);
		}
```
Here, everything works. when it comes to `for ... in` and `for ... of` Loops, loop iteration creates a new binding for each iteration. We are not changing they `const key` value.

<h4>`let` and `const` in the Global Scope</h4>

Declaring a new variable in the Global Scope using `var` creates a new property for the Global Object (`window` in the browser). If we use `let` or `const` instead, we create a new binding in the Global Scope but we don't create a new property for window object. This also means we can't change existing global variables. Have a look at the example below:
```javascript
	let name = "Max";

	console.log(name);
	console.log(window.name === name);	// false
```
<h4>How functions behave in a Block Scope?</h4>

2 very important things to mention when we use Functions in blocks `{}`:
1) functions get hoisted to the top of the block. We don't have a TDZ problem here unlike `let` or `const` - declared variables in the Block Scope.
2) Conditional Function Declarations in Block Scope is very different from ES5.

Have a look at this example:

```javascript
	{
		func();	//this works. can call a function before it is declared
		function func() {}
	}
		func();	// Reference Error. func() doesn't exist outside the `{}` block

	// block-scoped function declaration:

	var a = 5;
	if (a > 5) {
    function foo() {
        console.log( "1" );
    	}
	}
	else {
    function foo() {
        console.log( "2" );
    	}
	}

	foo(); 	// you get 2 in ES5, in ES6 you will get a Reference Error.
```

<b>best practice for `block bindings`:</b>
<p>Use `const` for a variable declarations as default and use `let` if you know the variable value will change later.</p>
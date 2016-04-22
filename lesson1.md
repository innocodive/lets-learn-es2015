<h1>Block Bindings</h1>

When we declare a variable in Javascript using `var`, the declaration gets hoisted to the top of the function or to the top of the code if it is declared outside the function. ES6 introduced a <b>Block-level</b> Declaration. In block-level decrations variables are not accessible outside the block `{}` or functions. We declare variables using `let` keyword. While hoisting works for var declarations it wouldn't work for let declarations. That is why it is a good practice to declare variables at the top of the scope or a function.
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
If we create an object using a `const` keyword, we cannot change the object name but we can change the object content (variable and method values);

```javascript
	const newObject = {
		myAge : 25
	};

	newObject.myAge = 30;	//this works

	newObject = {
		myName : "Max"
	};						// will throw an error
```

<h4>Temporal Dead Zone (TDZ)</h4>
When Javascript Engine reads through your code, it would look for the variable declarations. If it finds `var` declarations, it would be hoisted to the top of the global scope or a function scope. Because hoisting for `let` and `const` wouldn't work, they will be put in the Temporal Dead Zone. 

<h4>`var` and `let` in Loops</h4>
Have a look at this example: 
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
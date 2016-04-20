<h1>Block Bindings</h1>

When we declare a variable in Javascript using var, the declaration gets hoisted to the top of the function or to the top of the code if it is declared outside the function. ES6 introduced a Block-level Declaration. In block-level decrations variables are not accessible outside the block {} or functions. We declare variables using "let". While hoisting works for var declarations it wouldn't work for let declarations. That is why it is a good practice to declare variables at the top of the scope or a function.
```javascript
	var myAge = 36;
	if(age > myAge) {
		let myAge = age;
		console.log("now you became younger");
	} else {
		let age = myAge;
		console.log("we are at the same age");
	}
	console.log("myAge outside the scope: " + myAge);
```
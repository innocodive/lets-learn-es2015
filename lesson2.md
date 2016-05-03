<h3>Functions</h3>

<h4>Setting Default Values for a function Parameter</h4>

Setting Default Function parameter values didn't exist in pre-ES6 Javascript. We used to simulate that feature using Logixal OR (||) operator.

```javascript
	function myProfile(name, age) {
		name = name || "Max";
		age = age || 25;

		console.log(name + "\n" + age);
	}

	myProfile("Maxim", 35);
``` 
Here is how we will achieve this in ES6:

```javascript
	function myProfile(name, surname, age = 25, height = 183, weight) {
  		console.log("Name: " + name + "\nSurname: " + surname + "\nAge: " + age);
  		console.log("Height: " + height + "cm\nWeight: " + weight + "kg");
	}

	myProfile("Max", "Charyyev", undefined, undefined, 75);
```
myProfile() function expects `name, surname and weight` parameters always be passed. `age and height` parameters have default values. When we call we **myProfile() function** we can either pass different values for `age, height` or we can use `default` values. To do that we need to pass `undefined` to the **myProfile()** function. We cannot pass `null`. It is considered to be valid value.

1 more example on Setting a Default Value for a Function paramater:

```javascript
	function foo(x = 5, y = 10) {
    	console.log( x + y );
	}

	foo();
	foo( 10, 16 );
	foo( 0, 12 );

	foo( 8 );
	foo( 8, undefined );
	foo( 8, null );

	foo( undefined, 12 );
	foo( null, 12 );
```
This is how your Function `foo()` would look like in pre-ES6 Javascript:

```javascript
	function foo() {
    var x = arguments.length <= 0 || arguments[0] === undefined ? 5 : arguments[0];
    var y = arguments.length <= 1 || arguments[1] === undefined ? 10 : arguments[1];

    console.log(x + y);
}
```

<h4>`arguments` object behaviour</h4>

`arguments` object behaviour changes when there is a default parameter values exist. It behaves the same in both ES5 strict Mode and ES6. In ES5 nonstrict mode the changes in function default parameter values reflects in `arguments` object but ES5 strict mode and ES6 changes in function default parameter values doesn't effect the `arguments` object. Have a look  at this example:

```javascript
	function argsx(one = 10, two) {
  //console.log(one + "\n" + two);
  
  console.log(arguments[0] === undefined);  //true
  console.log(arguments[1] === two);        //true
  
  one = 35;
  two = 45;
  
  console.log(arguments[0] === one);        //false
  console.log(arguments[1] === two);        //false
}

argsx(undefined, 20);
```

<h4>Using Functions as a Default Function Parameter Value</h4>

Default Function Parameter Values could also be a function:

```javascript
	function getSurname() {
  		return "Charyyev";
	}

	function getInitials(name, surname = getSurname()) {
  		return name + " " + surname;
	}

	console.log(getInitials("Max"));
	console.log(getInitials("Max", "Mulkomanoff"));
```

<h4>Function Default Parameter Value TDZ</h4>

Function parameter initialization only happens when we <u>call</u> the function. If we try to access to any Function parameter which is not initialized yet, will throw an error. Situation here is exactly the same with what we experienced with `let` and `const`. Uninitialized function parameters are in TDZ, when they are called. Have a look at this example:

```javascript
	function getResult(num1 = num2, num2) {
  		return num1 + num2;
	}

	console.log(getResult(10, 20));			//30
	console.log(getResult(undefined, 10));	//num2 is not defined
```
When we call getResult() function with parameters `getResult(undefined, 10)`, will throw an error, because when `num1` is initialized `num2` is not initialized yet, it is in TDZ. 

From <strong>Understanding ECMAScript 6</strong> book:
> Function parameters have their own scope and their own TDZ that is separate from the function body scope. That means the default value of a parameter cannot access any variables declared inside the function body.

<h4>Spread or Rest Operator</h4>

ES6 introduced a new operator called <b>Spread/Rest Operator (...)</b>. Main reason behind the introduction of the `REST parameter` is to replace `arguments` array-like object. Have a look at the example below:

```javascript
	function foo(...args) {
    // `args` is already a real array

    console.log( ...args );
}
foo(1, 2, 3, 4, 5);		// 1, 2, 3, 4, 5
```
We used to do (ES5):
```javascript
	var args = Array.prototype.slice.call( arguments );		
	// arguments is an array-like object. turn `arguments` into a real array
	// and
	// pass all `args` as arguments to `foo(..)`
    foo.apply( null, args );
```

`Rest parameter` comes with 2 restrictions:<br/>
1) We can only have 1 rest parameter and it should be the last Function parameter.

```javascript
	function foo(...args, newargs) {
    // `args` is already a real array

    console.log( ...args );
}
foo(1, 2, 3, 4, 5);		// throws an error
```

2) rest parameter cannot be used in Object setter:

```javascript
	let ES6Object = {

    set param(...value) {}	//We will se a message "Setter must have exactly one formal parameter."
	};
```

<strong>Understanding ECMAScript 6</strong> book:
> This restriction exists because object literal setters are restricted to a single argument. Rest parameters are, by definition, an infinite number of arguments, so they’re not allowed in this context.


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

<h4>Spread or Rest Operator</h4>

ES6 introduced a new operator called <b>Spread/Rest Operator (...)</b>. Have a look at the example below:

```javascript
	function foo(...args) {
    // `args` is already a real array

    console.log( ...args );
}
foo(1, 2, 3, 4, 5);		// 1, 2, 3, 4, 5
```
We used to do
```javascript
	var args = Array.prototype.slice.call( arguments );		// turn `arguments` into a real array
	// and
	// pass all `args` as arguments to `foo(..)`
    foo.apply( null, args );
```
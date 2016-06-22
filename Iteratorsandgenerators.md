<h4>Iterators and Generators</h4>
<strong>Basic Rules for iterators:</strong><br>

Iterator is an object with `next()` method which in turn also returns a new object. A returned new object has 2 properties: `value` and `done`. `Value` points to the next element in an array and `done` is a boolean which is always false unless it is the end of the array.<br>
If we call `next()` method after we reach to the end of the array, `done` will return <i>true</i> and value will have <i>undefined</i> value.<br>

<h5>Generator</h5>
ES6 introduced a `generator` function which returns an iterator.

```javascript
	function *Generate(data) {
  		for (let i = 0; i < data.length; i++) {
    		yield data[i];
  		}
	}

	let generate = Generate(["developer", "engineer", "programmer"]);

	console.log(generate.next());
	console.log(generate.next());
	console.log(generate.next());
	console.log(generate.next());

	console.log(generate.next().value);
	console.log(generate.next().done);
```

Some important points about Generators:
- Generator function is a regular function with (*) in front of the function name
- `yield` is an "iterable".
- Generator Function stops execution after each yield statement
- `yield` keyword can only be used inside the Generator function. We cannot use it even with functions inside the generator function. In that sense, `yield` acts like a `return` keyword.
- We cannot create an Arrow Function which is also a Generator Function
- Generators assign `Symbol.iterator` property by default

Generator functions are just functions, that is why they could be used as we use regular functions.

```javascript
	// Generator Function Expression
	let Generator = function *(data) {
  		yield data[0];
	}
```
Generators can be an object method:
```javascript
	let object = {
  		*generate(data) {
    		for(let i=0; i < data.length; i++) {
      		yield data[i];
    		}
  		}
	};

	let gen = object.generate([1, 2, 3]);

	console.log(gen.next());
	console.log(gen.next());
	console.log(gen.next());
```
<h5>Iterables</h5>

Arrays, Sets, Maps and Strings are `Iterables` in ES6. All iterables have `Symbol.iterator` property. `Symbol.iterator` well-known symbol (function) returns an iterator for the given object. Iterators which are created by Generators are also iterables.<br>
ES6 introduced `for of` loop. Iterables are designed to be used with `for of` loop. `for of` loop makes iteration over Javascript collection objects easy. That way we can focus on the content of the collection objects more than tracking the index.

```javascript
	let goodHabits = ["eating healthy", "training", "coding"];

	for(let habit of goodHabits) {
  		console.log(habit);
	}
```
What happens in this code:
- First `for of` loop call `Symbol.iterator` on an array to get the iterator
- Then `ietrator.next()` method is called. It returns a result object with `value` property
- `Value` is stored in `habit` variable
- When `done` property on the result object is `true`, loop ends. `habit` never gets assigned an `undefined`

note: We cannot use `for of` loop on null, undefined or non-iterable object.<br>
<strong>How does `for of` work?</strong>

- get the default iterator
- check if the object is iterable

Have a look at the examples below:

<strong>get the default Iterator?</strong>
```javascript
	let goodHabits = ["eating healthy", "training", "coding"];

	let habit = goodHabits[Symbol.iterator]();
	console.log(habit.next());
```
<strong>check if the object is `iterable`?</strong>

```javascript
	function isIterable(collection) {
  		return typeof collection[Symbol.iterator] === "function";
	}

	console.log(isIterable(new Map()));       //true
	console.log(isIterable([1, 2, 3]));       //true
	console.log(isIterable(new WeakMap()));   //false
	console.log(isIterable(new Set()));       //true

```
All these happens behind the scenes.

<h5>Create your own iterables using `Symbol.iterator`</h5>
Logic behind is 1st to create a collection object like Arrays or Sets and etc. How do we do it?<br>
We know all collection objects have `[Symbol.iterator]` well-known Symbol on them. So, that is what we need to do:

```javascript
	let iterableObject = {
  		items : [],
  		*[Symbol.iterator]() {				// this is a (*)generator.
    		for(let item of this.items) {
      		yield item;
    		}
  		} 
	};

	let habits = ["eating healthy", "training", "coding"];
		habits.forEach(function(habit) {
  			iterableObject.items.push(habit);
	});

	for(let object of iterableObject) {
  		console.log(object);
	}	
```

<h4>Built-in Iterators</h4>
- Collection Iterators: Arrays, Sets & Maps. All 3 have these built-in iterators
  - entries() iterator returns an iterator whose values are key-value pairs
  - values() iterator returns an iterator whose values are values of the collection
  - keys() iterator iterator returns an iterator whose values are collection keys
- String Iterators
- NodeList Iterators

<h5>Collection Iterators</h5>
Collection Iterators are the most common iterators. We had a look at the Array Iterators above using some examples.
<strong>entries() iterator</strong>
It returns a key-value pairs for each item in a collection, everytime `next()` is called.

```javascript
	let habits = ["eating healthy", "training", "coding"];
	let isbn = new Set([1234, 9876, 4536, 7653]);
	let learning = new Map([["name", "Max"], ["age", 25], ["love", "coding"]]);

	for(let entry of habits.entries()) {
  		console.log(entry);
	}

	for(let entry of isbn.entries()) {
  		console.log(entry);
	}

	for(let entry of learning.entries()) {
  		console.log(entry);
	}
	/*
		result we get
		[0,"eating healthy"]
		[1,"training"]
		[2,"coding"]
		[1234,1234]
		[9876,9876]
		[4536,4536]
		[7653,7653]
		["name","Max"]
		["age",25]
		["love","coding"]
	*/
```
<strong>values() iterator</strong>
It basically returns the stored values of the collection. In this line of code `for(let entry of habits.entries()) {`, we just need to replace `entries()` with `values()`. <br>
`for(let entry of habits.values()) {`. We will get only values, not key-value pairs.<br>
<strong>keys() iterator</strong> returns only keys.

<h5>String Iterators</h5>
ES5 didn't fully support Unicode, but ES6 does. Bracket notation in ES5 would iterate over code units, not characters, but ES6 works the opposite way.
```javascript
	let data = "AȺʆ";

	for(let x of data) {
  		console.log(x);
	}
```
<h5>NodeList Iterators</h5>

NodeList is a type that represents a collection of elements. NodeList's default iterator acts the same as Array iterators. We can use `for of` loop to iterate NodeList elements.

```javascript
	let divs = document.querySelectorAll("div");

	for(let div of divs) {
  		console.log(div.id);
	}
```

<h5>Better use of Iterators</h5>
<strong>Passing arguments to iterators</strong><br>

```javascript
	function *iterate() {
  		let first = yield  1;
  		let second = (yield first * 2) + 5;
  		let third = (yield second * 3);
	}

	let it = iterate();

	console.log(it.next());     //{"value":1,"done":false}
	console.log(it.next(2));    //{"value":4,"done":false}
	console.log(it.next(5));    //{"value":30,"done":false}
	console.log(it.next(2));    //{"value":undefined, "done":true}
```

<strong>Passing Error Conditions to Iterators</strong>
```javascript
	function *iterate() {
  		let first = yield  1;
  		let second = (yield first * 2) + 5;
  		let third = (yield second * 3);
	}

	let it = iterate();

	console.log(it.next());     //{"value":1,"done":false}
	console.log(it.next(2));    //{"value":4,"done":false}
	console.log(it.throw(new Error("I am done with looping")));
```
We pass an `Error` object to the `throw()` method. This stops the code execution.<br>
We can catch errors inside the Generator using `try catch` block.
```javascript
	function *iterate() {
  		let first = yield  1;
  		let second;
  		try {
    		second = yield first + 17;
  			} catch(err) {
    			second = 29;
  		}
  		yield second + 14;
  		yield second + 11;
	}

	let it = iterate();

	console.log(it.next());
	console.log(it.next(5));
	console.log(it.throw(new Error("I am done with looping")));
	console.log(it.next());
	/*
		{"value":1,"done":false}
		{"value":22,"done":false}
		{"value":43,"done":false}
		{"value":40,"done":false}
	*/
```
Because, Generators are regular Functions, we can specify a return statement to return any value from the Generate when `next()` is called.

```javascript
	function *itReturn() {
  		yield 1;
  		return 20;
  		yield 2;
  		return 10;
	}

	let iterate = itReturn();

	console.log(iterate.next());
	console.log(iterate.next());
	console.log(iterate.next());
	/*
		{"value":1,"done":false}
		{"value":20,"done":true}
		{value:"undefined", "done":true}
	*/
```
2nd `next()` returns a `return` statement value. Any `next()` calls after the return statement will return a `undefined` value. Any other yields wouldn't be executed, but a returned object's value would be set to `undefined`.

<strong>`speread` operator and `for of` ignore the return statement value</strong>

<h5>Delegating Generators</h5>

We can combine many generators to use their values together, to pass their values or return statement values to each other.
```javascript
	function *age() {
  		yield 20;
  		yield 30;
  		return 2;
	}

	function *skills(number) {
  		for(let i = 0; i < number; i++){
    		yield "web developer";
  		}
	}

	function *combine() {
  		let result = yield *age();
  		yield result;
  		yield *skills(result);
	}

	let iterate = combine();

	console.log(iterate.next());
	console.log(iterate.next());
	console.log(iterate.next());
	console.log(iterate.next());
	console.log(iterate.next());
	console.log(iterate.next());
	/*
		{"value":20,"done":false}
		{"value":30,"done":false}
		{"value":2,"done":false}
		{"value":"web developer","done":false}
		{"value":"web developer","done":false}
		{"value":undefined, "done":true}
	*/
```
So, the way we do the generator Delegation is we use yield with generator names. That way when `next()` is called yield will delegate the call to the relevant Generator. `return` statement value is also used.

<h5>Asynchronius Task Running</h5>
The reason why Generators are related to Asynchronius Programming is the fact that we can pause code in the middle of the execution.

```javascript
	$(".button").on("click", function(data)) {
  		//do something
	}
```
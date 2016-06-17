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

Arrays, Sets, Maps and Strings are `Iterables` in ES6. All iterables have `Symbol.iterator` property. `Symbol.iterator` well-known symbol (function) returns an iterator for the given object. Iterators which are created by Generators are also iterables. 

```javascript
	let goodHabits = ["eating healthy", "training", "coding"];

	for(let habit of goodHabits) {
  		console.log(habit);
	}
```

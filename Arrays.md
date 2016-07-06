<h4>What is new?</h4>

ES6 introduced many Array methods.
<h5>Array.of()</h5>
The new Array() constructor behaves differently based on the type and number of arguments passed to it. <strong>Array.of() method</strong> creates a new array. It is introduced to deal with the quirk of creating an array using Array Constructor. 
```javascript
	let set1 = new Array(2),
    set2 = new Array("2"),
    set3 = new Array(1, 7),
    set4 = new Array(1, "5");
    
    console.log(set1.length);     //2
    console.log(set1[0]);         //undefined
    console.log(set2.length);     //1
    console.log(set2[0]);         //2
    console.log(set3.length);     //2
    console.log(set3[1]);         //7
    console.log(set4[1]);         //5
```
Array.of() example. This exactly the same with the example above but we get slightly different results:
```javascript
	let set1 = Array.of(2),
    set2 = Array.of("2"),
    set3 = Array.of(1, 7),
    set4 = Array.of(1, "5");
    
    console.log(set1.length);     //1
    console.log(set1[0]);         //2
    console.log(set2.length);     //1
    console.log(set2[0]);         //2
    console.log(set3.length);     //2
    console.log(set3[1]);         //7
    console.log(set4[1]);         //5
```
Understanding ECMAScript 6 Book:
> The Array.of() method does not use the Symbol.species property (discussed in Chapter 9) to determine the type of return value. Instead, it uses the current constructor (this inside the of() method) to determine the correct data type to return.

<h5>Array.from()</h5>
We used to use `Array.prototype.slice.call(arrayLikeObject)` to convert array-like objects into arrays. Array-like object is an object with numeric indices and and a length property. Since Array `slice()` method only needs numeric indices and a length property, it can work with array-like objects and would return an array.<br> 
Array.from() method takes iterable or an array-like object as a 1st argument and converts the argument into an array.
```javascript
	function ArrayFrom() {
		return Array.from(arguments);
		// args is an instance of an Array
	}
```
The `Array.from()` method also uses `this` to determine the type of array to return.

<strong>Array.from(arguments, mapping function)</strong>
We can pass 2nd optional parameter to Array.from() method. Or if the mapping function is on an object, then we can pass a 3rd argument which represents `this`.

```javascript
	function ArrayFrom() {
		return Array.from(arguments, (value) => value + 1);
		// 2nd argument to Array.from() method is a mapping function (arrow function)
	}

	let aFrom = ArrayFrom(1, 2, 3, 4, 5);
	console.log(aFrom);     //[2,3,4,5,6]
```

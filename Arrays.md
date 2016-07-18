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
Array.from(args, object method, object)
```javascript
	let obj = {
  		increase : 1,
  
  		add(value) {
    		return value + this.increase;
  		}
	};

	function ArrayFrom() {
		return Array.from(arguments, obj.add, obj);
	}

	let aFrom = ArrayFrom(1, 2, 3, 4, 5);
	console.log(aFrom);     //[2,3,4,5,6]
```

`Array.from()` method can turn any `Collection Object` method into an Array. Collection Objects (Array, Sets, Maps and Strings) are iterables. We can create our own iterable just adding `Symbol.iterator` method to it.

```javascript
	let numbers = {
  		*[Symbol.iterator]() {
    		yield 9;
    		yield 25;
    		yield 144;
  		}
	};

let sqrt = Array.from(numbers, (value) => Math.sqrt(value));

console.log(sqrt);		//[3, 5, 12]
```
Numbers Object has `Symbol.iterator` property.

"Understanding ECMAScript" Book:
> If an object is both array-like and iterable, then the iterator is used by Array.from() to determine the values to convert.

<h5>New Array Methods</h5>
<strong>find() and findIndex() methods:</strong><br>
Both find() and findIndex() accept two arguments: a callback function and an optional value to use for `this` inside the callback function. Methods return `true` if they find the element. They stop looking when the 1st matching element is found. Both methods are good to find an array element that matches the `condition` rather than a value. Use `indexOf()` or `lastIndexOf()` methods to a value.

```javascript
	let arr = [10, 20, 30, 40, 50];

	console.log(arr.find(n => n > 30));
```

<strong>Array.fill(): </strong> method fills 1 or more Array elements with a specific value. It takes at least 1 element to fill and 2 optional elements. `Array.fill(element, startFrom, end)`.

```javascript
	let arr = [10, 20, 30, 40, 50];

	//console.log(arr.fill(25));		//[25, 25, 25, 25, 25]
	//console.log(arr.fill(1, 2, 4));
	//console.log(arr.fill(5, 2));
	console.log(arr.fill(25, 0, 2));    //[25,25,30,40,50]
```
<strong>Array.copyWithin()</strong>: As name suggests it copies part or all elements of the array and pastes the elements inside the same array. `Array.copyWithin(startPastingFrom, startCopyingFrom)`
```javascript
	let arr = [10, 20, 30, 40, 50, 60];

	console.log(arr.copyWithin(3, 0));		//[10,20,30,10,20,30]
```
Start copying the `arr` Array elements starting from `arr[0]` and then paste those elements in the same array starting from `arr[3]`.
We can limit how many array elements would be overwritten: <br>
`Array.copyWithin(startPastingFrom, startCopyingFrom, numberOfElementsToPaste)`
```javascript
	let arr = [10, 20, 30, 40, 50, 60];

console.log(arr.copyWithin(3, 0, 2));		//[10,20,30,10,20,60]
```

<h5>Typed Arrays</h5>
They are there to work with Numeric Arrays. Typed Arrays are 1st created to use with WebGL as native Javascript numbers were too slow. ESMAScipt 6 introduced Typed Arrays. ECMAScript 6 version of the Typed Arrays are the evolution of the WebGL Typed Arrays.<br>
Javascript Numbers (Integers and Floats) are stored as a 64 bit floating point reprsentations of numbers. <br>
Typed arrays allow the storage and manipulation of eight different numeric types:

1. Signed 8-bit integer (int8)
2. Unsigned 8-bit integer (uint8)
3. Signed 16-bit integer (int16)
4. Unsigned 16-bit integer (uint16)
5. Signed 32-bit integer (int32)
6. Unsigned 32-bit integer (uint32)
7. 32-bit float (float32)
8. 64-bit float (float64)

In order to use Typed Arrays we need to create an array buffer to store the data.
<h6>Array Buffers</h6>
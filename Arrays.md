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
Typed Arrays are there to work with Numeric Arrays. Typed Arrays are 1st created to use with WebGL as native Javascript numbers were too slow. ESMAScipt 6 introduced Typed Arrays. ECMAScript 6 version of the Typed Arrays are the evolution of the WebGL Typed Arrays.<br>
Javascript Numbers (Integers and Floats) are stored as a 64 bit floating point reprsentations of numbers. <br>
Typed arrays allow the <strong>storage</strong> and <strong>manipulation</strong> of eight different numeric types:

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
It is a memory location with specific size (in bytes). We can change the Array Buffer content but we cannot change the Array Buffer size. We can create/allocate a memory using `ArrayBuffer()` constructor function. We can use `byteLength` property to retrieve the allocated memory size. We can use `slice()` Array method to create a new Array Buffer based on existing Array Buffer:

```javascript
	let buffer = new ArrayBuffer(20);
	console.log(buffer.byteLength);         //20

	let newBuffer = buffer.slice(10, 15);
	console.log(newBuffer.byteLength);      //5
```
To write Array Buffers we need to create a view.

<h5>Array Buffer Views</h5>
`Views` are interfaces to Array Buffers. Views enable us to work on array buffers (read and write) in one of the numeric data types. We use `DataView()` constructor to create an Array Buffer View. `DataView(arrayBuffer, startFrom, numberOfBytes)`<br>
We can create several views over the same array buffer, which can be useful if you want to use a single memory location for an entire application rather than dynamically allocating space as needed.
```javascript
	let arrBuffer = new ArrayBuffer(20);
	console.log(arrBuffer.byteLength);         //20

//view operates on entire allocated memory
	let view = new DataView(arrBuffer);
	console.log(view.byteLength);           //20

//view operates on only part of the allocated memory
	let view2 = new DataView(arrBuffer, 10, 5);
	console.log(view2.byteLength);           //5
```
Read-only Array Buffer View properties:
- buffer : The array buffer that the view is tied to
- byteOffset : The second argument to the DataView constructor, if provided (0 by default)
- byteLength : The third argument to the DataView constructor, if provided (the buffer’s byteLength by default)

```javascript
	let arrBuffer = new ArrayBuffer(20);
	let view1 = new DataView(arrBuffer);
	let view2 = new DataView(arrBuffer, 10, 5);

	console.log(view1.buffer === arrBuffer);    //true
	console.log(view2.buffer === arrBuffer);    //true
	console.log(view1.byteLength);              //20
	console.log(view2.byteLength);              //5
	console.log(view1.byteOffset);              //0
	console.log(view2.byteOffset);              //10
```

<h5>Reading and Writing on Allocated Memory</h5>

DataView prototype has a method for each numeric data type to write to Array Buffer and a method to read from an Array Buffer. Here are the methods:<br>
- getInt8(byteOffset, littleEndian)
- setInt8(byteOffset, value, littleEndian)
- getUint8(byteOffset, littleEndian)
- setUint8(byteOffset, value, littleEndian)
- getFloat32(byteOffset, littleEndian)
- setFloat32(byteOffset, value, littleEndian)
- getFloat64(byteOffset, littleEndian)
- setFloat64(byteOffset, value, littleEndian)

little Endian means the least significant byte is at byte 0<br>
```javascript
	let arrBuffer = new ArrayBuffer(3);
	let view = new DataView(arrBuffer);

	view.setInt8(0, 10);
	view.setInt16(1, 5);

	console.log(view.getInt8(0));		//10
	console.log(view.getInt16(1));		//5
	console.log(view.getInt16(0));		//2560
```
So far we only used DataView() constructor as an Array Buffer View. But we can also use Typed Array constructors. They are type-specific views for array buffers. Here is the list of Typed array constructors:
- Int8Array
- Uint8Array
- Uint8ClampedArray
- Int16Array
- Uint16Array
- Int32Array
- Uint32Array
- Float32Array
- Float64Array

Typed Array constructors have specific element sizes (between 1 and 4). Here is how we find out element sizes:

```javascript
	console.log(UInt8Array.BYTES_PER_ELEMENT);      // 1
	console.log(UInt16Array.BYTES_PER_ELEMENT);     // 2

	let ints = new Int8Array(5);
	console.log(ints.BYTES_PER_ELEMENT);            // 1
```
<h5>Creating type-specific Array Buffer Views</h5>
1 - We can create a View using the same approach like `DataView`.
```javascript
	let arrBuffer = new ArrayBuffer(20);
	let view1 = new Int8Array(arrBuffer);
	let view2 = new Int8Array(arrBuffer, 10, 5);

	console.log(view1.buffer === arrBuffer);    //true
	console.log(view2.buffer === arrBuffer);    //true
	console.log(view1.byteLength);              //20
	console.log(view2.byteLength);              //5
	console.log(view1.byteOffset);              //0
	console.log(view2.byteOffset);              //10
```

2 - Pass a Single element to a constructor to create a Typed Array. This arguments represents a number of elements not the byte size.

```javascript
	let integers = new Int8Array(5);
	let floats = new Float32Array(2);

	console.log(integers.length);       //5
	console.log(integers.byteLength);   //5

	console.log(floats.length);         //2
	console.log(floats.byteLength);     //8
```
3 - Pass an object to create a Typed Array. Those objects as an argument are below:
- An Array
- An Array-like Object
- An Iterable
- A Typed Array
```javascript
	let integers1 = new Int8Array([2, 20]);
	let integers2 = new Int32Array(integers1);

	console.log(integers1.length);                          //2
	console.log(integers1.byteLength);                      //2
	console.log(integers1.bufffer === integers2.buffer);    //false
	console.log(integers1[1]);                              //20
	
	integers1[0] = 25;
	integers1[1] = 35;

	console.log(integers1[1]);                              //35
```

<h5>Typed Array Methods</h5>
Typed arrays also include a large number of methods that are functionally equivalent to regular array methods. We can use the following array methods on typed arrays:

- copyWithin()
- entries()
- fill()
- filter()
- find()
- findIndex()
- forEach()
- indexOf()
- join()
- keys()
- lastIndexOf()
- map()
- reduce()
- reduceRight()
- reverse()
- slice()
- some()
- sort()
- values()

1 thing that we need to give an attention:
```javascript
	let ints = new Int16Array([25, 50]),
//map() returns an instance of a typed Array not an Array instance
    mapped = ints.map(v => v * 2);

	console.log(mapped instanceof Int16Array);  // true
	console.log(mapped instanceof Array);       //false
```

Typed Arrays have the same methods as regular arrays:
- entries() method
- keys() method
- values() method

We can also use `spread operator` and `for - of` loop with Typed Arrays.

<h5>of() and from() Typed Array Methods</h5>
Arrays have Array.from() and Array.of() methods. Typed Arrays have of() and from() methods and they return typed arrays.
```javascript
	let integers = Int8Array.of(5, 10, 15);
	let floats = Float32Array.from([1.5, 2.5]);

	console.log(floats instanceof Array);           //false
	console.log(floats instanceof Float32Array);    //true
	console.log(integers[0]);                          //5
	console.log(floats[1]);                            //2.5

/*
   we cannot do this. typed array size reamins the same. 
   assigning a value to a non-existing typed array indices will be ignored
*/
	integers[3] = 10;
	console.log(integers[3]);
	console.log(integers.length);                     //3
```

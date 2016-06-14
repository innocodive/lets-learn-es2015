<h4>Sets and Maps</h4>

Set is a list of values. We use it to check if the value is there. Set is not a replacement for an Array.
Map is a collection of keys. Maps are used as a cache, a quick access to values. ES5 didn't have default `Sets & Maps`. So we used to use Object properties as Sets & Maps. Same object could be as a Set or as a Map. The only difference is Map requires a value to be stored.

Sets example:
```javascript
	let person = Object.create(null);
	// all 3 works
	person.me = " ";
	person.me = true;
	person.me = "Max";

	//MAPS EXAMPLE : store the property value to retrieve
	let data = person.me;
	console.log(data);

	// SETS EXAMPLE: check if the property exists
	if(person.me) {
  		console.log(person.me);
	} else {
  		console.log("it is not initiated yet");
	}
```
Problem with ES5 way of doing `Sets`:
```javascript
	let person = Object.create(null);
		let me = {},
    		you = {};

	person[2] = "Max";
	person["2"] = "Charyyev";

	console.log(person);	//{"2":"Charyyev"}

	person[me] = "Michael";
	person[you] = "John";

	console.log(person);	//{"2":"Charyyev","[object Object]":"John"}
```
`person[2]` == `person["2"]`. These 2 are basically, equal. Object property name will automatically be translated to String. <br>
Another problem: If we use empty an object as an Object property key, empty object will also be translated to String, which it will become [object Object]. Any empty object, no matter what is it assigned to will be [object Object] when they are translated to Strings. As we see from the example, second assignment override the 1st ones.

We should use `in` operator to check if a certain property exists on an object. We should also make sure the object doesn't inherit from other object, as `in` searches the object prototype as well.

<h5>Set type</h5>
ES6 introduces a `Set type` which is an ordered set of values, no duplicates. 
Have a look at the example below:
```javascript
	let collection = new Set();

	collection.add("25");
	collection.add(25);
		// `SET` internally uses Object.is() to compare values. set wouldn't do type conversion
	console.log(collection);		//["25",25]
	console.log(collection.size);	//2

	// we check if the set is an array. it is not.
	// convert `collection` to an array. we get a new empty array
	console.log(Array.isArray(collection));		//false
	console.log(Array.prototype.slice.call(collection));	//[]

	//this is how we convert Set into an Array. We use `spread operator`
	let arr = [...collection];
	console.log(arr);
	console.log(Array.isArray(arr));
```

Situation is the same with when using empty objects as a set keys. The most important thing we need to keep in mind that `Set` uses `Object.is()` to compare values before it stores them.

```javascript
	let obj1 = {},
    obj2 = {};
    
    console.log(Object.is(obj1, obj2)); //false
    
	let collection = new Set();

	collection.add(obj1);
	collection.add(obj2);
	collection.add(obj1); //this will be ignored. no dublicates

	console.log(collection);  //[{}, {}]
	console.log(collection.size); //2

	/*
  		1) no dublicates
  		2) ordered list
  		Here, the Set constructor argument is an Array.
  		The Set constructor actually accepts any iterable object as an argument
	*/
	let arr = [1, 2, 3, 4, 1, 3, 2, 6, 5, 2, 4, 7, 3];
	let collection2 = new Set(arr);
	console.log(collection2);			//[1,2,3,4,6,5,7]

	// Check the Set if the value exists using `has()` method

	console.log(collection2.has(3));  //true
	console.log(collection.has(3));   //false

	// to delete items from a set
	collection2.delete(5);
	console.log(collection2);   //[1,2,3,4,6,7]

	// empty the entire set
	collection2.clear();
	console.log(collection2);   //[]
```

<h5>Iterating Sets</h5>

Sets has a `forEach()` method to iterate over each element in the Set. It works in a same way like in Arrays and Maps. `set.forEach()` has 3 parameters. `set.forEach(function(key, value, set) {})`. key and value are always the same value. This is designed this way to keep method consistent with the same method on Arrays and Maps.

```javascript
	let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
	let collection = new Set(arr);

	collection.forEach(function(key, value, set) {
  		if(key%5 === 0) {
    		console.log(key + " = " + value);		// 5 = 5, 10 = 10
  		}
	});
```
We can also `this` as a 2nd argument to `forEach()` callback function. We can also use `arrow functions` to achieve the same thing without using `this`. 

```javascript
	let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
	let collection = new Set(arr);

	let operations = {
  		print(number) {
    		console.log(number);
  		},
  		calculate(set) {
    		set.forEach(function(number) {
      			if(number % 5 === 0) {
        		this.print(number);
      			}
    		}, this);
  		}
	};

operations.calculate(collection); // 5, 10
```

We can use `[...value]` spread operator and `Sets` to eliminate duplicate values:

```javascript
	function zeroDuplicates(array) {
  		return [...new Set(array)];
	}

	var array = [1, 2, 3, 7, 4, 1, 3, 2, 6, 3];

	console.log(zeroDuplicates(array));
```

create an object and save it in a set. modify the object. then get the initial object value back from the set:

```javascript
	let obj = {},
    set = new Set();
    
    set.add(obj);
    console.log(set);   //[{}]
    console.log(obj);   //{}
    
    obj = null;
    console.log(set);   //[{}]
    console.log(obj);   //null
    
    obj = [...set][0];
    console.log(obj);   //{}
```
Important note is as long as a reference exists to the `obj` object, it cannot be garbage colected. We can call this <strong>Strong Set</strong> . This might eventually lead to a `Memory Leak`. To prevent this ES6 also offers <strong>Weak Sets</strong>. If weak reference to the object in the set is the only reference, then the object will be garbage collected.<br>

We can create Weak sets using `WeakSet()` constructor. It has `add(), has() and delete()` methods.

```javascript
	let set = new WeakSet();

	let arr = [1, 2, 3, 4, 5],
    	obj = {one : 1, two : 2},
    	name = "Max";

	set.add(obj);
	set.add(arr);
	set.add(name);		// Invalid value used in weak set

	console.log(set.has(obj));
	console.log(set.has(arr));
	console.log(set);

	obj = null;		//removes the weak set reference. this object will be garbage collected
```

We can cannot verify/check if the object reference is removed. has() method wouldn't work. Javascript Engine removes the object reference to a weak set. Weak sets are only good if the only thing we want is track the object reference. A couple more points about the weak set: <br>
- We cannot programmatically determine the weak set content
- We cannot store a primitive value in the weak set. All 3 weak set methods will throw an error, if we pass a primitive value to them
- Weak sets have no `size` operator and no forEach() method. They are basically, non-iterable.

<h5>Maps</h5>

Maps are an ordered list of key-value pairs. Map keys and values can have any type unlike object properties where property key values are converted to a String.

```javascript
	let map = new Map(),
  		obj1 = {name : "Max", surname : "Charyyev"},
  		obj2 = {};

		map.set(obj1 , "John");
		map.set(obj2, [1, 2, 3, 4, 5]);
		map.set("title", "web developer");
		map.set("age", 25);

		console.log(map.get(obj1));		//John
		console.log(map.get(obj2));		//[1, 2, 3, 4, 5]
		console.log(obj1);				//{name : "Max", surname : "Charyyev"}
		console.log(obj2);				//{}
		console.log(map.get("age"));	//25

		console.log(map.has(obj1));   //true
		map.delete(obj1);
		console.log(map.get(obj1)); //undefined
		console.log(map.size);      //3
		console.log(map.has(obj1)); //false
		map.clear();

		console.log(map.size);    //0
```
Objects and arrays can be used as keys and values and they wouldn't be converted to different types. Content of the original objects are not modified. 
Maps has 3 methods <strong>(has(key), delete(key) and clear())</strong>. Maps has `size` property as well. 
<br>
<strong>How to initialize a Map?</strong><br>
We pass an array with key-value pairs array. Confusing??? Have a look at the example below:

```javascript
	let map = new Map([["name", "Max"], ["arr1", [1, 2, 3]], ["arr2", ["one", "two"]]]);

	console.log(map);		//[["name","Max"],["arr1",[1,2,3]],["arr2",["one","two"]]]

	console.log(map.has("name"));		//true
	console.log(map.get("arr1"));		//[1,2,3]
	console.log(map.size);				//3
```

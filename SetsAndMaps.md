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


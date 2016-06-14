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


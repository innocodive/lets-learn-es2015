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

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

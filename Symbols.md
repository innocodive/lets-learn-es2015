<h4>New Primitive Type : Symbols</h4>

A symbol is a primitive type intruced in ES6. It enables developers to add non-string property names to objects. Symbols are created using a global Symbol() Function. A Symbol() Function takes an optional description argument. A description argument value is stored in the Internal Property [[Description]]. This property's value is read anytime Symbol's toString() method is called.

```javascript
	let habit = Symbol();
	let sport = Symbol("My favourite sport");

	let object = {};

	object[habit] = "eating healthy food";
	object[sport] = "bodybuilding";

	console.log(object[habit]);						// eating healthy food"
	console.log(object[sport]);						// bodybuilding

	console.log("My favourite sport" in object);	//false
	console.log(sport);								//Symbol("My favourite sport")
	console.log(typeof sport);						// symbol
```

We can use symbols with `bracket notation` and wherever we use object literal's `computed property name` is used:

```javascript
	let habit = Symbol();

	let object = {
  		[habit] : "eating healthy food",
  		name : "Max",
  		surname : "Charyyev"
	};

	let sport = Symbol("my favourite sport");

	Object.defineProperties(object, {
  		[sport] : {
    	value : "bodybuilding",
    	writable : false
  		}
	});

	console.log(object);			// {"name":"Max","surname":"Charyyev"}
	console.log(object[habit]);		// eating healthy food
	console.log(object[sport]);		// bodybuilding
```
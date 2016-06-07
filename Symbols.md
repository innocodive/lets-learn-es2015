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

If we wanted to share Symbols (use accross many files or many objects share the same symbol property) we need to use `Symbol.for("description")` structure instead of `Symbol()`.
```javascript
	let accNo = Symbol.for("accountNumber");

	let object = {};
	object[accNo] = 27458390;

	console.log(object[accNo]);
	console.log(accNo);
```

If we don't use a description parameter for for `Symbol.for()`, `console.log(accNo)` will give us `Symbol(undefined)` result.

```javascript
	let accNo = Symbol.for("accountNumber"),
    accNo2 = Symbol.for("accountNumber"),
    accNo3 = Symbol("accountNumber");

	let object = {
  		[accNo] : 27458390,
  		[accNo2] : 22776655
	};

	console.log(object[accNo]);
	console.log(accNo);

	console.log(accNo === accNo2);
	console.log(object[accNo2]);
	console.log(accNo2);

	console.log(Symbol.keyFor(accNo));
	console.log(Symbol.keyFor(accNo2));
	console.log(Symbol.keyFor(accNo3));
```

Symbol.for() method searches for the symbol with <i>accountNumber</i> key in the `global symbol registry`. if finds, it returns that symbol. If not, then creates a new symbol with <i>accountNumber</i> key and returns the new symbol. In the example above both accNo and accNo2 print the same number as they are the same symbol.
<p>`console.log(Symbol.keyFor(accNo3));` prints <i>undefined</i> as accNo3 is not in the `global symbol registry`</p>

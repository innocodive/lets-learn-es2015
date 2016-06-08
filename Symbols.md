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
<p>We can retrieve shared symbols using `Symbol.keyFor()` from global symbol registry. It looks for the shared symbol based on symbol keys.</p>
<p>Global Symbol Registry is basically a global shared environment. To prevent the naming collision with the 3rd party libraries, it is good to namespace our symbol keys.</p>

```javascript
	let accNo = Symbol.for("accountNumber");

	let aNumber = accNo / 5;  		//Cannot convert a Symbol value to a number
	let aNumber1 = accNo + "abc"; 	//Cannot convert a Symbol value to a string
	let aNumber2 = String(accNo) + "abc";
	let aNumber3 = accNo.toString() + "abc";

	console.log(aNumber);
```
Symbol coercion wouldn't happen automatically. You have to use `String()` or `toString()` to convert the symbol values to string.

<h5>How to Retrieve Symbol Properties?</h5>

```javascript
	let accNo = Symbol.for("accountNumber"),
    sCode = Symbol.for("sortCode");

	let obj = {
  		[accNo] : 12234567,
  		[sCode] : "40-87-73",
  		name : "Max",
  		surname : "Charyyev"
	};

	let sym = Object.getOwnPropertyNames(obj);
	let keys = Object.keys(obj);
	console.log(sym);             //["name","surname"]
	console.log(keys);            //["name","surname"]

	let symbols = Object.getOwnPropertySymbols(obj);
	console.log(symbols.length);  //2
	console.log(obj[symbols[0]]); //12234567
```

Object.keys() returns all enumerable property names. <br>
Object.getOwnPropertyNames() returns all property names on a given object. <br>
none of them would return symbol properties. We use `Object.getOwnPropertySymbols()` to return all symbol properties on a given object. Returned result would be an array of own property symbols.

<h5>Wel-known Symbols</h5>
Well-known symbols represent common behaviors in JavaScript that used to be internal-only operations before.

- Symbol.hasInstance
- Symbol.isConcatSpreadable
- Symbol.iterator
- Symbol.match
- Symbol.replace
- Symbol.search
- Symbol.species
- Symbol.split
- Symbol.toPrimitive
- Symbol.toStringTag
- Symbol.unscopables

> Overwriting a method defined with a well-known symbol changes an ordinary object to an exotic object because this changes some internal default behavior. There is no practical impact to your code as a result, it just changes the way the specification describes the object.
~ Understanding ECMAScript 6 by Nicolas Zakas

<h5>Symbol.hasInstance</h5>
Every Function has `Symbol.hasInstance` method. This method is defined on the `Function.prototype`. `Symbol.hasInstance` property is <i>nonwritable</i> and <i>nonconfigurable</i> and <i>nonenumerable</i>. Symbol.hasInstance accepts 1 argument. Returns true if the argument is an instance of the function.


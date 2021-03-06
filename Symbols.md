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

<p>Prior to ES6, internal workings of above methods were hidden from us. Symbols give us an access to the native behaviours of those methods. Those symbols above represent methods on Functions, Regular Expressions and so on. These symbol methods are defined on prototypes of Functions and Regular Expressions.</p>

> Overwriting a method defined with a well-known symbol changes an ordinary object to an exotic object because this changes some internal default behavior. There is no practical impact to your code as a result, it just changes the way the specification describes the object.
~ Understanding ECMAScript 6 by Nicolas Zakas

<h5>Symbol.hasInstance</h5>
Every Function has `Symbol.hasInstance` method. This method is defined on the `Function.prototype`. `Symbol.hasInstance` property is <i>nonwritable</i> and <i>nonconfigurable</i> and <i>nonenumerable</i>. Symbol.hasInstance accepts 1 argument. Returns true if the argument is an instance of the function.

```javascript
	// 2 statements below are the same

	object instanceof Array;
	Array[Symbol.hasInstance](object);
```

As we remember Symbol.hasInstance accepts 1 argument and returns true or false. We can redefine what value `Symbol.hasInstance`should return by changing its return value.

```javascript
	function Bodybuilding() {}

	Object.defineProperty(Bodybuilding, Symbol.hasInstance, {
		value : function(val) {
			return false;
		}
		});

		let bb = new Bodybuilding();

		console.log(bb instanceof Bodybuilding);	// supposed to be false, it prints true. why???
``` 

<h5>Symbol.isConcatSpreadable</h5>

`Symbol.isConcatSpreadable` property has a boolean value. it checks if an object has `length` property and `numeric keys`. Numeric keys should be added individually to the concat result.
```javascript
	let habits = {
  		0 : "eat healthy",
  		1 : "exercize every day",
  		2 : "spend time with family",
  		length : 3,
  		[Symbol.isConcatSpreadable] : true
	};

	let allHabits = ["get enough sleep"].concat(habits);

	console.log(allHabits.length + " : " + allHabits);
```
We can set `Symbol.isConcatSpreadable` to false to prevent concatination.

<h5>Symbol.match, Symbol.replace, Symbol.search, Symbol.split</h5>

We can define these methods on an object which will be able to implement pattern matching, no regular expressions.

```javascript
	let habits = {
  		[Symbol.match] : function(value) {},
  		[Symbol.replace] : function(value, replace) {},
  		[Symbol.search] : function(value) {},
  		[Symbol.split] : function(value) {}
	};

	let string1 = "Bodybuilding",
    	string2 = "eat healthy";
    
	let match = string1.search(habits),
    	replace = string2.replace(habits),
    	search = string1.search(habits),
    	split = string2.split(habits);
```
<h5>Symbol.toPrimitive method</h5>

Whenever we use `==` operator, Javascript converts objects to primitive values before they were compared. Which primitive value they should be converted was hidden from us. ES6 makes this changeable through using Symbol.toPrimitive method. <br>
Symbol.toPrimitive method is defined on each standard type. When a conversion to a primitive is needed, `Symbol.toPrimitive(hint)` is called with only 1 argument.

- hint = number : returns number
- hint = string : returns string
- hint = default : no preference

Standard objects treat `default` mode as a number mode. Date treats default as a string. Using Symbol.toPrimitive() method we can override these dafault coercion behaviour.

<strong>Behaviour of the `Number Mode`</strong>
- Call valueOf() method : basically, return the PRIMITIVE value of the String object
- Call toString() method : basically, convert Number to String and return the PRIMITIVE value
- None of the above worked? Throw an error

<strong>Behaviour of the `String Mode`</strong>
- Call toString() method : basically, convert Number to String and return the PRIMITIVE value
- Call valueOf() method : basically, return the PRIMITIVE value of the String object
- None of the above worked? Throw an error

We can override default conversion behaviours:

```javascript
	function convertPoundtoKg(weight, unit) {
  		this.weight = weight;
  		this.unit = unit;
	}

	Function.prototype[Symbol.toPrimitive] = function(hint) {
  		switch(hint) {
    		case "string":
      			//code
    		case "number":
      			//code
    		case "default":
      			//code
  		}
	};

	let convert = new convertPoundtoKg(70, "kg");

	console.log(convert);
```

How you call the function will decide which mode gets triggered.

<h5>Problem with identifying objects</h5>

It is not easy to identify what type of objects we are dealing with in browsers when we pass objects between multiple environments like passing `Arrays`. Developers used to rely on `Object.prototype.toString.call(objectValue) to identify object type. JS Libraries included functions like that:

```javascript
	function isArray(value) {
    	return Object.prototype.toString.call(value) === "[object Array]";
	}
```
This is how we identify if the JSON is native or not:

```javascript
	function supportsNativeJSON() {
    	return typeof JSON !== "undefined" &&
        	Object.prototype.toString.call(JSON) === "[object JSON]";
	}
```

ES 6 introduced a well-known Symbol `Symbol.toStringTag`. It defines which value should be produced if `Object.prototype.toString.call(value)` is called on each object.

```javascript
	function Person(name) {
  		this.name = name;
	}

	let max = new Person("Max");
	console.log("1: " + max.toString());

	Person.prototype[Symbol.toStringTag] = "Array";
	console.log("2: " + max.toString());

	Person.prototype.toString = function() {
  		return this.name;
	};

	console.log("3: " + max.toString());
	console.log("4: " + Object.prototype.toString.call(max));
```

The result of calling `Object.prototype.toString` on the Person object is [object Array] which is what we would get calling on real Arrays. This shows that using `Object.prototype.toString` on objects is not a reliable way of identifying object's type anymore. <br>
Changing string tag of any native object is also possible:

```javascript
	let object = {
  		name : "Max",
  		surname : "Charyyev"
	};

	Object.prototype[Symbol.toStringTag] = "Fitness";

	console.log(Object.prototype.toString.call(object));
```
This may not be useful, but it is possible in ES6.
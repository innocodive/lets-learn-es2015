<h4>ES6 Objects</h4>

- <strong>Ordinary Objects</strong> : Objects with Default Internal Behaviours
- <strong>Exotic Objects</strong> : they have different internal behaviours than default objects
- <strong>Standard Objects</strong> could be either Ordinary or Exotic
- All Standard Objects are `built-in objects`
- Built-in Objects become present when a code starts the execution in browsers or Node.js

Object Literals in ES5 is basically name - value pairs. It is short and concise.

```javascript
	"use strict";

	function initialize(name, surname) {
  		var age = 25;
  		return {
    		name: name,
    		surname: surname,
    		age: age
  		};
	}

	console.log(initialize("Max", "Charyyev"));
```
If we look at this example, we can see duplication of function properties and returned object parameters. In ES6, we can make this example even shorter. If object property names are the same as the function parameter names, we can only use object property names. 
Learning ECMAScript 6 Book:
> When a property in an object literal only has a name, the JavaScript engine looks into the surrounding scope for a variable of the same name. If it finds one, that variable’s value is assigned to the same name on the object literal.

We can modify the above example for ES6:
```javascript
	function initialize(name, surname) {
  		var age = 25;
  		return {
    		name,
    		surname,
    		age
  		};
	}

	console.log(initialize("Max", "Charyyev"));
```

<h4>Concise Method Syntax</h4>

ES6 introduces some minor changes to how we define an object method. Have a look at the example below:

```javascript
	// ES5
	var me = {
		name : "Max Charyyev",
		printMyName : function() {
			console.log(this.name);
		}
	};

	// ES6
	var me = {
		name : "Max Charyyev",
		printMyName() {
			console.log(this.name);
		}
	};
	
	me.printMyName();
```
We don't have to specify a name for the method and we don't need a `function` keyword. The rest of the synatx is the same with ES5. `printMyName` method created on `me` object and an anonymous function is assigned to `printMyName` property.

<h4>Square Brackets in Objects</h4>

When we use bracket notation, we can set any string value as an Object property. Object property names with spaces makes it impossible to reference them later using dot notation.

```javascript
	var me = {}, you = {}, name = "your name";

	me["my name"] = "Max Charyyev";
	you[name] = "Alexander Gintsburg";

	console.log(me["my name"]);
	console.log(you[name]);

	var object = {
  		"another name" : "Mary Magpie",
	};

	console.log(object["another name"]);
```
In this example, `name` in `you[name]` is a <strong>computed property name</strong>. In ES6, computed property names are part of the Object Literal Syntax. So, the example below works in ES6:

```javascript
	var name = "your name";

	var object = {
  		"another name" : "Mary Magpie",
  		[name] : "Mathew Hall"
	};

	console.log(object["another name"]);
	console.log(object[name]);
```

<h4>New Global Object Methods</h4>

<strong>Object.is()</strong> method to compare the values.
<strong>Object.assign()</strong> method is introduced to perform object composition in Javascript, to enhance mixin() approach.

Object.is() method works exactly the same with `===`. There are only acouple of cases it will give you more accurate result than `===` operator. 
- +0 and -0 are are not the same in Javascript Engine, but `===` operator will return true when they are compared.
- NaN === NaN will return false, but Object.is() method will return true.

```javascript
	console.log(NaN == NaN);    //false
	console.log(NaN === NaN);   //false

	console.log(+0 == -0);      //true
	console.log(+0 === -0);     //true

	console.log(Object.is(NaN, NaN));   //true
	console.log(Object.is(+0, -0));     //false
```

Object.assign() method:
Mixins are used to shallow copy one object's properties and methods without inheriting from that object.

```javascript
	function mixin(receiver, supplier) {
    Object.keys(supplier).forEach(function(key) {
        receiver[key] = supplier[key];
    });

    return receiver;
}

	var object1 = {
		name : "Max",
		surname : "Charyyev",
		getName : function() {
			return this.name + " - " + this.surname;
		}
	};

	object2 = {};

	mixin(object2, object1);

	object2.getName();
```
Because, this pattern is used often, ES6 added Object.assign() method. It accepts many suppliers. Latest supplier may overwrite the earlier suppliers properties on the receiver.
`Object.assign(receiver, supplier1, supplier2);`


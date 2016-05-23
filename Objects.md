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

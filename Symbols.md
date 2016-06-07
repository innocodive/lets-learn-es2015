<h4>New Primitive Type : Symbols</h4>

A symbol is a primitive type intruced in ES6. It enables developers to add non-string property names to objects. Symbols are created using a global Symbol() Function. A Symbol() Function takes an optional description argument. A description argument value is stored in the Internal Property [[Description]]. This property's value is read anytime Symbol's toString() method is called.

```javascript
	let habit = Symbol();
	let sport = Symbol("My favourite sport");

	let object = {};

	object[habit] = "eating healthy food";
	object[sport] = "bodybuilding";

	console.log(object[habit]);
	console.log(object[sport]);

	console.log("My favourite sport" in object);
	console.log(sport);
	console.log(typeof sport);
```

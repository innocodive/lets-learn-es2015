<h4>Classes</h4>

ES6 introduced a `class` syntax. It is basically a syntactic sugar on the `custom type decleration`. 
```javascript
	class Developers {
  
  	constructor(developer) {
    	this.developer = developer;
  	}
  
  	getDeveloper() {
    	console.log(this.developer);
  	}
}

	let dev = new Developers("javascript programmer");
	dev.getDeveloper();                       //javascript programmer

	console.log(typeof Developers);           //function
	console.log(typeof dev);				  //object
	console.log(dev instanceof Developers);   //true
	console.log(dev instanceof Object);       //true
	console.log(Developers.name);			  //Developers
```

Notes on Javascript Classes:
- Type of `Developers class` is still a Function. <i>Developers</i> class creates a function which behaves like a <i>constructor</i> method. 
- `constructor` is defined inside the class.
- Class declarations are `NOT HOISTED` but function declarations are hoisted
- Class Expressions are NOT HOISTED either
- Code inside the Class declaration run in `Strict Mode` automatically. No other choice.
- All methods inside a Class Declaration are non-enumerable.
- Class Methods don't have internal [[Construct]] method. We cannot use `new` keyword on class methods.
- Class constructor must be called using `new` keyword.

Example above could be written as a Class Definition:
```javascript
	let Developers = class {
  
  	constructor(developer) {
    	this.developer = developer;
  	}
  
  	getDeveloper() {
    	console.log(this.developer);
  	}
}

	let dev = new Developers("javascript programmer");
	dev.getDeveloper();                       //javascript programmer

	console.log(typeof Developers);           //function
	console.log(typeof dev);				  //object
	console.log(dev instanceof Developers);   //true
	console.log(dev instanceof Object);       //true
	console.log(Developers.name);			  //Developers
```
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
- Class names are declared as `const` internally. That is why class names cannot be changed inside class but can be changed after the class declarations and class expressions, meaning outside the class.
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
<strong>Named Class Expressions</strong><br>
Named Class Expressions only exist inside the Class. Outside the class, it would be undefined.
```javascript
	let Developers = class Developers2{
  
  	constructor(devs) {
    	this.developer = devs;
  	}
  
  	getDeveloper() {
    	console.log(this.developer);
  	}
}

	let dev = new Developers2("javascript programmer"); //Developers2 is not defined
	console.log(typeof Developers2);           //undefined
```

<h5>Classes are 1st class citizens</h5>

Anything that can be used as a value is a 1st class citizen in Javascript. <br>
Classes can be passed into Functions as arguments.
```javascript
	function returnObject(classObj) {
  		return new classObj();
	}

	let Developers = class {

    	getDeveloper() {
        	console.log("Javascript Programmer");
    	}
	}

	let dev = returnObject(Developers);
	console.log(dev.getDeveloper());		//Javascript Programmer
```
<strong>create Singletons using Classes</strong><br>
```javascript
	let dev = new class {
  		constructor(developer) {
    		this.developer = developer;
  		}
  
  		getDeveloper() {
    		console.log(this.developer);
  		}
	}("Javascript Programmer");

	console.log(dev.getDeveloper());
```
<strong>Computed Names in Classes</strong><br>

Class methods and Accessor properties can have `Computed Names`.
```javascript
	let methodName = "getDeveloper";
	let dev = new class {
  		constructor(developer) {
    		this.developer = developer;
  		}
  
  	[methodName]() {
    	console.log(this.developer);
  	}
}("Javascript Programmer");

	console.log(dev.getDeveloper());
```
<h5>Class Generator Methods</h5>

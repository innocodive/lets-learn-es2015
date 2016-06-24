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
	console.log(dev instanceof Developers);   //true
	console.log(dev instanceof Object);       //true
```

Notes on Javascript Classes:
- Type of `Developers class` is still a Function. <i>Developers</i> class creates a function which behaves like a <i>constructor</i> method. 
- `constructor` is defined inside the class.
- Class declarations are `NOT HOISTED` but function declarations are hoisted
- Code inside the Class declaration run in `Strict Mode` automatically. No other choice.
- All methods inside a Class Declaration are non-enumerable.

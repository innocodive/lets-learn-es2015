<h3>Functions</h3>

<h4>Setting Default Values for a function Parameter</h4>

Setting Default Function parameter values didn't exist in pre-ES6 Javascript. We used to simulate that feature using Logixal OR (||) operator.

```javascript
	function myProfile(name, age) {
		name = name || "Max";
		age = age || 25;

		console.log(name + "\n" + age);
	}

	myProfile("Maxim", 35);
``` 
Here is how we will achieve this in ES6:

```javascript
	function myProfile(name, surname, age = 25, height = 183, weight) {
  		console.log("Name: " + name + "\nSurname: " + surname + "\nAge: " + age);
  		console.log("Height: " + height + "cm\nWeight: " + weight + "kg");
	}

	myProfile("Max", "Charyyev", undefined, undefined, 75);
```
myProfile() function expects `name, surname and weight` parameters always be passed. `age and height` parameters have default values. When we call we **myProfile() function** we can either pass different values for `age, height` or we can use `default` values. To do that we need to pass `undefined` to the **myProfile()** function. We cannot pass `null`. It is considered to be valid value.

1 more example on Setting a Default Value for a Function paramater:

```javascript
	function foo(x = 5, y = 10) {
    	console.log( x + y );
	}

	foo();
	foo( 10, 16 );
	foo( 0, 12 );

	foo( 8 );
	foo( 8, undefined );
	foo( 8, null );

	foo( undefined, 12 );
	foo( null, 12 );
```
This is how your Function `foo()` would look like in pre-ES6 Javascript:

```javascript
	function foo() {
    var x = arguments.length <= 0 || arguments[0] === undefined ? 5 : arguments[0];
    var y = arguments.length <= 1 || arguments[1] === undefined ? 10 : arguments[1];

    console.log(x + y);
}
```

<h4>`arguments` object behaviour</h4>

`arguments` object behaviour changes when there is a default parameter values exist. It behaves the same in both ES5 strict Mode and ES6. In ES5 nonstrict mode the changes in function default parameter values reflects in `arguments` object but ES5 strict mode and ES6 changes in function default parameter values doesn't effect the `arguments` object. Have a look  at this example:

```javascript
	function argsx(one = 10, two) {
  //console.log(one + "\n" + two);
  
  console.log(arguments[0] === undefined);  //true
  console.log(arguments[1] === two);        //true
  
  one = 35;
  two = 45;
  
  console.log(arguments[0] === one);        //false
  console.log(arguments[1] === two);        //false
}

argsx(undefined, 20);
```

<h4>Using Functions as a Default Function Parameter Value</h4>

Default Function Parameter Values could also be a function:

```javascript
	function getSurname() {
  		return "Charyyev";
	}

	function getInitials(name, surname = getSurname()) {
  		return name + " " + surname;
	}

	console.log(getInitials("Max"));
	console.log(getInitials("Max", "Mulkomanoff"));
```

<h4>Function Default Parameter Value TDZ</h4>

Function parameter initialization only happens when we <u>call</u> the function. If we try to access to any Function parameter which is not initialized yet, will throw an error. Situation here is exactly the same with what we experienced with `let` and `const`. Uninitialized function parameters are in TDZ, when they are called. Have a look at this example:

```javascript
	function getResult(num1 = num2, num2) {
  		return num1 + num2;
	}

	console.log(getResult(10, 20));			//30
	console.log(getResult(undefined, 10));	//num2 is not defined
```
When we call getResult() function with parameters `getResult(undefined, 10)`, will throw an error, because when `num1` is initialized `num2` is not initialized yet, it is in TDZ. 

From <strong>Understanding ECMAScript 6</strong> book:
> Function parameters have their own scope and their own TDZ that is separate from the function body scope. That means the default value of a parameter cannot access any variables declared inside the function body.

<h4>Spread or Rest Operator</h4>

ES6 introduced a new operator called <b>Spread/Rest Operator (...)</b>. Main reason behind the introduction of the `REST parameter` is to replace `arguments` array-like object. Have a look at the example below:

```javascript
	function foo(...args) {
    // `args` is already a real array

    console.log( ...args );
}
foo(1, 2, 3, 4, 5);		// 1, 2, 3, 4, 5
```
We used to do (ES5):
```javascript
	var args = Array.prototype.slice.call( arguments );		
	// arguments is an array-like object. turn `arguments` into a real array
	// and
	// pass all `args` as arguments to `foo(..)`
    foo.apply( null, args );
```

`Rest parameter` comes with 2 restrictions:<br/>
1) We can only have 1 rest parameter and it should be the last Function parameter.

```javascript
	function foo(...args, newargs) {
    // `args` is already a real array

    console.log( ...args );
}
foo(1, 2, 3, 4, 5);		// throws an error
```

2) rest parameter cannot be used in Object setter:

```javascript
	let ES6Object = {

    set param(...value) {}	//We will se a message "Setter must have exactly one formal parameter."
	};
```

<strong>Understanding ECMAScript 6</strong> book:
> This restriction exists because object literal setters are restricted to a single argument. Rest parameters are, by definition, an infinite number of arguments, so they’re not allowed in this context.

<h4>Use of `rest` and `default parameters` with Function Constructor</h4>

With ECMAScript 6, we can now add Default Parameters and Rest Parameters to the Function Constructor. We need to assign a value to the default parameter:

```javascript
	var multiply = new Function("num1", "num2 = num1", "return num1 * num2");

	console.log(multiply(10, 5)); //50
	console.log(multiply(10));    //100
```

This is how we use `rest parameter` in Function Constructor:

```javascript
	var multiply = new Function("...args", "return args.length");

	console.log(multiply(10, 5)); //2
	console.log(multiply(10));    //1
```

This prints the length of the `args` array. One thing that we need to give an attention in these 2 examples is that the <strong>arguments</strong> to the Function Constructor and the Function body are in <strong>String</strong> form.

<h4>spread operator examples</h4>

Have a look at this example:

```javascript
	function spread(a, b, c) {
    console.log(a, b, c);
	}

	spread( ...["a", "b", "c"] ); 
```
We pass an array to the **spread function**, and this array elements will be spread out to its individual values as Function parameters. This is how we would do it with ES5:
```javascript
	spread.apply(undefined, ["a", "b", "c"]);
```

<h4>ES6 name property</h4>

ES6 added name property to Functions. It makes debugging easy.

```javascript
	function one() {}
	var two = function() {};
	var three = function four() {};

	var object = {
		myAge : function() {}
	};

	console.log(one.name);					//one
	console.log(two.name);					//two
	console.log(three.name);				//four
	console.log(object.myAge.name);			//myAge

	console.log(one.bind().name);			//bound one

	console.log((new Function()).name);		//anonymous
```

<h4>Find out how a Function is called</h4>

ES6 introduced a new metaproperty called <strong>new.target</strong>.
Understanding ECMAScript 6 Book:
> A metaproperty is a property of a non-object that provides additional information related to its target (such as new).

<strong>Why do we need that metaproperty?</strong>
Functions have 2 internal methods: [[Call]] and [[Construct]] internal methods. What is inside the Function (Function body) gets executed by one of these internal methods. If [[Call]] method is called, it just executes the Function body. If [[Construct]] method is called, it creates a new object and sets `this` to the new target and executes the Function body.
Now, if we want to find out how our Function is called, the commonly used method is using `instanceof` operator. When we call a Function using `new` operator, Javascript Engine calls [[Construct]] internal function method to create a new object, set `this` to the calling object and execute the Function. If we don't use `new` operator when we call a Function, then [[Call]] method executes the Function body but we don't have a new target object. Have a look at the examples below:

```javascript
	function Calculate(num1, num2) {
  		if(this instanceof Calculate) {
    		this.num1 = num1;
    		this.num2 = num2;
  		}
	}

	var one = new Calculate(10, 5);

	console.log(one);		// {"num1":10,"num2":5}
```
Now, let's see what happens if we call the Callculate Function without using `new` operator.

```javascript
	function Calculate(num1, num2) {
  		if(this instanceof Calculate) {
    		this.num1 = num1;
    		this.num2 = num2;
  		} else {
    		throw new Error("You forgot to use new operator. [[Call]] method got called. Target object is missing.");
  		}
	}

	var one = new Calculate(10, 5);
	var two = Calculate(5, 10);
```

We get an error message here, because `this` is not set to the target object. It shows the global object.
Now, we can check if the `new` operator is used when the Function is called through type checking. Have a look at the example below:

```javascript
	function Calculate(num1, num2) {
  		if(typeof new.target === "function") {
    		this.num1 = num1;
    		this.num2 = num2;
  		}
	}

	var one = new Calculate(10, 5);

	console.log(one);
```
If we do `console.log(new.target)`, it will print Calculate Function. If we don't use `new` operator when we call a function, we will get an undefined return. Another example:

```javascript
	function Calculate(num1, num2) {
  		if(typeof new.target === "function") {
    		this.num1 = num1;
    		this.num2 = num2;
  		}
	}

	function DontCalculate(num1, num2){
  		Calculate.call(this, num1, num2);
	}

		var one = new Calculate(10, 5);
		var two = new DontCalculate(20, 30);

		console.log(one);
		console.log(two);
```

In this example `typeof new.target` for DontCalculate will be `undefined` and returned result will be an empty Object. It shows that using `Function.call(this)` approach also calls [[Call]] internal Function method, therefore new object is not created and new.target is undefined.

<h4>Block-Level Function in ES6</h4>

Function Declarations inside the block ({}) in ES5 strict mode would give an error.

```javascript
	 "use strict";

	if(true) {
		console.log(Calculate());
		console.log(Multiply());

		function Calculate() {
			console.log("function definition body");
		}

		let Multiply = function() {
			console.log("function expression body");
		}
	}
	console.log(typeof Calculate);
	console.log(typeof Multiply);
```
This example wouldn't give the expected result in ES5 Strict Mode but it works in ES6. Function Declarations inside the block is considered as `Block-Level Functions` in ES6. Function Declarations get hoisted to the top of the block. That is why we can access the Function Declarations before we even they were declared. On the other hand, Function expressions which are declared using `let` would not be hoisted to the top of the block. That is why calling it before it was declared will give an error. This can be very handy feature when you decide to use hoisting or not in the block.
Block-Level Functions are not accessible outside the block. In ES5 non-strict mode, `function Calculate() {}` would be hoisted to the Global Scope. That is why `typeof` would give us a `function`.

<h4>Arrow Functions</h4>
Difference between Arrow Functions (=>) and traditional JS Functions. Arrow Functions:

- They have NO `this, super, new.target, arguments`. 
- They don't have [[Construct]] internal Function method. can't be called using `new` opearator
- No `prototype` property
- No `arguments`. We can only use named Function parameters and Rest operators to access to Function arguments
- Function arguments cannot have dublicate names in either mode

Arrow Functions are useful in eliminating `this` bindings related errors and confusions. Arrow Functions make the code optimization easy for a Javascript Engine.

```javascript
| ES6 Arrow Functions 						| ES5 Function equivalent |
|-------------------------------------------|-------------------------|
|	var returnNumber = value => value; 		| "use strict";

	console.log(returnNumber(10));			| var returnNumber = function returnNumber(value) {
	console.log(returnNumber.name);			|  	return value;
											|  };
											|  console.log(returnNumber(10));
											|  console.log(returnNumber.name);
--------------------------------------------------------------------------|
```
```javascript
	function calculate() {
  		var num1 = 10;
  		var num2 = 20;
  		return num1 + num2;
	}

	var returnNumber = () => calculate();

	console.log(returnNumber());
```
```javascript
	var myName = () => "Max Charyyev";
```
```javascript
	var result = (num1 + num2) => {
		return num1 + num2;
	};
```
```javascript
	var result = () => ({
		name : "Max",
		surname : "Charyyev"
	});
```
```javascript
	// this is a typical IIFE. we define an anonymous function, call it immediately. create a scope.

	let me = function(age) {
  
  	return {
    	myInitials : function() {
      		return "Max Charyyev";
    	}
  	};
  
	}(25);

	console.log(me.myInitials());

	// this is how we can achieve the same IIFE using Arrow Functions

	let me = ((age) => {
  
  	return {
    	myInitials : function() {
      		return "Max Charyyev";
    	}
  	};
  
	})(25);

	console.log(me.myInitials());
```

After seeing these examples, one thing takes my attention and it is <strong>shorter syntax</strong>. It improves readibility of the code if our functions are short. But in lengthy, complicated code, out of habit we tend to look for `function` keyword to understand the scope and look for the `return` to see what gets returned. Basically, it doesn't improve the readibility in a complicated, long code.

More Arrow Functions examples:

```javascript
	var GitHubbers = [
  		{ name : "Mathias Bynens", pushes : 1451 },
  		{ name : "Jack Franklin", pushes : 1298 },
  		{ name : "Max Charyyev", pushes : 47 }
	];

// ES5
console.log(GitHubbers.map(function(pusher) {
  return pusher.pushes;
}));

// ES6. Arrow Functions
console.log(GitHubbers.map(pusher => pusher.pushes));
```
Short, simple and clean. 
Below, we use ES6 Arrow Functions and Javascript Array Filter method to find out and print Prime numbers:

```javascript
	var isPrimeNumber = num => {
  		if(num < 2) return false;
    		for (var i = 2; i < num; i++) {
        		if(num % i == 0)
            		return false;
    			}
    	return true;
	};

	var first10 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
	var primeNumbers = first10.filter(isPrimeNumber);
	console.log(primeNumbers);
```

<h4>Arrow Functions and `this` binding</h4>

Main reason `=>` is designed is to solve dynamic `this` binding issues in some code. Have a look at this example from a "Understanding ECMAScript 6" book:

```javascript
	var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", function(event) {
            this.doSomething(event.type);     // error
        	}, false);
    	},

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    	}
	};
```

This code wouldn't work the way we expect. `this.doSomething(event.type)` is showing `document` object as a target, so, `doSomething()` function is not on document object but it is on `PageHandler` object. That is why we will get an error. Generally, these type of dynamic <strong>this</strong> related issues are solved using `bind(this)` or `self = this` approaches. We could change the code above slightly:

```javascript
	var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", function(event) {
            this.doSomething(event.type);     // error
        }.bind(this), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

Doing `document.addEventListener().bind(this)`. bind(this) creates another function and binds its `this` to the current object which is `PageHandler`, not the target object, which is `document` object. We could use <strong>Arrow Functions</strong> to solve this issue.

```javascript
	var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click",
                event => this.doSomething(event.type), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

Arrow Function has no `this` binding. Nonarrow Function which contains an arrow function would be target for `this`. In this example `this` for an arrow function would be the same with containing `init` function. 
Now, what if we use have `this` binding where we didn't have to use `self = this` or `bind(this)` solution, but we decide to use `=>`. Have a look at the example below:

```javascript
	var obj = {
  		func1 : function() {
    		console.log("function 1");
    		this.func2();
  		},
  		func2 : function() {
    		console.log("function 2");
  		}
	};

	obj.func1();
```
Let's write the same example using `=>`:
```javascript
	var obj = {
  		func1 : () => {
    		console.log("function 1");
    		this.func2();
  		},
  		func2 : () => {
    		console.log("function 2");
  		}
	};

	obj.func1();	//function 1
					//Cannot read property 'func2' of undefined
```
Arrow Functions get `this` from their parents. In our example it is a global object. That is why, `this` in <i>this.func2()</i> shows the global object where there is no func2() function.

<h4>Accessing the `arguments` in a parent Function using Arrow Functions</h4>

Here is an example:
```javascript
	function argReturn() {
  		return () => arguments[0];
	}

	var retr = new argReturn([10, 20], 30);

	console.log(retr());
```

<strong>So, where to use Arrow Functions? (http://bit.ly/1Sc0kZt)</strong>

- Use `function` in the global scope and for `Object.prototype` properties.
- Use `class` for object constructors.
- Use `=>` everywhere else.

<h4>Arrow Function Resource:</h4>
- Real Life ES6 - Arrow Functions by Jack Franklin (http://bit.ly/1TVbe4e)
- Where should I use Arrow Functions in ES6 (http://bit.ly/27qW2mC)
- Use of Arrow Functions by 2ality: (http://bit.ly/1JUew4L)

<h4>Tail Call Optimization (TCO) in ES6  Strict Mode</h4>

What is a `Tail Call`? <br/>
Tail Call is a Function call as a last statement in a Function. Javascript Engine optimizes the Call Stack in ES6 Strict Mode if certain conditions are met:
- If a Tail Call is not a Closure
- If a Tail Call is the last task in the Function
- If a <strong>result</strong> of a Tail Call is returned

Tail Call Optimization happens behind the scene and it is primary use case is recursive functions.

```javascript
	function outer() {
		return inner();		// optimized
	}

	function outer() {
		var innerVar = inner();
		return innerVar; 	// not optimized
	}

	function outer() {
		var name = "Max Charyyev";

		var myName = () => name;

		return myName(); 	// not optimized because of a function being a Closure
	}
```

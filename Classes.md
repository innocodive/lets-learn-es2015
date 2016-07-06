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
Any method in a class can be a generator. We need to put `*` in front of a method name:
```javascript
	class fitClass {
  		*myGenerator() {
    		yield 1;
    		yield 2;
  		}
	}

	let myClass = new fitClass();
	myClass.myGenerator();
	let genr = myClass.myGenerator();
	console.log(genr.next());			//{"value":1,"done":false}
	console.log(genr.next());			//{"value":2,"done":false}
	console.log(genr.next());			//{"value":undefined,"done":true}
```

If we need to iterate over a collection of values in a Class, we should define/create a default iterator using a Generator method.

```javascript
	class Workout {
  		constructor() {
    		this.items = [];
  		}
  		*[Symbol.iterator]() {
    	/*
    		we could either use the line below or use the for...of Loop
    		yield *this.items.values();
    	*/
    	for(let item of this.items) {
           yield item;
        }
  	}
}

	let workout = new Workout();

	let exercises = ["dead lift", "squats", "heap thrust"];
        exercises.forEach(function(exercise) {
            workout.items.push(exercise);
    });
    
  for(let x of workout) {
    console.log(x);
  }
```

<h5>Static Members</h5>

If we want Class instances share Class methods we add methods to Class prototype, but if we want methods only on the Class, then we add methods only to Classes. To do that we need `Static Members`.
```javascript
	function Workout(muscle) {
  		this.muscle = muscle;
	}

	//static method
	Workout.train = function(muscle) {
  		return new Workout(muscle);
	}

	let workout = Workout.train("biceps");
	console.log(workout);           //{"muscle":"biceps"}

	//instance method
	Workout.prototype.exercise = function(muscle) {
  		console.log(this.muscle);
	}

	let exercise = new Workout("Squats");
	exercise.exercise();            //Squats

	console.log(exercise.train());  //exercise.train is not a function
```

`train()` method is directly added to the `Workout` function. Function Object instances wouldn't have this method. This is how we simulate the `Static Members` in ES5.<br>
ES6 classes simplified this process introducing a `static` keyword to use just before the static method. Any method in a class can be a static except the constructor.

```javascript
	class Workout {
  
  	constructor(muscle) {
    	this.muscle = muscle;
  	}
  
  	exercise() {
    	console.log(this.muscle);
  	}
  
  	static train(muscle) {
    	return new Workout(muscle);
  	}
}

	let workout = Workout.train("biceps");
	console.log(workout);                 //{"muscle":"biceps"}

	let exercise = new Workout("squats");
	console.log(exercise.exercise());     //squats

	console.log(exercise.train("triceps"));   //exercise.train is not a function
```

<strong>Inheritance in ES6 using Classes</strong><br>
ES6 made it easy to do inheritance using classes. child class inheritance the parent class using `extends` keyword. `super` keyword is used to access to the parent class' constructor method. Classes that inherit from other classes are called `derived classes`. Have a look at the example below:

```javascript
	class SquareRoot {
    constructor(number) {
        this.number = number;
    }
    getSquareRoot() {
        return Math.sqrt(this.number);
    }
}

class bigSquareRoot extends SquareRoot {
    constructor(number) {
        super(number);
    }
}

var result = new bigSquareRoot(144);

console.log(result.getSquareRoot());                // 12
console.log(result instanceof bigSquareRoot);      // true
console.log(result instanceof SquareRoot);        // true
```

By the way, this is the default derived class example:
```javascript
	class DerivedClass extends ParentClass {
		constructor(...args) {
			super(...args);
		}
	}
```

<strong>Notes on derived classes:</strong><br>
- We have to use `super` keyword in derived classes if we define a constructor
- If we don't use a constructor in a derived class super is automatically called using all the arguments when we create a new instance of a derived class.
- `super()` is only used in derived classes
- `super()` is responsible for initializing `this`. That is why we cannot call `this` before `super` in a derived class constructor
- The only way to avoid calling super() is to return an object from the class constructor.

<h5>Overriding Class Methods</h5>
We can override parent class methods in the derived class. This is how it is done:

```javascript
	class SquareRoot {
    constructor(number) {
        this.number = number;
    }
    getSquareRoot() {
        return Math.sqrt(this.number);
    }
}

class bigSquareRoot extends SquareRoot {
    constructor(number) {
        super(number);
    }
    getSquareRoot() {
    /* we could use one them, not at the same time :)
    	1st expression is obvious
    	2nd expression calls the `getSquareRoot()` method in the parent class
    */
        return Math.sqrt(this.number);
        return super.getSquareRoot();
    }
}

var result = new bigSquareRoot(144);

console.log(result.getSquareRoot());                // 12
console.log(result instanceof bigSquareRoot);      // true
console.log(result instanceof SquareRoot);        // true
```

We override the parent class method using the same class method name in the derived class. This way parent class method no longer called unless we use `super` to access to the parent class method.

<h5>Static Members in Class Inheritance</h5>
If the base class has a static member it is also inherited by the derived class.
```javascript
	class SquareRoot {
    constructor(number) {
        this.number = number;
    }
    getSquareRoot() {
        return Math.sqrt(this.number);
    }
    static create(number) {
      return new SquareRoot(number);
    }
}

	class bigSquareRoot extends SquareRoot {
    	constructor(number) {
        	super(number);
    	}
	}

	var sqrt = bigSquareRoot.create(200);

	console.log(sqrt.getSquareRoot());  //14.142135623730951
```

<h5>Derived Class use cases</h5>
Derived Class can extend any expression as long as the expression is eventually a function with a <strong>[[Construct]]</strong> internal method and a <strong>prototype</strong>.

```javascript
	function SquareRoot(number) {
    	this.number = number;
	}

	SquareRoot.prototype.getSquareRoot = function() {
  		return Math.sqrt(this.number);
	};

	class bigSquareRoot extends SquareRoot {
    	constructor(number) {
        	super(number);
    	}
	}

	let sqrt = new bigSquareRoot(200);

	console.log(sqrt.getSquareRoot());  //14.142135623730951
```

Another example:
```javascript
	function SquareRoot(number) {
    	this.number = number;
	}

	SquareRoot.prototype.getSquareRoot = function() {
  		return Math.sqrt(this.number);
	};

	function SQRT() {
  		return SquareRoot;
	}

	class bigSquareRoot extends SQRT() {
    	constructor(number) {
        	super(number);
    	}
	}

	let sqrt = new bigSquareRoot(200);

	console.log(sqrt.getSquareRoot());  //14.142135623730951
```

We can also create a common <strong>mixin</strong> example using ES6 Classes:

```javascript
	let mixinObject1 = {
  		getSquareRoot() {
    		return Math.sqrt(this.number);
  		}
	};

	let mixinObject2 = {
  		getMultiplication() {
    		return this.number * this.number;
  		}
	};

	function mixin(...mixinsArgs) {
    	var parent = function() {};
    	Object.assign(parent.prototype, ...mixinsArgs);
    	return parent;
	}

	class DerivedMixin extends mixin(mixinObject1, mixinObject2) {
  		constructor(number) {
    		super();
    		this.number = number;
  		}
	}

let xyz = new DerivedMixin(144);
console.log(xyz.getSquareRoot());       //12
console.log(xyz.getMultiplication());   //20736
```

<h5>Inheriting from Built-in Objects</h5>
Inheriting from Built-in Objects using ES6 Classes are different from ES5 style classical inheritance. Difference in approach is based on how we create value of `this`:
- In Classical inheritance value of `this` starts as an instance of the derived object and receives the parent object's functionality
- In ES6 class-based inheritance value of `this` starts with all the functionality of the parent object

```javascript
	class ABC extends Array {}

	let arr = new ABC("one", "two", "three", "four");
	let chunk = arr.slice(0, 2);

	console.log(arr instanceof ABC);
	console.log(chunk instanceof ABC);
```

<h5>Symbol.species well-known symbol</h5>
when inheriting from built-ins, any method that returns an instance of the built-in will automatically return a derived class instance instead.
Following builtin types have `Symbol.species` property defined on them:
- Array
- ArrayBuffer (discussed in Chapter 10)
- Map
- Promise
- RegExp
- Set
- Typed Arrays

`Symbol.species` property returns a constructor function (returns `this`). Using Symbol.species, any derived class can determine what type of value should be returned when a method returns an instance.

```javascript
	class MyArray extends Array {
    static get [Symbol.species]() {
        return Array;
    }
}

let items = new MyArray(1, 2, 3, 4),
    subitems = items.slice(1, 3);

console.log(items instanceof MyArray);      // true
console.log(subitems instanceof Array);     // true
console.log(subitems instanceof MyArray);   //false
console.log(subitems instanceof Array);     //true
```
As we see from this example, we determine what to type should be returned from the class instance. All inherited methods like slice() will return an instance of Array, but not the instance of MyArray.

Understanding ECMAScript 6 Book:
> In general, you should use the Symbol.species property whenever you might want to use this.constructor in a class method. Doing so allows derived classes to override the return type easily. Additionally, if you are creating derived classes from a class that has Symbol.species defined, be sure to use that value instead of the constructor.

```javascript
	class Parent {
    static get [Symbol.species]() {
        return this;
    }

    constructor(value) {
        this.value = value;
    }

    clone() {
        return new this.constructor[Symbol.species](this.value);
    }
}

class Child1 extends Parent {
    // empty
}

class Child2 extends Parent {
    static get [Symbol.species]() {
        return Parent;
    }
}

let child1 = new Child1("foo"),
    clone1 = child1.clone(),
    child2 = new Child2("bar"),
    clone2 = child2.clone();

console.log(clone1 instanceof Child1);             // true
console.log(clone1 instanceof Parent);             // true
console.log(clone2 instanceof Parent);             // true
console.log(clone2 instanceof Child2);             //false
```
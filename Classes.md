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


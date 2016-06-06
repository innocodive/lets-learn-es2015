<h4>Desctructuring</h4>

ES6 introduced `destructuring` to make it easy to pull out data from existing objects or arrays. It is basically breaking down existing data structures into smaller peices.

<h5>Destructuring Objects</h5>

Have a look at the example below:

```javascript
	let habits = {
  		activity1 : "fitness training",
  		activity2 : "eating healthy food",
  		activity3 : "coding"
	};

	let { activity1, activity2, activity3 } = habits;

	console.log(activity1, activity2, activity3);
```

Give an attention to these 2 important details:
- In this line `let { activity1, activity2, activity3 } = habits;` these 3 variable names must be the same with  `habits` object property names. You cannot do this: `let { a1, a2, a3 } = habits;`.
- You cannot do this: `let {act1, act2, act3 };`. we must initialize this structure like `let { a1, a2, a3 } = habits`.
We can overwrite the existing variables:

```javascript
	let habits = {
  		activity1 : "fitness training",
  		activity2 : "eating healthy food",
  		activity3 : "coding"
	}, 
 	activity1 = "smoking",
 	activity2 = "sleeping",
 	activity3 = "binge drinking";

	({ activity1, activity2, activity3 } = habits);
	/* 
		give an attention to how the line above is destructured. A block statement cannot be on the left side of the `=`, that is why we put paranthesis around the statement to show that this is an expression.
	*/
	console.log(activity1, activity2, activity3);
```
Another example:

```javascript
	let habits = {
  		activity1 : "fitness training",
  		activity2 : "eating healthy food",
  		activity3 : "coding"
	}, 
 	activity1 = "smoking",
 	activity2 = "sleeping",
 	activity3 = "binge drinking";
 
 	function activity(act) {
   		console.log(act === habits);
   		console.log(act.activity1);
 	}
 
 	activity({ activity1, activity2, activity3 } = habits);

 	/*
 		there is no trickin the above line of code. Assignment is done first and then `habits` object is passed to the `activity` function as a parameter.
 	*/

	console.log(activity1, activity2, activity3);
```
One more example:
```javascript
	let habits = {
  		activity1 : "fitness training",
  		activity2 : "eating healthy food",
  		activity3 : "coding"
	}, 
 	activity1 = "smoking",
 	activity2 = "sleeping",
 	activity3 = "binge drinking",
 	onemore = "watching TV";
 
 	function activity(act) {
   		console.log(act === habits);
   		console.log(act.activity1);
 	}

	({ activity1, activity2, activity3, onemore } = habits);

	console.log(activity1, activity2, activity3, onemore);
	/*
		fitness training eating healthy food coding undefined
	*/
```
We get undefined for `onemore` variable.
<p>If we want to have different variable names than object property names, this is how we do it:</p>
```javascript
	let habits = {
  		activity1 : "fitness training",
  		activity2 : "coding"
	};

	let { activity1 : a1, activity2 : a2 } = habits;
	let { activity1 : x1, activity3 : x2 = "eating healthy food" } = habits;	//using default values

	console.log(a1, a2);
	console.log(x1, x2);
```
In the example above `activity1 : a1` means left of the colon (:) is a location of a value and right side is a name of the value.


<h5>Pulling out data from nested objects</h5>
Have a look at the example below:
```javascript
	let habits = {
  		one : "eating healthy",
  		two : "coding",
  		three : {
    		four : {
      			a : "training",
      			b : "resting"
    		}
  		}
	};

	let { three : {four} } = habits;
	let { three : { four : exercise } } = habits;	//give a different name to the local var

	console.log(four.a, four.b);
	console.log(exercise.a, exercise.b);
```

In this line, `let { three : {four} } = habits;` again left side of the colon is a location of the value. If there is a curly brace on the right side of the colon, that means location is nested and go 1 more level deeper.

<h4>Array Destructuring</h4>

Array destructuring sysntax is very similar to Object destructuring. We pull out data based on array element positions rather than named parameters. We also always need to initialize the array destructuring statement with var, let or const.
```javascript
	let dayInMyLife = ["train", "eat healthy", "code", "family time"];

	let [one, two, three] = dayInMyLife;
	let [, , , four] = dayInMyLife;

	console.log(one, two, three, four);
```
We can overwrite the existing variables:
```javascript
	let dayInMyLife = ["train", "eat healthy", "code", "family time"], 
		one = "sleep", 
		two = "smoke", 
		three = "be lazy";

		[one, two, three] = dayInMyLife;

		console.log(one, two, three);
```

We can specify default values in array destructuring:
```javascript
	let dayInMyLife = ["train", "eat healthy", "code", "family time"];

	let [one, two, three, , five = "do nothing"] = dayInMyLife;

	console.log(one, two, three, five);
```

<h5>Swap variable values using Array destructuring</h5>

If we tried to swap values in ES5, we would use a 3rd temporary variable to do that. With ES6 array destructuring, we don't need a 3rd temporary variable but we need a temporary array (array literal) to do that.

```javascript
	let a = 1, b = 2;

	[a, b] = [b, a];	// right side of the (=) is a temporary array literal where value swapping happens

	console.log(a, b);
```
<h5>Nested Array Destructuring</h5>

It works the same way how Nested Object Destructuring works.

```javascript
	let dayInMyLife = ["train", ["sleep", "smoke"], "code", "family time"];

		let [one, [ , two ], three, four] = dayInMyLife;

		console.log(one, two, three, four);
```

<h5>Using REST items in array destructuring and cloning arrays</h5>

Have a look at the example below:

```javascript
	let dayInMyLife = [["sleep", "smoke"], "train", "code", "family time"];

		let [[ one ], ...two] = dayInMyLife;
		let [...three] = dayInMyLife;

		console.log(one, two[0]);
		console.log(three);
```
<strong>One warning : </strong>REST should be last item in the array, no commas after REST item.
`let [...three, four] = dayInMyLife;` this would throw an error.

Let's say I received a complex JSON data. We need to pull out specific data, both object and array data. We can achieve this using Object and Array Destructuring without reading through the entire JSON data.

```javascript
	let habits = {
        goodHabits : {
          work : {
            one : "fitness training",
            two : "eating healthy food",
            three : "coding"
          },
          family : {
            one : "family time"
          }
        },
        badHabits : {
          personal : {
            one : "watching TV",
            two : "eating unhealthy"
          },
          public : {
            one : "smoking"
          }
        },
        importanceOrder : [1, 2]
    };
    
    let { goodHabits : { work },
          importanceOrder : [ index ] } = habits;
    
    console.log(work.one);
    console.log(index);
```

<h5>Destructured params as an optional function parameter</h5>
When functions take many optional parameters, one common approach to deal with those optional parameters was to use `options` object inside the function. It also means we have to read through the function body to see what kind of optional parameters are used.

```javascript
	function createHabits(type, value, options) {
  		options = options || {};
  
  		let intensity = options.intensity,
      		repeat = options.repeat;
	}

	createHabits("good habit", "fitness training", 
            {
              intensity : "45 minutes of training",
              repeat : "5 times a week"
            });
```
We can rewrite the example above using destructured parameters with few changes:
```javascript
	function createHabits(type, value, { intensity, repeat, extras } = {}) {
  		console.log(intensity);
  		console.log(extras);
	}
  

	createHabits("good habit", "fitness training", 
            {
              intensity : "45 minutes of training",
              repeat : "5 times a week"
            });
```
Missing function arguments will be set to undefined like in `console.log(extras);`. Another thing here is, all destructuring objects and arrays we've seen so far throws an error if the right side of the `=` is null or undefined like `{ intensity, repeat} = undefined`. That is why in the example above missing 3rd argument will throw an error. To prevent that we assign a default empty object like this `{ intensity, repeat, extras } = {}`.

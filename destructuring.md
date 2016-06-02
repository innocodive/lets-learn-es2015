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
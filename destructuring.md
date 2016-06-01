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

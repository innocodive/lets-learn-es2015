<h4>Find a Substring in a String</h4>

First Javascript String method comes to mind when we search a substring in a string is `indexOf()` method. ES6 introduced 3 more methods to accomplish similar tasks. I also added there the repeat() method which is introduced in ES6. The repeat() method returns a string unlike other 3 strings.

1. The **includes()** method returns <b><u>true</b></u> if the given text is found anywhere within the string. It returns false if not.
2. The **startsWith()** method returns true if the given text is found at the beginning of the string. It returns false if not.
3. The **endsWith()** method returns true if the given text is found at the end of the string. It returns false if not.
4. The **repeat()** takes a string as an argument and repeats it as given times.

```javascript
	var str = "I am learning ES6!";

	console.log(str.startsWith("am"));
	console.log(str.endsWith("ES6"));
	console.log(str.includes("I am"));
	console.log(str.startsWith("l", 6));
	console.log(str.includes("ing", 8));
	console.log(str.endsWith("!", 12));

	console.log("abc".repeat(2));	//abcabc
	console.log("123".repeat(3));	//123123123
```

Quote from `Understanding EcmaScript 6` Book:
> The `startsWith()`, `endsWith()`, and `includes()` methods will throw an error if you pass a regular expression instead of a string. This stands in contrast to `indexOf()` and `lastIndexOf()`, which both convert a regular expression argument into a string and then search for that string.

<h4>Template Literals</h4>
Template Literals are Javascript expressions. Things we can do now thanks to the introduction of `Template Literals` in ES6:

- Multiline Strings
- Substitute a part of a string with a variable value
- HTML Escaping
- Tagged Templates

```javascript
	// here is the Template Literals basic syntax
	// we use backtick (`) instead of usual (") or (')

	let str = `I am learning ES6`;
	let str = `I am learning \`ES6\``;	// use backslash (\) to escape backtick (`)
```
<h6>Multiline Strings</h6>

We don't need to depend on arrays or new line (\n) to create a multiline strings as we used to do. We can put a multiline strings in a backtick (`) and it would do the job for us.
```javascript
	let str = `multiline string
				example`;
	console.log(str);
```
<h6>Substitutions</h6>
Substitutions are Javascript expessions. We use substitution delimeter `${ }` to embed in a string any accesible local variable in the same scope.

```javascript
	let who = "I am", 
	what = "ES6", 
	message = `${who} learning ${what}`;

	console.log(message);
```

We can nest Template Literals:

```javascript
	let who = "I am", 
	what = "ES6", 
	message = `Everyday little by little, ${ 
	`${who} learning ${what}` }`;

	console.log(message);
```

<h6>Tagged Templates</h6>

We basically pass the entire Template Literal with its Substitutions to a `TAG Function`, and this function performs the transformation on the string and returns the final string value. Here is the basic syntax for Tagged Templates:

```javascript
	let message = tag`template literal string`;	// here, `tag` is a Function
```

This is the basic syntax for the Tag Function:

```javascript
	function tag(literals, ...substitutions) {
	//return final string value
}
```
As you can see the `tag function` uses **REST/SPREAD** arguments. Consider this Template Literal example below:

```javascript
	let who = "I am", 
	what = "ES6", 
	message = printTempLiteral`Everyday little by little, ${ 
	`${who} learning ${what}` }`;

	console.log(message);
```

Function `printTempLiteral()` will receive 2 arguments. First it will receive `literals array`, basically anything except substituions:

1. Empty string before the 1st substitution ("")
2. String between 1st and 2nd substitions
3. String after the 2nd substitution

then it receives all the elements of `substitutions array`.
So, `literals[0]` is always the 1st thing comes in the Final String. There is always one fewer substitution than literal, which means the expression `substitutions.length === literals.length - 1` is always true.

So, here is the final form of our example:

```javascript
	function printTempLiteral(literals, ...substitutions) {
    let result = "";

    // run the loop only for the substitution count
    for (let i = 0; i < substitutions.length; i++) {
        result += literals[i];
        result += substitutions[i];
    }

    // add the last literal
    result += literals[literals.length - 1];

    return result;
	}

	let who = "I am", 
		what = "ES6", 
		message = printTempLiteral`Everyday little by little, ${ 
				`${who} learning ${what}` }`;

	console.log(message);	// Everyday little by little, I am learning ES6
```

<h6>Using String.raw() tag in Template Literals</h6>

If we have to deal raw string data, the simplest way to achieve that is to use `String.raw()` tag.

```javascript
	let string1 = `multiline\nString`,
		string2 = String.raw`Singline\nString`;

		console.log(string1);
		console.log(string2);
```

We can pass raw string data into template tags. We need to access to `literal.raw[]` array to get and process the raw string data. We use the same example we used before but with a small change:

```javascript
	function printTempLiteral(literals, ...substitutions) {
    let result = "";

    // run the loop only for the substitution count
    for (let i = 0; i < substitutions.length; i++) {
        result += literals.raw[i];		// get the raw values
        result += substitutions[i];
    }

    // add the last literal
    result += literals[literals.length - 1];

    return result;
    }

    let who = "I am", 
        what = "ES6", 
        message = printTempLiteral`Everyday little by little\n, ${ 
                `${who} learning ${what}` }`;

    console.log(message);
```
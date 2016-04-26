<h4>Find a Substring in a String</h4>

First Javascript String method comes to mind when we search a substring in a string is `indexOf()` method. ES6 introduced 3 more methods to accomplish similar tasks. I also added there the repeat() method which is introduced in ES6. The repeat() method returns a string unlike other 3 strings.

1. The **includes()** method returns true if the given text is found anywhere within the string. It returns false if not.
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
Things we can do now thanks to the introduction of `Template Literals` in ES6:

- Multiline Strings
- Substitute a part of a string with a variable value
- HTML Escaping

```javascript
	// we use backtick (`) instead of usual (") or (')

	let str = `I am learning ES6`;
	let str = `I am learning \'ES6\``;	// use backslash (\) to escape backtick (`)
```
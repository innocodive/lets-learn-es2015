<h4>What is new?</h4>

ES6 introduced many Array methods.
The new Array() constructor behaves differently based on the type and number of arguments passed to it. <strong>Array.of() method</strong> creates a new array. It is introduced to deal with the quirk of creating an array using Array Constructor. 
```javascript
	let set1 = new Array(2),
    set2 = new Array("2"),
    set3 = new Array(1, 7),
    set4 = new Array(1, "5");
    
    console.log(set1.length);     //2
    console.log(set1[0]);         //undefined
    console.log(set2.length);     //1
    console.log(set2[0]);         //2
    console.log(set3.length);     //2
    console.log(set3[1]);         //7
    console.log(set4[1]);         //5
```
Array.of() example. This exactly the same with the example above but we get slightly different results:
```javascript
	let set1 = Array.of(2),
    set2 = Array.of("2"),
    set3 = Array.of(1, 7),
    set4 = Array.of(1, "5");
    
    console.log(set1.length);     //1
    console.log(set1[0]);         //2
    console.log(set2.length);     //1
    console.log(set2[0]);         //2
    console.log(set3.length);     //2
    console.log(set3[1]);         //7
    console.log(set4[1]);         //5
```
Understanding ECMAScript 6 Book:
> The Array.of() method does not use the Symbol.species property (discussed in Chapter 9) to determine the type of return value. Instead, it uses the current constructor (this inside the of() method) to determine the correct data type to return.


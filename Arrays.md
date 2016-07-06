<h4>What is new?</h4>

ES6 introduced many Array methods.
<strong>Array.of() method</strong> creates a new array. It is introduced to deal with the quirk of creating an array using Array Constructor. The new Array() constructor behaves differently based on the type and number of arguments passed to it.
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
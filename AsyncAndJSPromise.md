<h4>Asynchronous Programming and ES6 Promises</h4>

Evolution of Asynchronous Programming:
1 - The Event Model
2 - The callback pattern
3 - ES6 Promises

The Event Model example:
```javascript
	let target = document.getElementById(".fire-event");
	target.onclick = function(event) {
  		console.log("click event happened");
};
```

Node.js popularized the The Callback Pattern:
```javascript
	$.getJSON("jsonFile.txt", function(data) {
  		//do something with the data
	});
```
Callback Pattern was problematic when it comes to accomplishing more complex functionalities like running 2 async operations in parallel or starting 2 async operations at the same time. ES6 Promises bring improvements to solve those mentioned issues.

<h4>ES6 Promise basics</h4>

From [MDN] (https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise):
> The Promise object is used for asynchronous computations. A Promise represents an operation that hasn't completed yet, but is expected in the future.

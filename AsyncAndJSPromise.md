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

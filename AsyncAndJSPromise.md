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

Node.js popularized the The Callback Pattern. Here is an example from jQuery:
```javascript
	$.getJSON("jsonFile.txt", function(data) {
  		//do something with the data
	});
```
Callback Pattern was problematic when it comes to accomplishing more complex functionalities like running 2 async operations in parallel or starting 2 async operations at the same time. ES6 Promises bring improvements to solve those mentioned issues.

<h4>ES6 Promise basics</h4>

From [MDN] (https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise):
> The Promise object is used for asynchronous computations. A Promise represents an operation that hasn't completed yet, but is expected in the future.

<strong>3 states of Promise:</strong> Promise has an internal property [[PromiseState]]. It is set to `pending`, `fullfilled` or `rejected`. We cannot programmatically access to this property value. `then()` method is what we need to monitor the promise state and take an action:
- Pending
- Fullfilled
- Rejected

A promise represents a proxy for a value not necessarily known when the promise is created. Using this method, we can associate handlers to async action's eventual success value or failure reason. This way we make async methods to return values same like synchronous methods. Return a promise not the final value.

All promises have a `then()` method. Then() method takes 2 arguments (functions). 1st function is to call when the promise is fullfilled and the 2nd function is to call when the promise rejected. Both promise arguments are optional, so we can listen to both or any of them separately.

Understanding ECMAScript 6 Book:
> Any object that implements the then() method in this way is called a thenable. All promises are thenables, but not all thenables are promises.

```javascript
	let promise = readFile("sampleText.txt");

	promise.then(function(result) {
  		//fullfilled
  		console.log(result);
	}, function(error) {
  			//rejected
  			console.log(error.message);
	});
```
catch() method is also acts like then() only when a rejection handler is associated with it:
```javascript
	promise.catch(function(err) {
    	// rejection
    	console.error(err.message);
	});

	// is the same as:

	promise.then(null, function(err) {
    	// rejection
    	console.error(err.message);
	});
```

We should always attach a rejection handler to a promise, otherwise it would fail silently.

<strong>What is going to happen if we attach handlers after the promise fullfilled?</strong>
Handlers will still be executed.

Each call to then() or catch() creates a new job to be executed when the promise is resolved. But these jobs end up in a separate job queue that is reserved strictly for promises.

<h5>How to create a Promise?</h5>
We use a `Promise` constructor to create a new promise. The constructor accepts 1 argument, which is called `executor function`. An executor function accepts 2 arguments: `resolve() function` and `reject() function`. The resolve() function is called when the executor has finished successfully to signal that the promise is ready to be resolved, while the reject() function indicates that the executor has failed.
```javascript
	function createPromise(filename) {
  return new Promise(function(resolve, reject) {
    readFile(filename, function(error, result) {
      //check if the call is failed
      if(error) {
        reject(error);
        return;
      } else {
        resolve(result);
      }
    });
  });
}

let promise = createPromise("mySampleTerxt.txt");

promise.then(function(result) {
  console.log(result);
}, function(error) {
  console.log(error.message);
});
```


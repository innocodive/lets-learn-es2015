<h4>Asynchronous Programming and ES6 Promises</h4>

Evolution of Asynchronous Programming:
- The Event Model
- The callback pattern
- ES6 Promises

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

<h5>How to create an Unsettled Promise?</h5>
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

	let promise = createPromise("mySampleText.txt");

	promise.then(function(result) {
  		console.log(result);
	}, function(error) {
  		console.log(error.message);
	});
```

The executor runs <strong>`immediately`</strong> before any code when readFile() is called. When either resolve() or reject() is called inside the executor, a job is added to the job queue to resolve the promise. This is called job scheduling. Job scheduling basically means, don't run imediately, run later.
Have a look at the example below:

```javascript
	let promise = new Promise(function(resolve, reject) {
    	console.log("Promise");
    	resolve();		//Calling resolve() triggers an asynchronous operation
	});

//Functions passed to then() and catch() are executed asynchronously, and also added to the job queue
	promise.then(function() {
    	console.log("Resolved.");
	});

	console.log("Hi!");

	/*
Promise
Hi!
Resolved
	*/
```

Note that even though the call to then() appears before the console.log("Hi!") line, it doesn’t actually execute until later (unlike the executor). That’s because fulfillment and rejection handlers are always added to the end of the job queue after the executor has completed.

<h5>How to create a Settled Promise?</h5>

There are two methods that create settled promises given a specific value:
- Promise.resolve()
- Promise.reject()

<h5>Settled Promises</h5>
- Promise.resolve(): Accepts 1 argument and returns a promise in fullfilled state
- Promise.reject(): Accepts 1 argument and returns a promise in rejected state

No job scheduling occurs. We need to listen to the promise to retrieve the value:

```javascript
	let yeap = Promise.resolve(100);

	yeap.then(function(value) {
  		console.log(value);
	});

	let nope = Promise.reject(200);

	nope.catch(function(value) {
  		console.log(value);
	});
```
Understanding ECMAScript 6 Book:
> If you pass a promise to either the Promise.resolve() or Promise.reject() methods, the promise is returned without modification.

<h5>Create a non-promise thenable</h5>
Both `Promise.resolve()` and `Promise.reject()` also accept `non-promise thenables` as arguments. When passed a non-promise thenable, these methods create `a new promise` that is called after the then() function.<br>
A non-promise `thenable` is created when an object has a `then() method` that accepts a `resolve` and a `reject` argument.<br>
At this point thenable doesn't have a promise characteristics except then() method. We can pass a thenable as an argument to the `Promise.resolve()` as an argument to convert thenable into a fullfilled promise:
```javascript
	let thenable = {
		then : function(resolve, reject) {
			resolve(100);
	}
};

let yeap = Promise.resolve(thenable);

yeap.then(function(value) {
	console.log(value);
});
```
Same example could be done using `Promise.reject()` and `nope.catch().<br>
<strong>How to find out if a thenable is a promise?</strong><br>
When you’re unsure if an object is a promise, passing the object through Promise.resolve() or Promise.reject() (depending on your anticipated result) is the best way to find out because promises just pass through unchanged.

<h5>Executor Function Errors</h5>
If an error is thrown inside an executor, then the promise’s rejection handler is called.<br>
There is an implicit try-catch inside every executor such that the error is caught and then passed to the rejection handler:
```javascript
	let promise = new Promise(function(resolve, reject) {
  		throw new Error("fake error message");
	});

	promise.catch(function(error) {
  		console.log(error.message);
	});
// example below is the same with the one above

	let promise = new Promise(function(resolve, reject) {
  		try {
    		throw new Error("fake error message");
  		} catch(err) {
    		reject(err);
  		}
	});

	promise.catch(function(error) {
  		console.log(error.message);
	});
```
An Error thrown in the Executor will only be reported if a rejection handler is attached to the promise, otherwise an error will be suppressed, another way of saying, it will fail silently.

<h5>Rejection Handling in a Promise</h5>
A `rejectionhandled` event is fired after these 2 things happen:
- A javascript Promise is rejected
- After a rejection is handled by the promise rejection handler


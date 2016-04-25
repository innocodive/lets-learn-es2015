<h4>Spread or Rest Operator</h4>

ES6 introduced a new operator called <b>Spread/Rest Operator (...)</b>. Have a look at the example below:

```javascript
	function foo(...args) {
    // `args` is already a real array

    console.log( ...args );
}
foo(1, 2, 3, 4, 5);		// 1, 2, 3, 4, 5
```
We used to do
```javascript
	var args = Array.prototype.slice.call( arguments );		// turn `arguments` into a real array
	// and
	// pass all `args` as arguments to `foo(..)`
    foo.apply( null, args );
```
<h4>Setting Default Values for a function Parameter</h4>

Setting a Default Value for a Function paramater got much simpler with ES6. Have a look at this example:

```javascript
	function foo(x = 5, y = 10) {
    	console.log( x + y );
	}

	foo();
	foo( 10, 16 );
	foo( 0, 12 );

	foo( 8 );
	foo( 8, undefined );
	foo( 8, null );

	foo( undefined, 12 );
	foo( null, 12 );
```
This is how your Function `foo()` would look like in pre-ES6 Javascript:

```javascript
	function foo() {
    var x = arguments.length <= 0 || arguments[0] === undefined ? 5 : arguments[0];
    var y = arguments.length <= 1 || arguments[1] === undefined ? 10 : arguments[1];

    console.log(x + y);
}
```

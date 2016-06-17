<h4>Iterators and Generators</h4>
<strong>Basic Rules for iterators:</strong><br>

Iterator is an object with `next()` method which in turn also returns a new object. A returned new object has 2 properties: `value` and `done`. `Value` points to the next element in an array and `done` is a boolean which is always false unless it is the end of the array.<br>
If we call `next()` method after we reach to the end of the array, `done` will return <i>true</i> and value will have <i>undefined</i> value.<br>

ES6 introduced a `generator` function which returns an iterator.
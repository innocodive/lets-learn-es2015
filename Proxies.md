<h4>Proxies and Reflection API</h4>
ES6 exposes inner-workings of the Javascript Engine. ES6 allows developers to create their own built-in objects. This is done through use of `proxies`.
<strong>What are proxies?</strong><br>
Proxies are wrappers that can intercept and alter low-level operations of the JavaScript engine.<br>
Arrays are considered an `exotic object` in ES6, because of its non-standard behaviour. You can modify array items changing `length` property, and adding more items to the array effects its `legnth` property. Developers couldn't mimic this behaviour in their own created objects before ES6. We can achieve this using `proxies` in ES6.


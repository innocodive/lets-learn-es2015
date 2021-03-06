<h4>Proxies and Reflection API</h4>
ES6 exposes inner-workings of the Javascript Engine. ES6 allows developers to create their own built-in objects. This is done through use of `proxies`.<br>
<strong>What are proxies?</strong><br>
Proxies are wrappers that can intercept and alter low-level operations of the JavaScript engine.<br>
Arrays are considered an `exotic object` in ES6, because of its non-standard behaviour. You can modify array items changing `length` property, and adding more items to the array effects its `legnth` property. Developers couldn't mimic this behaviour in their own created objects before ES6. We can achieve this using `proxies` in ES6.
```javascript
	let arr = ["one", "two", "three"];

	console.log(arr.length);      //3
	arr.length = 2;
	console.log(arr.length);      //2
	console.log(arr);             //["one", "two"]
```

<h5>What are Proxies and Reflection API?</h5>

We can create proxies by calling `new Proxy()`. We use proxies in place of another object is called `target object`. Proxies and target objects are virtually the same in terms of functionality. We use proxies to intercept and override low-level operations on the target. This is done using `trap` functions which responds to certain operations.<br>
`Reflection Object` is a collection of methods which provide the default behaviour for the same low-level operations which proxies can override.<br>
Reflection API methods names are the same with proxy trap function names.

Proxy Trap	| Overrides the Behaviour Of	| Default Behaviour
------------|---------------------------|-----------------
get	| Reading a property value	| Reflect.get()
set	| Writing to a property	| Reflect.set()
has	| The in operator	| Reflect.has()
deleteProperty	| The delete operator	| Reflect.deleteProperty()
getPrototypeOf	| Object.getPrototypeOf()	| Reflect.getPrototypeOf()
setPrototypeOf	| Object.setPrototypeOf()	| Reflect.setPrototypeOf()
isExtensible	| Object.isExtensible()	| Reflect.isExtensible()
preventExtensions	| Object.preventExtensions()	| Reflect.preventExtensions()
getOwnPropertyDescriptor	| Object.getOwnPropertyDescriptor()	| Reflect.getOwnPropertyDescriptor()
defineProperty	| Object.defineProperty()	| Reflect.defineProperty
ownKeys	| Object.keys, <br>Object.getOwnPropertyNames(), <br>Object.getOwnPropertySymbols()	| Reflect.ownKeys()
apply	| Calling a function	| Reflect.apply()
construct	| Calling a function with new	| Reflect.construct()

Each trap overrides some built-in behavior of JavaScript objects, allowing you to intercept and modify the behaviour. If you still need to use the built-in behaviour, then you can use the corresponding reflection API method.

<strong>Proxy without a trap</strong><br>
```javascript
	let target = {}, handler = {};
	let proxy = new Proxy(target, handler);

	proxy.name = "proxy";
	console.log(target.name);			//proxy

	target.name = "target";
	console.log(proxy.name);			//target
```
`proxy` forwards all operations directly to `target`. When "proxy" is assigned to the proxy.name property, name is created on `target`. The proxy itself is not storing this property. It, basically, forwards the operation to target.

<strong>set Trap</strong><br>
`set` trap receives 4 arguments:<br>
`set(trapTarget, key, value, receiver)`<br>
`trapTarget` is a proxy's target and `receiver` is generally a proxy.<br>
`Reflect.set(trapTarget, key, value, receiver)`<br>
Reflect.set() accepts exactly the same arguments set Trap accepts.

```javascript
	let target = {
    name: "target"
};

let proxy = new Proxy(target, {
    set(trapTarget, key, value, receiver) {
        //validate key and value
        // and then add the property
        return Reflect.set(trapTarget, key, value, receiver);
    }
});

// adding a new property
proxy.count = 1;
console.log(proxy.count);       // 1
console.log(target.count);      // 1
```

<strong>Object Shape Validation or get Trap</strong><br>
Understanding ECMASript 6 Book:
> An object shape is the collection of properties and methods available on the object. JavaScript engines use object shapes to optimize code, often creating classes to represent the objects.

`get` Trap accepts 3 arguments:<br>
`get(trapTarget, key, receiver)`. Reflect.get is the same: `Reflect.get(trapTarget, key, receiver)`<br>

```javascript
	let proxy = new Proxy({}, {
        get(trapTarget, key, receiver) {
            //check if key in receiver
            //then add the property
            return Reflect.get(trapTarget, key, receiver);
        }
    });

// nonexistent properties throw an error
console.log(proxy.nme);             // throws error
```


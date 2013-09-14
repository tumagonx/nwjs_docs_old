Different windows of a node-webkit-based application have different JavaScript contexts, i.e. each window has its own global object and its own set of global constructors (such as `Array` or `Object`).

That's common practice among web browsers. It's a good thing because, for example:

* when an object's prototype is replaced or augmented by some library (such as [Prototype](http://prototypejs.org/)) or a simpler script, the analogous objects in other windows are unaffected nevertheless;

* when a programmer makes a mistake (such as [missing `new` before a poorly written constructor](http://ejohn.org/blog/simple-class-instantiation/)) and the bug affects (pollutes) the global scope, it still cannot affect larger areas (several windows);

* malicious applications cannot access confidential data structures in other windows.

Node modules in node-webkit run in their own shared Node context.

## Determining the context of a script

If the `require()` method (of Node.js [modules API](http://nodejs.org/docs/latest/api/modules.html)) is used, then the required module runs in the Node's context.

If HTML `<script src="...">` element (or jQuery's [`$.getScript()`](http://api.jquery.com/jQuery.getScript/), or any other similar method) is used in some window, then the script runs in the context of that window.

If the module is given as the value of the [`"node-main"`](https://github.com/rogerwang/node-webkit/wiki/node-main) property of the application's [manifest file](https://github.com/rogerwang/node-webkit/wiki/Manifest-format), then the module runs in the Node's context but later has access to the `window` object. (See the “[node-main](https://github.com/rogerwang/node-webkit/wiki/node-main)” article for details.)

## Features and limitations of the Node's context

Scripts running in the Node's context may use `__dirname` variable to read the path of their file's directory.

The Node.js `global` object is the global object in the Node's context. Any WebKit window's `window` object is not the global object and even is not implicitly available (the special case of [node-main](https://github.com/rogerwang/node-webkit/wiki/node-main) is the only exception), i.e. you have to (explicitly) pass the `window` object to your module's function if you need to access it.

That also means you cannot rely on `alert()` (which is actually `window.alert()`) for debugging. You may, however, use `console.log()`; its output (and the output of other similar methods such as `console.warn()` and `console.error()`) is redirected to WebKit's console. You may see it in your “[Developer Tools](Debugging-with-devtools)” window (on its “Console” tab).

You cannot use `require('nw.gui')` (to access the node-webkit's [GUI API](https://github.com/rogerwang/node-webkit/wiki/API-Overview-and-Notices)) from the Node's context, because there's no GUI outside of a window.

## Working around differences of contexts

While differences of contexts are generally benefitial, sometimes they may constitute a problem in your (or some other person's) code, and a need for a workaround arises.

The most common cause for such problems is the behaviour of the `instanceof` operator in JavaScript. As you may [see in MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof), the operation `someValue instanceof someConstructor` tests whether an object has in its prototype chain the `prototype` property of the given constructor. However, if `someValue` is passed from a different JavaScript context, then it has its own line of ancestor objects, and the `someValue instanceof someConstructor` check fails inevitably.

For example, a simple check `someValue instanceof Array` cannot determine if a variable's value is an array's if it's passed from another context (see “[Determining with absolute accuracy whether or not a JavaScript object is an array](http://web.mit.edu/jwalden/www/isArray.html)” for details).

The same problem arises when the `.constructor` property is checked directly (for example, when `someValue.constructor === Array` is used instead of `someValue instanceof Array`).

The following constructors are children of the context-dependent global object, and thus their instances are affected:

* **Standard object types:** `Array`, `Boolean`, `Date`, `Function`, `Number`, `Object`, `RegExp`, `String`.

* **Typed array types:** `ArrayBuffer`, `DataView`, `Float32Array`, `Float64Array`, `Int16Array`, `Int32Array`, `Int8Array`, `Uint16Array`, `Uint32Array`, `Uint8Array`.

* **Error types:** `Error`, `EvalError`, `RangeError`, `ReferenceError`, `SyntaxError`, `TypeError`, `URIError`.

There are several ways to work around this problem.

### Avoiding instanceof

The easiest way to prevent context-related problems is to avoid using `instanceof` when a value may come from another JavaScript context. For example, you may use [`Array.isArray`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray) method to check whether a value is an array, and that method works reliably across contexts.

However, if such a convenient alternate method is not readily available, or when you face a problem in someone other's (not your own) code and patching that would need a hassle, then another workaround is necessary.

### Using a constructor from the other context

When you foresee passing a value to some other context, you may providently use a constructor from that context in order to construct you value. The value then would easily pass any `instanceof` checks in that context.

For example, the well-known [`async`](https://github.com/caolan/async) module uses (in [its code](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js) dated 2013-05-20) numerous `.constructor` checks (in lines [472](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js#L472), [505](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js#L505), [545](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js#L545), [675](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js#L675), [752](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js#L752)) and thus fails whenever it encounters an array from another context. For example, if you run the following code from a node-webkit's window context,

```js
require('async').waterfall([
   function(callback){
      console.log('1.');
      callback(null, 'one', 'two');
   },
   function(arg1, arg2, callback){
      console.log('2.');
      callback(null, 'three');
   },
   function(arg1, callback){
      console.log('3.');
      callback(null, 'done');
   }
], function (err, result) {
   console.log('Fin.');
   if( err ) throw err;
   console.log(result);
});
```
it throws the `Error: First argument to waterfall must be an array of functions` (erroneously thinking it's not an array).

Using the [`nwglobal`](https://github.com/Mithgol/nwglobal) module, you may access the Node's context's `Array` constructor and rewrite the above code:

```js
require('async').waterfall( require('nwglobal').Array(
   function(callback){
      console.log('1.');
      callback(null, 'one', 'two');
   },
   function(arg1, arg2, callback){
      console.log('2.');
      callback(null, 'three');
   },
   function(arg1, callback){
      console.log('3.');
      callback(null, 'done');
   }
), function (err, result) {
   console.log('Fin.');
   if( err ) throw err;
   console.log(result);
});
```

It makes the `async` module happy.

However, in some cases you cannot (or won't) use the constructor directly to create your value. (For example, as you may [see in MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function), using the `Function` constructor is less efficient than declaring a function, and it also does not create a closure.) In such cases another workaround is necessary.

### Replacing `__proto__`

The non-standard (but widely implemented) `__proto__` property of an object can be used (as you may [see in MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)) to change the object's internal “&#91;&#91;Prototype&#93;&#93;” property (initially containing the prototype of its constructor).

When you foresee passing a value to some other context, you may providently replace the value's `__proto__` property with a constructor from that context. The value then would easily pass any `instanceof` checks in that context.

For example, the well-known [`async`](https://github.com/caolan/async) module uses (in [its code](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js) dated 2013-05-20) an `instanceof Function` check ([on line 428](https://github.com/caolan/async/blob/d8601a17ab0bc6a1572227998b6e9182637f37b6/lib/async.js#L428)) and thus it fails whenever it encounters a function from another context. For example, if you run the following code from a node-webkit's window context,

```js
var getData = function (callback) {
   setTimeout(function(){
       console.log('1.1: got data');
       callback();
   }, 300);
}
var makeFolder=  function (callback) {
   setTimeout(function(){
       console.log('1.1: made folder');
       callback();
   }, 200);
}
var writeFile= function(callback) {
   setTimeout(function(){
       console.log('1.1: wrote file');
       callback(null, 'myfile');
   }, 300);
}
var emailFiles= function(callback, results) {
   console.log('1.1: emailed file: '+results.writeFile);
   callback(null, results.writeFile);
}
require('async').auto({
   getData:getData ,
   makeFolder:makeFolder,
   writeFile: ['getData', 'makeFolder',writeFile],
   emailFiles: ['writeFile',emailFiles]
}, function(err, results) {
   console.log('1.1: err: '+ err);
   console.log('1.1: results: '+ results);
});
```

it throws the `has no method slice` error (erroneously thinking that the given value is not a function and thus it has to be an array, and then attempting to slice that “array”).

Using the [`nwglobal`](https://github.com/Mithgol/nwglobal) module, you may access the Node's context's `Function` constructor and rewrite the above code:

```js
var getData = function (callback) {
   setTimeout(function(){
       console.log('1.1: got data');
       callback();
   }, 300);
}
getData.__proto__ = require('nwglobal').Function;
var makeFolder=  function (callback) {
   setTimeout(function(){
       console.log('1.1: made folder');
       callback();
   }, 200);
}
makeFolder.__proto__ = require('nwglobal').Function;
var writeFile= function(callback) {
   setTimeout(function(){
       console.log('1.1: wrote file');
       callback(null, 'myfile');
   }, 300);
}
writeFile.__proto__ = require('nwglobal').Function;
var emailFiles= function(callback, results) {
   console.log('1.1: emailed file: '+results.writeFile);
   callback(null, results.writeFile);
}
emailFiles.__proto__ = require('nwglobal').Function;
require('async').auto({
   getData:getData ,
   makeFolder:makeFolder,
   writeFile: ['getData', 'makeFolder',writeFile],
   emailFiles: ['writeFile',emailFiles]
}, function(err, results) {
   console.log('1.1: err: '+ err);
   console.log('1.1: results: '+ results);
});
```

It makes the `async` module happy.
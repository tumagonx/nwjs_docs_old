Different windows of a node-webkit-based application have different JavaScript contexts, i.e. each window has its own global object and its own set of global constructors (such as `Array` or `Object`).

That's common practice among web browsers. It's a good thing because, for example:

* when an object's prototype is replaced or augmented by some library (such as [Prototype](http://prototypejs.org/)) or a simpler script, the analogous objects in other windows are unaffected nevertheless;

* when a programmer makes a mistake (such as [missing `new` before a poorly written constructor](http://ejohn.org/blog/simple-class-instantiation/)) and the bug affects (pollutes) the global scope, it still cannot affect larger areas (several windows);

* malicious applications cannot access confidential data structures in other window.

Node modules in node-webkit run in their own shared Node context.

## Determining the context of a script

If the `require()` method (of Node.js [modules API](http://nodejs.org/docs/latest/api/modules.html)) is used, then the required module runs in the Node's context.

If HTML `<script src="...">` element (or jQuery's [`$.getScript()`](http://api.jquery.com/jQuery.getScript/), or any other similar method) is used in some window, then the script runs in the context of that window.

If the module is given as the value of the [`"node-main"`](https://github.com/rogerwang/node-webkit/wiki/node-main) property, it runs in the Node's context but later has an access to the `window` object. (See the “[node-main](https://github.com/rogerwang/node-webkit/wiki/node-main)” article for details.)

## Features and limitations of the Node's context

Scripts running in the Node's context may use `__dirname` variable to read the path of their file's directory.

The Node.js `global` object is the global object in the Node's context. Any WebKit window's `window` object is not the global object and even is not implicitly available (the special case of [node-main](https://github.com/rogerwang/node-webkit/wiki/node-main) is the only exception), i.e. you have to (explicitly) pass the `window` object to your module's function if you need to access it.

That also means you cannot rely on `alert()` (which is actually `window.alert()`) for debugging. You may, however, use `console.log()`; its output (and the output of other similar methods such as `console.warn()` and `console.error()`) is redirected to WebKit's console. You may see it in your “[Developer Tools](Debugging-with-devtools)” window (on its “Console” tab).

You cannot use `require('nw.gui')` (to access the node-webkit's [GUI API](https://github.com/rogerwang/node-webkit/wiki/API-Overview-and-Notices)) from the Node's context, because there's no GUI outside of a window.
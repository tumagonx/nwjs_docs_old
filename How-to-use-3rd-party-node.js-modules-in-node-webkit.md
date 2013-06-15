There are three types of modules in Node.js:

* internal modules (parts of [Node API](http://nodejs.org/docs/latest/api/))

* 3rd party modules written in JavaScript

* 3rd party modules with C/C++ addons

All of these types can be used in node-webkit.

## Internal modules

The internal (built-in) modules of Node.js can be used as directly as in Node, according to the documentation on [Node API](http://nodejs.org/docs/latest/api/).

For example, `var fs = require('fs')` is enough to start using the file system module.

For example, you may use `process` without any `require(…)`, as in Node.

There are, however, some small changes between Node's and node-webkit's API. (See “[[Changes related to node]]” for details.)

## 3rd party JavaScript modules

If a 3rd party module is written totally in JavaScript (i.e. does not contain any C/C++ [addons](http://nodejs.org/docs/latest/api/addons.html)), it can be used in node-webkit the same way it is used in Node: `require('moduleName')`.

However, the behaviour of relative paths in that `require()` method depends on how the parent file itself is used in the application (here “the parent file” is the file in which the `require()` method is called):

* If the parent file was also required by Node (using `require()`), then the child's relative path is treated as relative to its parent.

* If the parent file is included by WebKit (using any web technology: classic DOM `window.open()`, node-webkit's [`Window.open()`](Window#openurl-options), classic DOM [`XMLHttpRequest`](https://developer.mozilla.org/en/docs/DOM/XMLHttpRequest), jQuery's [`$.getScript()`](http://api.jquery.com/jQuery.getScript/), HTML `<script src="...">` element, etc.), then the child's relative path is treated as relative to the application's root directory.

The former rule means that any module's submodules are required exactly as in Node.

The latter rule means that you may put the necessary modules in the `/node_modules` subdirectory of your application and then `require()` them in scripts on your HTML pages.

For example, you may install such modules from [`npm` packages](https://npmjs.org/) by running `npm install` in your application's directory (where the [manifest](Manifest-format) is), because `npm` would automatically put these modules in the `/node_modules` subdirectory.

### Example: async
Here is an example of installing and using `async` module:

```bash
$ cd /path/to/your/app
$ npm install async
```

Here is the list of files in the whole tree:

```bash
$ find .
.
./package.json
./index.html
./node_modules
./node_modules/async
./node_modules/async/.gitmodules
./node_modules/async/package.json
./node_modules/async/Makefile
./node_modules/async/LICENSE
./node_modules/async/README.md
./node_modules/async/.npmignore
./node_modules/async/lib
./node_modules/async/lib/async.js
./node_modules/async/index.js
```

Your application's manifest (`package.json`) may then look like the following:
```json
{
  "name": "nw-demo",
  "main": "index.html"
}
```

And here's an example of `index.html`:
```html
<html>
<head>
<title>test</title>
<script>
var async=require('async');
</script>
</head>
<body>
test should be here.
</body>
</html>
```

## 3rd party modules with C/C++ addons

For modules containing [C/C++ addons](http://nodejs.org/docs/latest/api/addons.html) the situation is slightly different because the ABI (application binary interface) of node-webkit differs from Node's ABI.

When such a module is installed for Node (with `npm install` command), `npm` uses its internal version of the [`node-gyp`](https://github.com/TooTallNate/node-gyp) tool to build the addons (from their source code) for Node.js.

To build such a module for node-webkit, [`nw-gyp`](https://github.com/rogerwang/nw-gyp) (a special fork of node-gyp) is necessary. You also have to run `nw-gyp` manually, because `npm` (being a Node's tool) does not know anything about building for node-webkit and using nw-gyp.

To install nw-gyp, run `npm install nw-gyp -g`.

To run nw-gyp, please meet its [requirements](https://github.com/rogerwang/nw-gyp#installation) (you'll need a proper Python engine and C/C++ compiler). These requirements are not different from node-gyp's.

To build a module for node-webkit, you may at first obtain it from an npm package, as if for Node.js (`npm install modulename`), but then rebuild it for node-webkit (`nw-gyp rebuild --target=0.5.0`).

Alternatively, you may get the module's source code elsewhere (for example, from GitHub) and run `nw-gyp rebuild --target=0.5.0` on it.

In both of these alternatives,

* nw-gyp must be run in the module's directory (where the module's `binding.gyp` resides),

* the necessary target version of node-webkit (such as `0.5.0` in `nw-gyp rebuild --target=0.5.0`) must be given explicitly, because nw-gyp cannot autodetect it. (You should also rebuild a module for any newer version of node-webkit, because the ABI is not stable.)

The built C/C++ addons of Node and node-webkit are mutually incompatible. For example, you cannot use `npm test` to test an addon-containing module in Node, if that addon is built for node-webkit: the test will always fail (either with some cryptic message or with a silent crash of the whole engine).

For more information on that matter (including further limitations and known issues), see “[[Build native modules with nw gyp]]”.
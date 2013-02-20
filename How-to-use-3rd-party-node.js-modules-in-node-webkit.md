You can install the 3rd party modules in your application's directory and then just call `require()` from your code in HTML. The path /path/to/your/app/node_modules is in the module search path of node-webkit.

The built-in Node modules such as `fs` can be used directly without these steps.

Native modules should be built by [nw-gyp](https://npmjs.org/package/nw-gyp) rather than `node-gyp`, which `npm` uses by default. For more information, see [[Build native modules with nw gyp]].

Here is an example of loading `async` module:
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
package.json:
```json
{
  "name": "nw-demo",
  "main": "index.html"
}
```
index.html:
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
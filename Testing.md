Now node-webkit's testing works have to parts: automatic tests and manual test. Node-webkit uses [Mocha](http://visionmedia.github.io/mocha/) to run automatice tests.

## node-webkit tests

[[How to run node-webkit's test cases]]

## How do we test with Mocha

In folder `automatic_test`, every subdirectory has a file called `mocha_test.js`. In `mocha_test.js`, there are test cases writed using mocha. Finally we load these test case and run them in `index.html`.

## How to write test case for node-webkit

Mocha is easy to use, you can directly wirte mocha code in `mocha_test.js`. But if you have a simple app and want to ues it, there are some traps.

Since you need to spawn a new process for the app, but there would be problem using the communication method of node.js's `child_process` modoule. So you need to use other method for the data transporting like using socket.

In node-webkit testing system, we have implement a simple module `nw_test_app` for the job. How to use it:

`mocha_test.js`:

```javascript
var child = app_test.createChildProcess({
  execPath: process.execPath,
  appPath: 'path_to_app',
  end: function(data, app) {
    if (data.ok) {
      done();
    } else {
      done('error');
    }
  }
});
```

`app`:

```javascript
var client = require('nw_test_app').createClient({
  argv: gui.App.argv,
  data: {ok : true},
});
```



Before you start developing an express-based NW.js(node-webkit) application, you should see [About Node.js server side script in nw.js](https://github.com/nwjs/nw.js/wiki/About-Node.js-server-side-script-in-nw.js), as it provides some good ways to replace those frameworks.

But if you already have an express-based webapp, and you want NW.js to use it as desktop app, you might encounter problems.

First, express provide a web server and doesn't have an `index.html` file, so what should you do with the `package.json` `main` property?

The solution is that you can create a `index.html` file, and use the following code to run the system:

```<script type="text/javascript" src="app.js"></script>```

In NW.js you can use node modules in html files, for more details on this, see: [Using Node modules](https://github.com/nwjs/nw.js/wiki/Using-Node-modules)

Second, express 3.x/4.x use the `./bin/www` folder to start a web server, so if you run the `app.js` it will fail to start. If you want run a node app directly, not through a html file, you can use the following way.

We can setup `node-main` in `package.json`. The `node-main` property is a command which will be call when NW.js app start, for more details see: [Node main](https://github.com/nwjs/nw.js/wiki/Node-main).  
And for express, we should also setup the `main` property to `http://localhost:3000`. If we setup a filename to that, NW.js will open it with the file protocol, so you will see the source code. If you setup a url with http protocol, NW.js will open it just like a browser.

```
"node-main": "./bin/www",
"main": "http://localhost:3000"
```

Put the above code in your package.json and run it. It seems OK but there is a problem (maybe a bug), in that NW.js shows a blank page. You must refresh the page, and then the content will show.  
There is a way to solve this problem: you can create a html file and write JavaScript code `location.href="http://localhost:3000/"` in it, then setup the `main` as `your-dir-html.html`.

If the application that you're working with doesn't contain a www binary, try something like this:

```
"node-main": "./app.js",
"main": "http://localhost:3000"
```
This worked with the mean-stack-relational application (https://github.com/jpotts18/mean-stack-relational) which doesn't have a www binary.
`node-main` is a field in the [manifest format](Manifest-format), with which you can specify the path to a Javascript file to run on startup. It is treated in the same way as the 'main module' you would run in Node.js.

The script will be running in Node's context which won't be destructed across page navigation in Webkit. So it can be used to write some 'background' or 'daemon' like code.

Besides Node's symbols, the following symbols is available in Node's context:

*  **window**: defined as a property of 'global', points to the DOM window global object. Note that it would be updated upon page navigation.

Since the `node-main` script is the main module of Node.js, it can be referred from DOM context with `process.mainModule`.
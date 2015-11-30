*nw-gyp is supported from version 0.3.2*

nw-gyp is a hack on node-gyp to support node-webkit specific headers and libraries. 

The usage is the same with node-gyp, except that you need to specify the version of node-webkit manually. 

````bash
$ npm install -g nw-gyp
$ cd myaddon
$ nw-gyp configure --target=0.12.2
$ nw-gyp build
````

Please see https://github.com/nwjs/nw-gyp for more details.

Note: you do not need to build NW.js, Chromium or Node.js from source in order to use nw-gyp; because when calling `nw-gyp configure --target=<version>` it will download and cache the headers you need (`v8.h` and `node.h`) for that version of NW.js.

For some packages you may need to use [node-pre-gyp](https://github.com/mapbox/node-pre-gyp) (e.g. when you get the error "Undefined variable module_name in binding.gyp while trying to load binding.gyp"), which supports building for both node.js and node-webkit by using either node-gyp or nw-gyp.

````bash
$ npm install node-pre-gyp
$ node-pre-gyp build --runtime=node-webkit --target=0.3.3
````

## Known issues and limitations

**NOTE**: The V8 version in node-webkit is different with the version in Node.js. And because V8 doesn't have any stable ABI, your native module could be broken. But we are trying our best to support native modules, deprecated API is supported to maintain backward compatibility. And if you find your module can't be built, file an issue.

On Windows, the executable file `nw.exe` cannot be renamed, or the native module cannot be loaded since it will try to find `nw.exe`. See https://github.com/nwjs/nw.js/issues/199

On OS X, Webkit is only 32-bit (i386) but newer versions of Node are build as 64-bit (x86_64) so you might need ensure libraries your module links to are universal builds.
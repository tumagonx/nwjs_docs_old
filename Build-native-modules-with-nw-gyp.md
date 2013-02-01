*nw-gyp is supported from version 0.3.2*

nw-gyp is a hack on node-gyp to support node-webkit specific headers and libraries. 

The usage is the same with node-gyp, except that you need to specify the version of node-webkit manually. 

**NOTE**: native module is supposed to be rebuilt with newer version of node-webkit, because V8 doesn't have any stable ABI.

````bash
$ npm install nw-gyp
$ nw-gyp configure --target=0.3.3
$ nw-gyp build
````

Please see https://github.com/rogerwang/nw-gyp for more details.
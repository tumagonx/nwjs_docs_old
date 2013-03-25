*nw-gyp is supported from version 0.3.2*

nw-gyp is a hack on node-gyp to support node-webkit specific headers and libraries. 

The usage is the same with node-gyp, except that you need to specify the version of node-webkit manually. 

````bash
$ npm install nw-gyp
$ nw-gyp configure --target=0.3.3
$ nw-gyp build
````

Please see https://github.com/rogerwang/nw-gyp for more details.

## Known issues and limitations

**NOTE**: native module is supposed to be rebuilt with newer version of node-webkit, because V8 doesn't have any stable ABI.

On Windows, the executable file `nw.exe` cannot be renamed, or the native module cannot be loaded since it will try to find `nw.exe`. See https://github.com/rogerwang/node-webkit/issues/199

On OS X, Webkit is only 32-bit (i386) but newer versions of Node are build as 64-bit (x86_64) so you might need ensure libraries your module links to are universal builds.
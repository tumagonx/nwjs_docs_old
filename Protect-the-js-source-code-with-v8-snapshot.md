_Since v0.4.2_

**Note that this feature is still experimental. Please expect changes (of interface and usage) in future versions.**

The JavaScript source code of your application can be protected by compiling to native code. Only the native code is distributed with the application and is loaded when the application starts.

There are important limitations in the current implementation. Please see the last section.

## Compilation

JS source code is compiled to native code with the tool `nwsnapshot`, which is provided in the binary download of `node-webkit`. To use it:

````bash
nwsnapshot --extra_code source.js snapshot.bin
````
The `snapshot.bin` file is needed to be distributed with your application. You can name it whatever you want.

## Package

Add the following field to `package.json`:
````json
"snapshot" : "snapshot.bin"
````

## Limitation

The source code being compiled cannot be too big. `nwsnapshot` will report error when this happens.

The compiled code runs slower than normal JS: ~30% performance according to v8bench.

The compiled code is not cross-platform or compatible between versions of node-webkit. So you'll need to run `nwsnapshot` for each of the platforms when you package your application.
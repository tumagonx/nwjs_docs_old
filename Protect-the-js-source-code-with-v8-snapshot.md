_Since v0.4.2_

**This feature is still experimental. Please expect changes (of interface and usage) in future versions.**

The JavaScript source code of your application can be protected by compiling to native code. Only the native code is distributed with the application and is loaded when the application starts.

There are important limitations in the current implementation. Please see the last section.

## Compilation

JS source code is compiled to native code (aka. 'snapshot') with the tool `nwsnapshot`, which is provided in the binary download of `node-webkit`. To use it:

````bash
nwsnapshot --extra_code source.js snapshot.bin
````
The `snapshot.bin` file is needed to be distributed with your application. You can name it whatever you want.

## Package

Add the following field to `package.json`:
````json
"snapshot" : "snapshot.bin"
````

## Run

It's important to remember that the compiled code runs **when you launch `nwsnapshot`**. Then the JS heap state is saved to the binary file (e.g. `snapshot.bin`) and restored right before your application launches. So it's better to just define functions or variables there.

And the scripts runs/loads loads very early (you can assume it's earlier than context creation) so DOM objects such as `window` is not defined. So you may want to defined functions and pass `window` as argument.

The snapshot is a kind of 'template' used to create JS contexts. So the objects defined there will be in every JS contexts.

## Limitation

The source code being compiled **cannot be too big**. `nwsnapshot` will report error when this happens.

The compiled code runs **slower than normal JS**: ~30% performance according to v8bench. Other JS source code will not be affected.

The compiled code is **not cross-platform nor compatible between versions** of node-webkit. So you'll need to run `nwsnapshot` for each of the platforms when you package your application.
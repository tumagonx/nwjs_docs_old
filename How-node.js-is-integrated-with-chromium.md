`node.js` itself is not well prepared for being integrated into another project, so it would take some jobs to make it run in `Chromium`.

## Replace Chromium's Message Pump

`Chromium` internally use class `MessageLoop` and `MessagePump` to support its events loop, since `node` uses `libuv` to support its events loop, we need to implement a new `MessagePump` which uses `libuv` as its underlying events library.

See `base/message_pump_uv.cc`, `base/message_pump_uv.cc`, `base/message_loop.cc` and `base/message_loop.h` to understand our work.

On Mac things are a lot different, see `base/message_pump_mac.h` and `base/message_pump_mac.mm`.

## Modify node

Though `node` has a `Start` function that setups everything,it will redirect everything into its own message loop, so we split the `Start` function into several parts, see `third_party/node/src/node.cc`.

And since `node` itself expects to execute a script file, there are many logics that deal with script execution in `third_party/node/src/node.js`, we heavily modified that part and used some tricks to make its module system run under `node-webkit`.

Another modification resided under `third_party/WebKit/Source/WebCore/bindings/v8/V8DOMWindowShell.cpp` in line 363, we inserted some script after WebKit is initialized, so `node` can get a right `pathname` and `filename` which is vital to `require` function.

## Insert node's symbols into webkit

This is the most important and trickiest part of `node-webkit`, first we initialize a context for `node`, and make `node` setup all its stuff under it, see `content/renderer/renderer_main.cc`, then when WebKit has installed DOM into its context, we move everything from `node` to `WebKit`, see `third_party/WebKit/Source/WebCore/bindings/v8/V8DOMWindowShell.cpp` in line 346.

A potential problem is that all node's callbacks will be executed under node's context, since the message loop runs under node's context. But all codes from `<script>` tag will run under WebKit's context, this could cause some problems since same codes under different contexts.

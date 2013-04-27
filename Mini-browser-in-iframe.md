node-webkit has special support to let you load external websites in an iframe in your application. 2 attributes `nwdisable` and `nwfaketop` of the iframe element are introduced to do this:

* `nwdisable` (since 0.5.0) is used to disable Node support in the iframe and make it a `Normal frame` (see [wiki/Security])
* `nwfaketop` (since 0.5.1) is used to trap the navigation and the access (such as window.top, window.parent) in this iframe.

For more discussion, see https://github.com/rogerwang/node-webkit/issues/534
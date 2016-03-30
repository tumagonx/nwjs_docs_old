**NOTE: some content in this wiki applies only to 0.12 and earlier versions. For official documentation on 0.13 and later, see http://docs.nwjs.io**

**This feature is deprecated since 0.13.0. See [migration note](http://docs.nwjs.io/en/latest/For%20Users/Migration/From%200.12%20to%200.13/) for details.**

_Following content requires node-webkit >= v0.2.5_

In order to expose some internal information about node-webkit in a easy way, we provide `nw:` protocol which is roughly the same with `about:` protocol in chrome. Currently we have the following internal pages available:

### nw:blank

A blank page showing `node-webkit` logo; instead of showing `about:blank`, we display this page when no app is loaded.

### nw:version

Displays the node-webkit and node.js version strings.
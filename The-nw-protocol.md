_Following content requires node-webkit >= v0.2.5_

In order to expose some internal information about node-webkit in a easy way, we provide `nw:` protocol which is roughly the same with `about:` protocol in chrome. Currently we have follow internal pages:

### nw:blank

A blank page showing `node-webkit` logo, it's shown when no app is loaded instead of `about:blank`.

### nw:version

Showing node-webkit and node's versions.
_since v0.7.0_

The app protocol is defined like http protocol: `app://<host>/path`. The host part is essential. You can define it to anything you want. The root of `path` refers to the application's directory, which is the directory where the manifest file resides.

It's provided for the ease of migrating files from your web site, e.g. repackage your web site as a node-webkit application.

Regarding [security](Security), it's treated as local file protocol and have access to Node functionality. 
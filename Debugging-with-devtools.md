**NOTE: some content in this wiki applies only to 0.12 and earlier versions. For official documentation on 0.13 and later, see http://docs.nwjs.io**

*Note: following guides only apply to node-webkit >= v0.3.0*

## Open Developer Tools

In order to show the `devtools` button in toolbar, you should edit package.json to make sure your window shows the toolbar:

```json
{
  "window": {
    "toolbar": true
  }
}
```

Then you can open devtools from the `devtools` button (the one on the right of url entry) in the toolbar.

Alternatively, even when this button (or the toolbar as a whole) is not visible, you may open devtools programmatically (calling the NW.js's [`require('nw.gui').Window.get().showDevTools()`](Window#windowshowdevtools) method from one of your scripts). **Note:** On NW.js 0.13.0+ the command is `nw.Window.get().showDevTools()`.

**Requirement:** On Windows and Linux, you need to make sure `nw.pak` is in the same directory with `nw` (Linux) or `nw.exe` (Windows).

## Remote Debugging

You can use the `--remote-debugging-port=port` command parameter to specify which port the devtools should listen to. For example, by running `nw --remote-debugging-port=9222`, you can open `http://localhost:9222/` to visit the debugger remotely.

## Bugs of Developer Tools

Currently not everything of developer tools is working well, below are the things that don't work:

* node modules don't show in script sources

## Why does devtools show an empty window?

First off, ensure you are using the `SDK` version of NW.js. The `Normal` version can still launch the Chromium developer tools window, however it will be empty.

Under certain network settings, it may show a white page for devtools. This might not be a bug of node-webkit, devtools in node-webkit is indeed a remote debugger, it needs to open a local server and transfer data via sockets.

So if you encounter empty window when opening the devtools, please check following things:

* `nw.pak` should be in the same directory with `nw.exe`.
* Your proxy settings: bypass localhost in the settings.

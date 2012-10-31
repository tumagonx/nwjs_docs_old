*Note: following guides only apply to node-webkit >= v0.3.0*

## Open Developer Tools

In order to show the `devtools` button in toolbar, you should make sure your window shows the toolbar:

```json
{
  "window": {
    "toolbar": true
  }
}
```

Then you can open devtools from the `devtools` button (the one on the right of url entry) in the toolbar.

**Note:** On Windows and Linux, you need to make sure `nw.pak` is in the same directory with `nw`(Linux) or `nw.exe` (Window)

## Remote Debugging

You can use the `--remote-debugging-port=port` command parameter to specify which port the devtools should listen to. For example, by running `nw --remote-debugging-port=9222`, you can open `http://localhost:9222/` to visit the debugger remotely.

## Bugs of Developer Tools

Currently not everything of developer tools is working well, bellow are the things that don't work:

* node modules don't shown in script sources

## Why the devtools shows an empty window?

Under certain Windows machines, the devtools loads very slow, it may show a white page at first and needs about 30s to be fully loaded. This is not a bug of node-webkit, devtools in node-webkit is indeed a remote debugger, it needs to open a local server and transfer data via sockets.

So if you encounter empty window when opening the devtools, please check following things:

* `nw.pak` should be in the same directory with `nw.exe`.
* Your proxy settings.
* Antivirus or firewall software.
* Check if you boot your Windows VM in VMWare Fusion mode.

If you still have problems after making sure nothing is slowing down the devtools, than you have to wait until devtools is fully loaded.
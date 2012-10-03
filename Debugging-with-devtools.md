*Note: following guides only apply to node-webkit >= v0.2.2*

## Open Developer Tools

In order to show the `Debug` menu, you should manually append an `developer` switch:

````bash
$ nw --developer
````

Note, on Windows the command parameter is a little different:

````
nw /developer
````

Then when you open the developer tools from the application menu `Debug` -> `Show DevTools`.

## Bugs of Developer Tools

Currently not everything of developer tools is working well, bellow are the things that don't work:

* node modules don't shown in script sources

## Why the devtools shows an empty window?

Under certain Windows machines, the devtools loads very slow, it may show a white page at first and needs about 30s to be fully loaded. This is not a bug of node-webkit, devtools in node-webkit is indeed a remote debugger, it needs to open a local server and transfer data via sockets.

So if you encounter empty window when opening the devtools, please check following things:

* Your proxy settings.
* Antivirus or firewall software.
* Check if you boot your Windows VM in VMWare Fusion mode.

If you still have problems after making sure nothing is slowing down the devtools, than you have to wait until devtools is fully loaded.
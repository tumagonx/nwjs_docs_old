## Open Developer Tools

`node-webkit` supports only remote debugging now, so in order to use chrome's developer tools on `node-webkit`, you need to specify an empty port on startup:

````bash
$ nw --remote-debugging-port=8259
````

Note, on Windows the command parameter is a little different:

````
nw /remote-debugging-port=8259
````

Then when you open the developer tools from the context menu item `Show DevTools`, most things functions well.

## Bugs of Developer Tools

Currently not everything of developer tools is working well, bellow are the things that don't work:

* node modules don't shown in script sources
* render process crashed when the debugger continues execution
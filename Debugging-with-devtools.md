*Note: following guides only apply to node-webkit >= v0.2.2*

## Open Developer Tools

In order to show the `Debug` menu, you should manually append an `developer` switch:

````bash
$ nw --developer`
````

Note, on Windows the command parameter is a little different:

````
nw /developer
````

Then when you open the developer tools from the application menu `Debug` -> `Show DevTools`.

## Bugs of Developer Tools

Currently not everything of developer tools is working well, bellow are the things that don't work:

* node modules don't shown in script sources
* render process crashed when the debugger continues execution

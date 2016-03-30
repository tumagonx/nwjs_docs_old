**NOTE: some content in this wiki applies only to 0.12 and earlier versions. For official documentation on 0.13 and later, see http://docs.nwjs.io**

_Shell API requires node-webkit >= v0.3.0_

`Shell` is a collection of APIs that do desktop related jobs.

## Synopsis

```javascript
// Load native UI library.
var gui = require('nw.gui');

// Open URL with default browser.
gui.Shell.openExternal('https://github.com/rogerwang/node-webkit');

// Open a text file with default text editor.
gui.Shell.openItem('test.txt');

// Open a file in file explorer.
gui.Shell.showItemInFolder('test.txt');
```

## Reference

### openExternal(URI)
Open the given external protocol `URI` in the desktop's default manner. (For example, mailto: URLs in the default mail user agent.)

### openItem(file_path)
Open the given `file_path` in the desktop's default manner.

### showItemInFolder(file_path)
Show the given `file_path` in a file manager. If possible, select the file.
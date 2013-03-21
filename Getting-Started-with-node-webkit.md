This chapter is contains some tutorial information to get you started with node-webkit programming. It assumes that you have node-webkit binaries. If you want to build node-webkit itself first, refer to the [[Building node-webkit]] section.

Since node-webkit is based on [Chromium](www.chromium.org) and node.js. It let us call node.js code and modules directly from the DOM, and using all Web technologies for your apps. So we can easily package a web application to a native application.

## Basics

To begin our introduction to node-webkit, we'll start with the simplest program possible.

**Example 1. Hello World**

![Capture3](https://f.cloud.github.com/assets/2891424/279516/5fba0cca-912b-11e2-983d-c2e8a66c3706.PNG)

Create `index.html`:

```html
<html>
<head>
<title>Hello World!</title>
</head>
<body>
<h1>Hello World!</h1>
</body>
</html>
```

Create `package.json`:

```json
{
  "name": "nw-demo",
  "main": "index.html"
}
```

Compress `index.html` and `package.json` into a zip archive, and rename
it to `app.nw`:

    app.nw
    |-- package.json
    `-- index.html

Download the prebuilt binary for your platform and use it to open the
`app.nw` file:

```bash
$ ./nw app.nw
```

Note: on Windows, you can drag the `app.nw` to `nw.exe` to open it.



**Example 2. Native UI API**

![Capture4](https://f.cloud.github.com/assets/2891424/279875/e8572dd0-913d-11e2-8a82-ea021ca07ce6.PNG)

There are APIs for native UI controls in node-webkit. You can use these for controlling window, menu, etc.

The following example shows how to use menu.

```html
<html>
<head>
  <title> Menu </title>
</head>
<body>
<script>
// Load native UI library
var gui = require('nw.gui');

// Create an empty menu
var menu = new gui.Menu();

// Add some items with label
menu.append(new gui.MenuItem({ label: 'Item A' }));
menu.append(new gui.MenuItem({ label: 'Item B' }));
menu.append(new gui.MenuItem({ type: 'separator' }));
menu.append(new gui.MenuItem({ label: 'Item C' }));

// Remove one item
menu.removeAt(1);

// Iterate menu's items
for (var i = 0; i < menu.items.length; ++i) {
  console.log(menu.items[i]);
}

// Add a item and bind a callback to item
menu.append(new gui.MenuItem({
label: 'Click Me',
click: function() {
  // Create element in html body
  var element = document.createElement('div');
  element.appendChild(document.createTextNode('Clicked OK'));
  document.body.appendChild(element);
}
}));

// Popup as context menu
document.body.addEventListener('contextmenu', function(ev) { 
  ev.preventDefault();
  // Popup at place you click
  menu.popup(ev.x, ev.y);
  return false;
}, false);

// Get the current window
var win = gui.Window.get();

// Create a menubar for window menu
var menubar = new gui.Menu({ type: 'menubar' });

// Create a menuitem
var sub1 = new gui.Menu();


sub1.append(new gui.MenuItem({
label: 'Test1',
click: function() {
  var element = document.createElement('div');
  element.appendChild(document.createTextNode('Test 1'));
  document.body.appendChild(element);
}
}));

// You can have submenu!
menubar.append(new gui.MenuItem({ label: 'Sub1', submenu: sub1}));

//assign the menubar to window menu
win.menu = menubar;

</script>  
</body>
</html>
```

**Example 3. Using node.js**

You can call node.js and modules directly fomr the DOM. So it enable endless possibilities for writing apps with node-webkit.

```html
<html>
<body>
<script>
// get the system platformu sing node.js
var os = require('os')
document.write('Our computer is: ', os.platform())
</script>
</body>
</html>
```


## Run and Package Apps

Now, we can write simple node-webkit apps. Next is to learn how to run and package them. 

**Run the App**

There are two general ways to run apps for all platform.

* From a folder. The startup path specifies this folder.
* From a .nw file (a renamed .zip-file). The startup path specifies the file.

For Example:

````bash
nw path_to_app_dir
nw path_to_app.nw
````

Go back to [Wiki](https://github.com/rogerwang/node-webkit/wiki) for much more.
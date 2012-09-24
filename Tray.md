_Tray API requires node-webkit >= v0.3.0_

`Tray` is an abstraction of different controls on different platforms, usually it's a small icon shown on the OS's notification area. On Mac it's called `Status Item`, on GTK it's `Status Icon`, and on Windows it's `System Tray Icon`.

## Synopsis

```javascript
// Load native UI library
var gui = require('nw.gui');

// Create a tray icon
var tray = new gui.Tray({ title: 'Tray', icon: 'img/icon.png' });

// Give it a menu
var menu = new gui.Menu({ title: 'Menu' });
menu.append({ label: 'Item' });
tray.menu = menu;

// Remove the tray
tray.remove();
tray = null;
```

## Reference

### new Tray(option)

Create a new `Tray`, `option` is an object contains initial settings for the `Tray`. `option` can have following fields: `title`, `tooltip`, `icon` and `menu`.

Every field has its own property in the `Tray`, see documentation of each property for details.

### Tray.title

**Get** or **Set** the `title` of `Tray`.

On Mac `title` will be showed on status bar along with its `icon`, but it doesn't have effects on GTK and Windows, since the latter two platforms only support tray to be showed as icons.

### Tray.tooltip

**Get** or **Set** the `tooltip` of `Tray`. `tooltip` shows when you hover the `Tray` with mouse.

`tooltip` is showed on all three platforms.

### Tray.icon

**Get** or **Set** the `icon` of `Tray`, `icon` must a path to your icon file. It can be a relative path which points to an icon in your app, or an absolute path pointing to a file in user's system.

### Tray.menu

**Get** or **Set** the `menu` of `Tray`, `menu` will be showed when you click on the `Tray` icon.

On Mac, this is the only way to set the tray menu since you cannot listen to left and right click on `Tray`. While on GTK and Window, node-webkit will listen to the clicking on `Tray` and automatically show the `menu` if you set it.

However, if you choose to listen to left and right click event of `Tray` on GTK and Windows, the `menu` will not automatically be showed, and you need to take care of different behaviours on different platforms.

### Tray.remove()

Remove the tray.

Once removed, you will not be able to show it again and you should set your tray variable to `null`. This limitation is inherited from Cocoa, since we need to keep same behaviour on all platforms.

## See also

* [[Menu]]
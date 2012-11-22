_MenuItem API requires node-webkit >= 0.3.0_

`MenuItem` represents an item in a menu. A `MenuItem` can be a separator or a normal item which has label and icon. `MenuItem` is usually used with `Menu` together.

## Synopsis

```javascript
// Load native UI library
var gui = require('nw.gui');
var item;

// Create a separator
item = new gui.MenuItem({ type: 'separator' });

// Create a normal item with label and icon
item = new gui.MenuItem({
  type: "normal", 
  label: "I'm a menu item",
  icon: "img/icon.png"
});

// Or you can omit the 'type' field for normal items
item = new gui.MenuItem({ label: 'Simple item' });

// Bind a callback to item
item = new gui.MenuItem({
  label: "Click me",
  click: function() {
    console.log("I'm clicked");
  }
});

// You can have submenu!
var submenu = new gui.Menu();
submenu.append(new gui.MenuItem({ label: 'Item 1' }));
submenu.append(new gui.MenuItem({ label: 'Item 2' }));
submenu.append(new gui.MenuItem({ label: 'Item 3' }));
item.submenu = submenu;

// And everything can be changed at runtime
item.label = 'New label';
item.click = function() { console.log('New click callback'); };
```

## Reference

### new MenuItem(option)

Create new `MenuItem`, `option` is an object contains initial settings for the `MenuItem`. `option` can have following fields: `label`, `icon`, `tooltip`, `type`, `click`, `checked`, `enabled` and `submenu`.

Every field has its own property in the `MenuItem`, see documentation of each property for details.

### MenuItem.type

**Get** the `type` of a `MenuItem`, it can be `separator`, `checkbox` and `normal` for now.

A `MenuItem`'s type can be set only when you create it, it cannot be changed at runtime.

### MenuItem.label

**Get** or **Set** the `label` of a `MenuItem`, it can only be plain text for now.

### MenuItem.icon

**Get** or **Set** the `icon` of a `MenuItem`, it must a path to your icon file. It can be a relative path which points to an icon in your app, or an absolute path pointing to a file in user's system.

It has no effect on setting `icon` of a `separator` item.

### MenuItem.tooltip

**Get** or **Set** the `tooltip` of a `MenuItem`, it can only be plain text. A `tooltip` is short string that describes the menu item, it will show when you hover your mouse on the item.

### MenuItem.checked

**Get** or **Set** whether the `MenuItem` is `checked`. Usually if a `MenuItem` is checked, there will be a mark on the left side of it. It's used mostly to indicate whether an option is on.

### MenuItem.enabled

**Get** or **Set** whether the `MenuItem` is `enabled`. An disabled `MenuItem` will be greyed out and you will not be able to click on it.

### MenuItem.submenu

**Get** or **Set** the `submenu` of a `MenuItem`, the `submenu` must be a `Menu` object. Developers had better set `submenu` in the `option` when creating the `MenuItem`, changing it at runtime is slow on some platforms.

### MenuItem.click

**Get** or **Set** the `click` callback of a `MenuItem`, the `click` must be a valid function, it will be called when users activate the item.

## Events
Following events can be listened by using `MenuItem.on()` function, for more information on how to receive events, you can visit [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

### click

Emitted when user activates the menu item.

## See Also

* [[Menu]]
_App API requires node-webkit >= v0.3.1_

## Synopsis

```javascript
// Load native UI library
var gui = require('nw.gui');

// Quit current app
gui.App.quit();
```

## Reference

### quit()

Quit current app.

### menu

**Get** or **Set** the application menu.

**Note:** Only Mac has the application menu, and node-webkit will automatically merge your menu with the standard original application menu. For example, following code will create a simplest application menu:

```javascript
var gui = require('nw.gui');

gui.App.menu = new gui.Menu({ type: 'menubar' });
```

And unlike the original application menu of node-webkit, the newly created one will change all `node-webkit` string to `your-app-name`.

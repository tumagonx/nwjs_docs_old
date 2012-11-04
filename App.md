_App API requires node-webkit >= v0.3.1_

## Synopsis

```javascript
// Load native UI library
var gui = require('nw.gui');

// Print arguments
console.log(gui.App.argv);

// Quit current app
gui.App.quit();
```

## Reference

### argv

**Get** the command line arguments when starting the app.

### closeAllWindows()

Send the `close` event to all windows of current app, if no window is blocking the `close` event, then the app will quit after all windows have done shutdown. Use this method to quit an app will give windows a chance to save data.

### quit()

Quit current app. This method will **not** send `close` event to windows and app will just quit quietly.
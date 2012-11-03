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

### quit()

Quit current app.

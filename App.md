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

### fullArgv

**Get** all the command line arguments when starting the app. Because node-webkit itself used switches like `--no-sandbox` and `--process-per-tab`, it would confuse the app when the switches were meant to be given to node-webkit, so `App.argv` just filtered such switches (arguments' precedence were kept). You can get the switches to be filtered with `App.filteredArgv`.

### closeAllWindows()

_Available after node-webkit v0.3.2_

Send the `close` event to all windows of current app, if no window is blocking the `close` event, then the app will quit after all windows have done shutdown. Use this method to quit an app will give windows a chance to save data.

### quit()

Quit current app. This method will **not** send `close` event to windows and app will just quit quietly.

## Events

Following events can be listened by using `App.on()` function, for more information on how to receive events, you can visit [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

### open

_Available after node-webkit v0.3.2_

Emitted when users opened a file with your app. For more on this, see [[Handling files]].
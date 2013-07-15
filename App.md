_Since v0.3.1_

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

### dataPath

_since v0.6.1_

**Get** the application's data path in user's directory. Windows: `%LOCALAPPDATA%/<name>`; Linux: `~/.config/<name>`; OSX: `~/Library/Application Support/<name>` where `<name>` is the field in the manifest.

### clearCache()

_Since v0.6.0_

Clear the HTTP cache in memory and the one on disk. This method call is synchronized.

### closeAllWindows()

_since v0.3.2_

Send the `close` event to all windows of current app, if no window is blocking the `close` event, then the app will quit after all windows have done shutdown. Use this method to quit an app will give windows a chance to save data.

### getProxyForURL(url)

_since v0.6.3_

Query the proxy to be used for loading `url` in DOM. The return value is in the same format used in [PAC](http://en.wikipedia.org/wiki/Proxy_auto-config) (e.g. "DIRECT", "PROXY localhost:8080").

### quit()

Quit current app. This method will **not** send `close` event to windows and app will just quit quietly.

## Events

Following events can be listened by using `App.on()` function, for more information on how to receive events, you can visit [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

### open

_Since v0.3.2_

Emitted when users opened a file with your app. For more on this, see [[Handling files and arguments]].
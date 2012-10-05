_Window API requires node-webkit >= v0.3.0_

`Window` is a wrapper of DOM's `window` object, it has extended operations and can receive various window events.

You can not create a new `Window`, you can only get a `Window` object by using `Window.get()`. Every `Window` is an instance of [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter) object, and you're able to use `Window.on(...)` to response to native window's events.

## Synopsis

```javascript
// Load native UI library
var gui = require('nw.gui');

// Get the current window
var win = gui.Window.get();

// Listen to the minimize event
win.on('minimize', function() {
  console.log('Window is minimized');
}

// Minimize the window
win.minimize();

// Unlisten the minimize event
win.removeAllListeners('minimize');

// Create a new window and get it
var new_win = gui.Window.get(
  window.open('https://github.com')
);

// And listen to new window's focus event
new_win.on('focus', function() {
  console.log('New window is focused');
});
```

## Reference

### get([window_object])

If `window_object` is not specifed, then return current window's `Window` object, otherwise return `window_object`'s `Window` object.

```javascript
// Get the current window
var win = gui.Window.get();

// Create a new window and get it
var new_win = gui.Window.get(
  window.open('https://github.com')
);
```

### Window.moveTo(x, y)

Moves a window's left and top edge to the specified coordinates.

### Window.moveBy(x, y)

Moves a window a specified number of pixels relative to its current coordinates.

### Window.resizeTo(width, height)

Resizes a window to the specified `width` and `height`.

### Window.resizeBy(width, height)

Resizes a window by the specified amount.

### Window.focus()

Focus on the window.

### Window.blur()

Move focus away. Usually it will move focus to other windows of your app, since on some platforms there is no concept of blur.

### Window.show()

Show the window if it's not showed, `show` will not focus on the window on some platforms, so you need to call `focus` if you want to.

`show(false)` has the same effect with `hide()`.

### Window.hide()

Hide the window. Users will not be able to find the window if it's hidden.

### Window.close([force])

Close current window, you can catch the `close` event to prevent the closing. If `force` is specified and equals to `true`, then the `close` event will be ignored.

Usually you would like to listen to the `close` event and do some shutdown work and then do a `close(true)` to really close the window.

```javascript
win.on('close', function() {
  console.log("We're closing...");
  this.close(true);
});

win.close();
```

### Window.maximize()

Maximize the window on GTK and Windows, zoom the window on Mac.

### Window.unmaximize()

Unmaximize the window, e.g. the reverse of `maximize()`.

### Window.minimize()

Minimize the window to taskbar on Windows, iconify the window on GTK, and miniaturize the window on Mac.

### Window.restore()

Restore window to previous state after the window is minimized, e.g. the reverse of `minimize()`. It's not named `unminimize` since `restore` is already used commonly on Window.

### Window.enterFullscreen()

Make the window fullscreen. This function is different with HTML5 FullScreen API, which can make part of the page fullscreen, `Window.enterFullscreen()` will only fullscreen the whole window.

### Window.leaveFullscreen()

Leave the fullscreen mode.

### Window.openDevTools()

Open the devtools to inspect the window.

## Events

Following events can be listened by using `Window.on()` function, for more information on how to receive events, you can visit [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

### close

The `close` event is a special event that will affect the result of the `Window.close()` function. If developer is listening to the `close` event of a window, the `Window.close()` call to that window will not close the window but send the `close` event.

Usually you would do some shutdown work in the callback of `close` event, and then call `this.close(true)` to really close the window, which will not be caught again. Forgetting to add `true` when calling `this.close()` in the callback will result in infinite loop.

For use case you can see demo code of `Window.close()` above or `Best Practice` bellow.

### focus

Emitted when window gets focus.

### blur

Emitted when window loses focus.

### minimize

Emitted when window is minimized.

### restore

Emitted when window is restored from minimize state.

### enter-fullscreen

Emitted when window enters fullscreen state.

### leave-fullscreen

Emitted when window leaves fullscreen state.

## Best Practice

### Popup 'Document is not saved' dialog on close

TODO

### Save window state on close and restore on startup

TODO

## See also

* [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter)

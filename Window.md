_Window API requires node-webkit >= v0.3.0_

`Window` is a wrapper of DOM's `window` object, it has extended operations and can receive various window events.

Every `Window` is an instance of [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter) object, and you're able to use `Window.on(...)` to response to native window's events.

## Synopsis

```javascript
// Load native UI library
var gui = require('nw.gui');

// Get the current window
var win = gui.Window.get();

// Listen to the minimize event
win.on('minimize', function() {
  console.log('Window is minimized');
});

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

### open(url[, options])

Open a new window and load `url` in it, you can specify extra `options` with the window. All window subfields in [[Manifest format]] can be used. Since v0.4.0, a boolean field `new-instance` can be used to start a new Node instance (webkit process).

```javascript
var win = gui.Window.open('https://github.com', {
  position: 'center',
  width: 901,
  height: 127
});
```

### Window.window

**Get** DOM's `window` object in the native window.

### Window.x/Window.y

**Get** or **Set** left/top offset from window to screen.

### Window.width/Window.height

**Get** or **Set** window's size.

### Window.title

**Get** or **Set** window's title.

### Window.isFullscreen

_since v0.3.5_

**Get** or **Set** whether we're in fullscreen mode.

### Window.isKioskMode

_since v0.3.5_

**Get** or **Set** whether we're in kiosk mode.

### Window.zoom
_since v0.4.1_

**Get** or **Set** the page zoom. `0` for normal size; positive value for zooming in; negative value for zooming out.

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
  this.hide(); // Pretend to be closed already
  console.log("We're closing...");
  this.close(true);
});

win.close();
```

### Window.reload()

_Requires node-webkit >= v0.3.5_

Reloads the current window.

### Window.reloadIgnoringCache()

_Requires node-webkit >= v0.3.5_

Like reload(), but don't use caches (aka "shift-reload").

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

### Window.toggleFullscreen()

_Requires node-webkit >= v0.3.5_

Toggle the fullscreen mode.

### Window.enterKioskMode()

_Requires node-webkit >= v0.3.1_

Enter the Kiosk mode. In Kiosk mode, the app will be fullscreen and try to prevent users from leaving the app, so you should remember to provide a way in app to leave Kiosk mode. This mode is mainly used for presentation on public displays.

### Window.leaveKioskMode()

_Requires node-webkit >= v0.3.1_

Leave the Kiosk mode.

### Window.toggleKioskMode()

_Requires node-webkit >= v0.3.5_

Toggle the kiosk mode.

### Window.showDevTools(id)

Open the devtools to inspect the window.

The `id` argument is supported since v0.6.0. It should be the value of `id` attribute of any `iframe` element in the window. It can be used to limit the devtools window to inspect only the iframe. For more information, please read [Devtools Jail Feature].

### Window.setMaximumSize(width, height)

Set window's maximum size.

### Window.setMinimumSize(width, height)

Set window's minimum size.

### Window.setResizable(Boolean resizable)

Set whether window is resizable.

### Window.setAlwaysOnTop(Boolean top)

_Requires node-webkit >= v0.3.4_

Sets the widget to be on top of all other windows in the windowing system.

### Window.setPosition(String position)

Shortcut to move window to specified `position`. Currently only `center` is supported on all platforms, which will put window in the middle of the screen.

### Window.requestAttention(Boolean attention)

Pass `true` to indicate that the window needs user's action, pass `false` to cancel it. The final behaviour depends on the platform.

### Window.capturePage(callback [, image_format]) 
_since v0.4.2_

Captures the visible area of the window. Note: a simple demo usage can be found here: https://gist.github.com/zhchbin/4749217 

`callback` should be a function that will be called when the the image data is ready. It looks like this:
```javascript
function(dataUrl) {...};
```
A `dataURL(string)` which encodes an image of the visible area of the captured
window. May be assigned to the 'src' property of an HTML Image element for display.

`image_format` is enumerated string ["jpeg", "png"]. Default is 'jpeg'.

## Events

Following events can be listened by using `Window.on()` function, for more information on how to receive events, you can visit [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

### close

The `close` event is a special event that will affect the result of the `Window.close()` function. If developer is listening to the `close` event of a window, the `Window.close()` call to that window will not close the window but send the `close` event.

Usually you would do some shutdown work in the callback of `close` event, and then call `this.close(true)` to really close the window, which will not be caught again. Forgetting to add `true` when calling `this.close()` in the callback will result in infinite loop.

And if the shutdown work takes some time, users may feel that the app is exiting slowly, which is bad experience, so you could just hide the window in the `close` event before really closing it to make a smooth user experience.

For use case you can see demo code of `Window.close()` above.

### closed

The `closed` event is emitted **after** corresponding window is closed. Normally you'll not be able to get this event since after the window is closed all js objects will be released. But it's useful if you're listening this window's events in another window, whose objects will not be released.

```html
<script>
  var gui = require('nw.gui');

  // Open a new window.
  var win = gui.Window.get(
    window.open('popup.html')
  );

  // Release the 'win' object here after the new window is closed.
  win.on('closed', function() {
    win = null;
  });

  // Listen to main window's close event
  gui.Window.get().on('close', function() {
    // Hide the window to give use the feeling of closing immediately
    this.hide();

    // If the new window is still open then close it.
    if (win != null)
      win.close(true);

    // After closing the new window, close the main window.
    this.close(true);
  });
</script>
```

### loading

_Requires node-webkit >= v0.3.5_

Emitted when the window starts to reload, normally you cannot catch this event because usually it's emitted before you actually setup the callback.

The only situation that you can catch this event is when you refresh the window and listen to this event in another window.

### loaded

_Requires node-webkit >= v0.3.5_

Emitted when the window is fully loaded, this event behaves the same with `window.onload`, but doesn't rely on the DOM.

### focus

Emitted when window gets focus.

### blur

Emitted when window loses focus.

### minimize

Emitted when window is minimized.

### restore

Emitted when window is restored from minimize state.

### maximize

Emitted when window is maximized.

### unmaximize

Emitted when window is restored from maximize state. Note that on some platforms window can be resized even when maximized, so the `unmaximize` may not be emitted when a maximized window is resized instead of being unmaximized

### enter-fullscreen

Emitted when window enters fullscreen state.

### leave-fullscreen

Emitted when window leaves fullscreen state.

### zoom

Emitted when window zooming changed. It has a parameter indicating the new zoom level. See `Window.zoom` for the parameter's value definition.

### capturepagedone
Emitted after the capturePage method is called and image data is ready. See `Window.capturePage's` callback function for the parameter's value definition. 

## Issues

Now on linux, the function `setMaximumSize()/setMinimumSize()` and `setResizable()` can not be set at the same time. If do so, they all do not work. Please taking care of this.

## See also

* [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter)
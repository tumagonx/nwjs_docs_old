_Screen API requires node-webkit >= v0.10.2_

`Screen` is an instance of [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter) object, and you're able to use `Screen.on(...)` to respond to native screen's events.

`Screen` is a singleton object, need to be initiated once by calling `gui.Screen.Init()`

## Reference

### Screen.Init()
Init the Screen singleton object, you only need to call this once

### Screen.screens
return the array of screen

screen has following structure:
```javascript
screen {
  id : int,   // unique id for a screen
  bounds : {  // physical screen resolution, can be negative, not necessarily start from 0,depending on screen arrangement
    x : int,
    y : int,
    width : int,
    height : int
  },
  work_area : { // useable area within the screen bound (due to native OS taskbar)
    x : int,
    y : int,
    width : int,
    height : int
  },
  scaleFactor : float,
  isBuiltIn : bool
}
```
## Events

Following events can be listened by using `Screen.on()` function, for more information on how to receive events, you can visit [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

### displayBoundsChanged

emitted when the screen resolution, arrangement is changed

### displayAdded

emitted when a new screen added

### displayRemoved

emitted when existing screen removed

## Synopsis
```javascript
function ScreenToString(screen) {
  var string = "";
  string += "screen " + screen.id + " ";
  var rect = screen.bounds;
  string += "bound{" + rect.x + ", " + rect.y + ", " + rect.width + ", " + rect.height + "} ";
  rect = screen.work_area;
  string += "work_area{" + rect.x + ", " + rect.y + ", " + rect.width + ", " + rect.height + "} ";
  string += " scaleFactor: " + screen.scaleFactor;
  string += " isBuiltIn: " + screen.isBuiltIn;
  string += "<br>";
  return string;
}

//init must be called once during startup, before any function to gui.Screen can be called
gui.Screen.Init();
var string = "";
var screens = gui.Screen.screens;
// store all the screen information into string
for(var i=0; i<screens.length; i++) {
  string += ScreenToString(screens[i]);
}

var screenCB = {
  onDisplayBoundsChanged : function(screen) {
    var out = "OnDisplayBoundsChanged " + ScreenToString(screen);
  },

  onDisplayAdded : function(screen) {
    var out = "OnDisplayAdded " + ScreenToString(screen);
  },

  onDisplayRemoved : function(screen) {
    var out = "OnDisplayRemoved " + ScreenToString(screen);
  }
};

// listen to screen events
gui.Screen.on('displayBoundsChanged', screenCB.onDisplayBoundsChanged);
gui.Screen.on('displayAdded', screenCB.onDisplayAdded);
gui.Screen.on('displayRemoved', screenCB.onDisplayRemoved);
```
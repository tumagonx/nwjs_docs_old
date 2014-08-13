_Screen API requires node-webkit >= v0.10.2_

`Screen` is an instance of [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter) object, and you're able to use `Screen.on(...)` to respond to native screen's events.

`Screen` is a singleton object, need to be initiated once by calling `gui.Screen.Init()`


screen has following structure:
```javascript
screen {
  id : int,
  bounds : { 
    x : int,
    y : int,
    width : int,
    height : int
  },
  work_area : { 
    x : int,
    y : int,
    width : int,
    height : int
  },
  scaleFactor : float,
  isBuiltIn : bool
}
```
How to use it?
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

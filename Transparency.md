_Transparency requires node-webkit >= v0.11.2_

in your html body, specify the alpha of the background colour, in my example below, I set it to be full transparent
```HTML
<body style="background-color:rgba(0,0,0,0);">
```

you can set the window's transparency "on the fly" using
```javascript 
var gui = require('nw.gui');
var win = gui.Window.get();
win.setTransparent(!win.isTransparent);
```
you can also specify it in package.json
```json
  "window": {
    "transparent": true
  }
```
### Linux
you need to run node webkit with these params
```params
--enable-transparent-visuals --disable-gpu
```
and your window manager needs to support compositing

## Click through transparency
_Click through transparency requires node-webkit >= v0.11.4_
to enable Transparency clickthrough (currently only supported on OSX / WIN)
you must run with these args
```params
--disable-gpu --force-cpu-draw"
```
the click through is only supported for **frameless**, **non resizable** frame, though it might work for other configuration, depending on the OS
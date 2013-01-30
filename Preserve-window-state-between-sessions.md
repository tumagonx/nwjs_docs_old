First you need to lazily show the window until your app loads previous state:

```json
{
  "window": {
    "show": false
  }
}
```

Then you can load everything in the `onload` or `$(document).ready`, and save them by listening to `close` event:

```html
<html>
<body>
<script>
  var gui = require('nw.gui');
  var win = gui.Window.get();

  // Save size on close.
  win.on('close', function() {
    localStorage.x      = win.x;
    localStorage.y      = win.y;
    localStorage.width  = win.width;
    localStorage.height = win.height;
    this.close(true);
  });

  // Restore on startup.
  onload = function() {
    if (localStorage.width && localStorage.height) {
      win.resizeTo(parseInt(localStorage.width), parseInt(localStorage.height));
      win.moveTo(parseInt(localStorage.x), parseInt(localStorage.y));
    }

    win.show();
  };
</script>
</body>
</html>
```
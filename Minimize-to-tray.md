_This example needs node-webkit >= v0.3.0_

On Windows, a common (bad) window design pattern is to minimize the window to the notification area. e.g. Hide and window when you click the minimize button and add a tray icon to bring the window back.

To implement it in node-webkit, you can use the code bellow. But you should remember that this behaviour is not recommended in most times.

```html
<html>
<body>
  <div>Minimize to tray</div>
  <script>
    // Load library
    var gui = require('nw.gui');
    
    // Reference to window and tray
    var win = gui.Window.get();
    var tray;

    // Get the minimize event
    win.on('minimize', function() {
      // Hide window
      this.hide();

      // Show tray
      tray = new gui.Tray({ icon: 'icon.png' });

      // Show window and remove tray when clicked
      tray.on('click', function() {
        win.show();
        this.remove();
        tray = null;
      });
    });
  </script>
</body>
</html>
```
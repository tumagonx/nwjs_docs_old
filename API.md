# Window

## Use
    var win = gui.Window.get([window_object]);  // If window_object is not specifed, then return current window's Window object, otherwise return window_object's Window object.
    var win = gui.Window.open(url[, options]);  // Open a new window and load url in it, you can specify extra options with the window. All window subfields in Manifest format can be used.

## Properties
    win.window;                   // Get DOM's window object in the native window.
    win.x;                        // Get or Set left/top offset from window to screen.
    win.y;                        // Get or Set left/top offset from window to screen.
    win.width;                    // Get or Set window's size.
    win.height;                   // Get or Set window's size.
    win.title;                    // Get or Set window's title.
    win.menu;                     // Get or Set window's menubar. Set with a Menu with type menubar.
    win.isFullScreen;             // Get or Set whether we're in fullscreen mode.
    win.isKioskMode;              // Get or Set whether we're in kiosk mode.
    win.zoomLevel;                // Get or Set the page zoom. 0 for normal size; positive value for zooming in; negative value for zooming out.

# Window

## Use
    var win = gui.Window.get([window_object]);  
If window_object is not specifed, then return current window's Window object, otherwise return window_object's Window object.

    var win = gui.Window.open(url[, options]);  
Open a new window and load url in it, you can specify extra options with the window. All window subfields in Manifest format can be used.

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


## Methods

### Move To
Moves a window's left and top edge to the specified coordinates.

    win.moveTo(x, y);

### Move By
Moves a window a specified number of pixels relative to its current coordinates.

    win.moveBy(x, y);

### Resize To
Resizes a window to the specified width and height.

    win.resizeTo(width, height);

### Resize By
Resizes a window by the specified amount.

    win.resizeBy(width, height);

### Focus
Focus on the window.

    win.focus();                  

### Blur
Move focus away. Usually it will move focus to other windows of your app, since on some platforms there is no concept of blur.

    win.blur();                   

### Show
Show the window if it's not showed, show will not focus on the window on some platforms, so you need to call focus if you want to. show(false) has the same effect with hide().

    win.show();

win.hide();                   // Hide the window. Users will not be able to find the window if it's hidden.
win.close([force]);           // Close current window, you can catch the close event to prevent the closing. If force is specified and equals to true, then the close event will be ignored.
win.reload();                 // Reloads the current window.
win.reloadIgnoringCache();    // Like reload(), but don't use caches (aka "shift-reload").
win.maximize();               // Maximize the window on GTK and Windows, zoom the window on Mac.
win.unmaximize();             // Unmaximize the window, e.g. the reverse of maximize().
win.minimize();               // Minimize the window to taskbar on Windows, iconify the window on GTK, and miniaturize the window on Mac.
win.restore();                // Restore window to previous state after the window is minimized, e.g. the reverse of minimize(). It's not named unminimize since restore is already used commonly on Window.
win.enterFullscreen();        // Make the window fullscreen. This function is different with HTML5 FullScreen API, which can make part of the page fullscreen, Window.enterFullscreen() will only fullscreen the whole window.
win.leaveFullscreen();        // Leave the fullscreen mode.
win.toggleFullscreen();       // Toggle the fullscreen mode.
win.enterKioskMode();         // Enter the Kiosk mode. In Kiosk mode, the app will be fullscreen and try to prevent users from leaving the app, so you should remember to provide a way in app to leave Kiosk mode. This mode is mainly used for presentation on public displays.
win.leaveKioskMode();         // Leave the Kiosk mode.
win.toggleKioskMode();        // Toggle the kiosk mode.

win.showDevTools([id | iframe, headless]);  // Open the devtools to inspect the window.
win.closeDevTools();                        // Close the devtools window.
win.isDevToolsOpen();                       // Query the status of devtools window.
win.setMaximumSize(width, height);          // Set window's maximum size.
win.setMinimumSize(width, height);          // Set window's minimum size.
win.setResizable(Boolean resizable);        // Set whether window is resizable.
win.setAlwaysOnTop(Boolean top);            // Sets the widget to be on top of all other windows in the windowing system.
win.setPosition(String position);           // Shortcut to move window to specified position. Currently only center is supported on all platforms, which will put window in the middle of the screen.
win.setShowInTaskbar(Boolean show);         // Control whether to show window in taskbar or dock.
win.requestAttention(Boolean attention);    // Pass true to indicate that the window needs user's action, pass false to cancel it. The final behaviour depends on the platform.
win.setBadgeLabel(label);                   // Windows and OSX only. Set the badge label on the window icon in taskbar or dock.
win.cookies.*;                              // This includes multiple functions to manipulate the cookies. The API is defined in the same way as Chrome Extensions'. node-webkit supports the get, getAll, remove and set methods; onChanged event (supporting both addListener and removeListener function on this event).
win.eval(frame, script);                    // Execute a piece of JavaScript in the window, if frame argument is null, or in the context of an iframe, if frame is an iframe object. The script argument is the content of the JavaScript source code.
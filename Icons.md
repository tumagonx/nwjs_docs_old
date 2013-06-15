##Title bar icon
To change icon in title bar of your app, put a reference to PNG/JPG into package.json
```
{
  ... other stuff here ...
  "window": {
     "icon": "path/to/my/app.png", 
  }
}
```

##Application icon
This is platform specific unfortunately.

###Windows
After creating package you would have to use resource editor to change bundled icon. Some sources recommend freeware [Resource Hacker](http://www.angusj.com/resourcehacker/) which would do the trick ( [example](http://www.techtalkz.com/tips-n-tricks/3866-how-change-default-icon-exe-using-resource-editor-resource-hacker.html) )

###OSX
Assuming you have your icon in PNG/JPG format, you'd first need to convert it to [icns file format](http://en.wikipedia.org/wiki/Apple_Icon_Image_format). There are different ways to accomplish this, but the easiest is probably using free version of [IMG2ICNS](http://www.img2icnsapp.com/). 
Drag your image into app window and export resulting icon as ```nw.icns```. You'd need to replace default nw.icns inside ```Your.app/Contents/Resources```. Or you may want to change the name of your icon to smth like  ```mysupericon.icns``` and change the value of CFBundleTypeIconFile in Your.app/Contents/Info.plist to reflect new name. 

###Linux
You'd need to create proper [.desktop file](https://wiki.archlinux.org/index.php/Desktop_Entries).
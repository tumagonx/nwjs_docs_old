Refer to the official documentation by Apple regarding generating the `icns` icon [here](https://developer.apple.com/library/archive/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html)

You need to have matched the requirements or the Application Loader will fail.

* icon_16x16.png
* icon_16x16@2x.png
* icon_32x32.png
* icon_32x32@2x.png
* icon_128x128.png
* icon_128x128@2x.png
* icon_256x256.png
* icon_256x256@2x.png
* icon_512x512.png
* icon_512x512@2x.png

1. Create an iconset `$ mkdir [name].iconset`
2. Add all the PNG's to the iconset folder
3. Run the icon util `$ iconutil -c icns <iconset filename>`
4. Replace the `app.icns` and the `document.icns` file with the generated icns inside `your.app/Contents/Resources`
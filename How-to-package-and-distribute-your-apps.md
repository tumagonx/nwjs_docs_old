## Make a package

_Since our package system is similar to [LÖVE](https://love2d.org), following guides are modified from its [Wiki](https://love2d.org/wiki/Game_Distribution)._

An app's package is a zip archive with `.nw` as extension. Three caveats:

* There must be a `package.json` file that describes the package, see [[Manifest format]].
* The `package.json` file must be at the root of the archive. 
* In the `.nw`, the file and directory path names are case sensitive. This can be puzzling for Windows and Mac OS X users, whose filesystem is case insensitive, and whose apps may work when unzipped but not when packaged.

Here's how to proceed to generate a working `.nw` file:

### Windows 

1. Create a zip file (this is built into XP, Vista and 7) 
2. Copy all of your files into the zip file, retaining directory structure and making sure that the `package.json` file is in the root directory (if you make a zip file containing a folder with your stuff in it, then it's not going to work) 
3. Rename the file extension from `.zip` to `.nw`. By default, file extensions may be hidden. You need to (press `alt`), go to folder options and uncheck "Hide extensions for known file types" to be able to rename the zip. 

### Linux / OS X 

From the command line: 

1. Go to your project directory a la `cd ~/Projects/my_app`
2. Run `zip -r ../${PWD##*/}.nw *`
3. Your fully-prepared `.nw` file shall be located right outside of your project directory 
4. Cake!

## Making an executable file out of a .nw file 

Many people are (understandably) concerned about what end-users need to do in order to run an app. If users receive a `.nw` alone, they will naturally need `node-webkit` installed (or at least unzipped) in order to run it. But, with `node-webkit`, you can merge the `.nw` file with the `node-webkit` executable. 

In general, it's recommended to offer a `.nw` for download, and optionally "merged" versions for the platforms where this makes things simpler. 

Two things should be noted: 

* The end result will not be a single executable, you must also include some DLLs in your zip-file. 
* The resulting executable from the merge will still be readable by archiving software, such as WinZip.

### Windows 

Here's how to do it on Windows. In a console, type this: 

    copy /b nw.exe+app.nw app.exe 

Then, all you have to do is zip app.exe and required DLLs, and distribute them. Yes; this does mean that the game will have a private copy of `node-webkit`, but there's nothing wrong with that. It also means that you will have to create one package for each platform you would like to support, or simply offer the `.nw` alone for the other platforms. 

### Linux 

On Linux, it's similar: 

    cat /usr/bin/nw app.nw > app && chmod +x app 

Then, you'll have to make a package for various packaging systems with dependencies as the `node-webkit` package. Were you to make a .deb package this way, for instance, the user would not have to install the `node-webkit` package separately. 

Eventually, we will provide scripts which do this automatically for various package systems. You'll have to figure it out yourself until then. 

### Mac OS X 

_Following guides apply to node-webkit >= 0.2.3_

On OS X, the `node-webkit.app` is a directory that can be easily changed. You need to modify following files to make a real distribution of yours:

* `Contents/MacOS/node-webkit`: the binary itself, see instructions of Linux on how to append `.nw` file to the binary.
* `Contents/Resources/app.icns`: icon of your app.
* `Contents/Info.plist`: the apple package description file.

About the `Info.plist` the file, you can view [Implementing Cocoa's Standard About Panel](http://cocoadevcentral.com/articles/000071.php) on how this file will influence your app and what fields you should modify.

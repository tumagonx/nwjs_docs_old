**You can use [node-webkit-builder](https://github.com/mllrsohn/node-webkit-builder) to automate this process.**

Structure of files for autobuild cross-platform app:

```
my-app/
├── Resources/
│   ├── package.json
│   ├── index.html
│   ├── build.sh
│   ├── build.bat
│   └── build.command
├── build/
│   ├── linux
│   ├── win32
│   └── osx
```

In Linux we can "install" node-webkit in: /opt/node-webkit

##build.sh for Linux:
```
#zip all files to nw archive
zip -r my-app.nw ./*
#copy nw.pak from current build node-webkit
cp /opt/node-webkit/nw.pak ./nw.pak
#compilation to executable form
cat /opt/node-webkit/nw ./my-app.nw > ../build/linux/my-app && chmod +x ../build/linux/my-app
#move nw.pak to build folder
mv ./nw.pak ../build/linux/nw.pak
#remove my-app.nw
rm ./my-app.nw
#run application
../build/linux/my-app
```

In Windows we can "install" node-webkit in: c:\node-webkit

In Windows we must use for example 7-zip [http://www.7-zip.org/] batch file - in this system we cannot make zip archive from native console...

##build.bat for Windows:
```bat
rem zip all files without git to zip archive -2 compression methods - fast (-mx0) or strong (-mx9)
7z.exe a -tzip my-app.nw * -xr!?git\* -mx0
rem copy nw.pak from current build node-webkit to current (%~dp0) folder
copy c:\node-webkit\nw.pak nw.pak
rem copy icudt.dll from current build node-webkit
copy c:\node-webkit\icudt.dll icudt.dll
rem compilation to executable form
copy /b c:\node-webkit\nw.exe+%~dp0my-app.nw ..\build\win32\my-app.exe
rem move nw.pak to build folder
copy nw.pak ..\build\win32\nw.pak
del nw.pak
rem move icudt.dll to build folder
copy icudt.dll ..\build\win32\icudt.dll
del icudt.dll
rem remove my-app.nw
del my-app.nw
rem run application
..\build\win32\my-app.exe
```
## PowerShell Script
[PowerShell Script](https://gist.github.com/romanov/abc494ee7b08f232f539)

## Building a .dmg for mac when you don't own a mac
If you want to create a proper .dmg file for OSX, you will need a an OSX environment to create the package. These *cannot* be built on Windows or Linux!

You can either hijack a friend's Mac, or run virtualize OSX on VirtualBox or VMWare

Example OSX compatible shell script that SCP's node-webkit-builder's output to an OSX machine and builds a .dmg using [create-dmg](https://github.com/andreyvit/create-dmg). 
Copies output script back to the remote host where you ran node-webkit-builder.

build-osx.sh
```
# cleanup old input and output dir and rebuild them.
rm -rf ./input
mkdir input
rm -rf ./output
mkdir output

#copy over files from remote host to build on this machine
scp -r myuser@nodewebkitbuilderhost:/var/www/my-app/build/osx ./input

#make sure all relevant execute permissions are set properly, or the app will not start.
chmod +x ./input/myapp/myapp.app
chmod +x ./input/myapp/myapp.app/Contents/MacOS/nwjs 
chmod +x ./input/myapp/myapp.app/Contents/Frameworks/nwjs\ Helper.app/Contents/MacOS/nwjs\ Helper 
chmod +x ./input/myapp/myapp.app/Contents/Frameworks/nwjs\ Helper\ NP.app/Contents/MacOS/nwjs\ Helper\ NP 
chmod +x ./input/myapp/myapp.app/Contents/Frameworks/nwjs\ Helper\ EH.app/Contents/MacOS/nwjs\ Helper\ EH 
chmod +x ./input/myapp/myapp.app/Contents/Frameworks/nwjs\ Framework.framework/nwjs\ Framework 

#run create-dmg from parent directory. Adjust this location when needed.
../create-dmg/create-dmg \
--volname "My App Installer" \
--window-pos 200 120 \
--window-size 800 400 \
--icon-size 100 \
--icon myapp.app 200 190 \
--hide-extension myapp.app \
--app-drop-link 600 185 \
output/myapp.dmg \
input/myapp/

#copy back results to node-webkit-builder host
scp ./output/myapp.dmg myuser@nodewebkitbuilderhost:/var/www/my-app/build/
```




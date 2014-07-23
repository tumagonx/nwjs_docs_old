Sublime Text 2 is a great cross-platform editor for building node-webkit apps, you can download it here: http://www.sublimetext.com/2

### Mac OS X

1. Download node-webkit.app and place it in /Applications folder
2. From the Sublime Text 2 menu select `Tools -> Build System -> New Build System`
3. Enter the following code:

````json
{
    "cmd": ["node-webkit", "--enable-logging", "${project_path:${file_path}}"],
    "working_dir": "${project_path:${file_path}}",
    "path": "/Applications/node-webkit.app/Contents/MacOS/"
}
````

4. Open a new window in Sublime Text 2 using `File -> New Window`
5. Add your project to the window using `Project -> Add Folder to Project...`
6. Open your main application file (ex.: index.html) from the left side menu and select `Tools -> Build`
7. At this point node-webkit application will launch with your project and you will be able to see the debugging output in Sublime Text 2

Work the same with Sublime Text 3.

### Windows
As above, but the commands for the build system are as follows (replacing the path with the location of nw.exe):

````json
{
    "cmd": ["nw.exe", "--enable-logging", "${project_path:${file_path}}"],
    "working_dir": "${project_path:${file_path}}",
    "path": "C:/Tools/node-webkit/",
    "shell": true
}
````

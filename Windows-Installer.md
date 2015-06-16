### Batch for create build version

**Dependencies:**
* 7-Zip
* sed.exe
* Bat_To_Exe_Converter.exe
* Inno Setup 5
* Resourcer.exe

**Directory tree created by the batch**

`- dist
 +---- release\
     +---- {version}\
     |   |---- start.exe
     |   +---- bin\
     |       |---- application.exe
     |       |---- nw.pak
     |       |---- libEGL.dll
     |       |---- ...
     +---- installer\
     +---- update\
`
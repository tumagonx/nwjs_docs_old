### Batch for create build version

**Dependencies:**
* 7-Zip
* sed.exe
* Bat_To_Exe_Converter.exe
* Inno Setup 5
* Resourcer.exe

**Directory tree created by the batch**

`dist\release\{version}\start.exe
 dist\release\{version}\bin\application.exe
 dist\release\{version}\bin\nw.pak
 dist\release\{version}\bin\libEGL.dll
 dist\release\{version}\bin\...
 dist\installer\installer_{version}.exe
 dist\update\package.json
 dist\update\app_v{version}.zip
`
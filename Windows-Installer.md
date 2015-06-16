### Batch for create build version

**Dependencies:**
* 7-Zip
* sed.exe
* Bat_To_Exe_Converter.exe
* Inno Setup 5
* Resourcer.exe

**Directory tree created by the batch:**
<pre>
 dist\release\{version}\start.exe dist\release\{version}\bin\application.exe
 dist\release\{version}\bin\nw.pak
 dist\release\{version}\bin\libEGL.dll
 dist\release\{version}\bin\...
 dist\installer\installer_{version}.exe
 dist\update\package.json
 dist\update\app_v{version}.zip
</pre>

**Batch**

<pre>
@echo off
@cls

set name=unity
set version=1.0.1
set updater_url=http://url/to/update/package.json
set path_dist=C:\path\to\dist
set path_www=C:\path\to\www
set path_icon=C:\path\to\icon.ico

set path_7zip="C:\path\to\7-Zip\7z"
set path_sed="C:\path\to\sed.exe"
set path_bat2exe=C:\path\to\Bat_To_Exe_Converter.exe"
set path_inno_setup_exe="C:\path\to\InnoSetup\ISCC.exe"
set path_icon_resource="C:\path\to\Resource"
set path_inno_setup_scp=C:\path\to\script.iss
set path_bin=C:\path\to\nw_bin

set path_bat=C:\path\to\start.bat
set path_tools=C:\path\to\tools

set mypath=%CD%
@cd %mypath%

rem cria a pasta release\%version%\bin
%path_release_version% quando existia ficava bloqueada
echo.
echo starting compilation version "%version%"...
set path_release_version=%path_dist%\release\%version%
set path_update_version=%path_dist%\update\%version%
if not exist %path_dist%\release md %path_dist%\release
if not exist %path_dist%\update md %path_dist%\update
if not exist %path_dist%\installer md %path_dist%\installer
if exist %path_release_version% rd /s /q %path_release_version%
if exist %path_update_version% rd /s /q %path_update_version%
ping -n 1 127.0.0.1 > nul
md %path_release_version%
md %path_release_version%\bin
md %path_release_version%\tools
md %path_release_version%\sed
md %path_release_version%\www
md %path_update_version%
del /q %path_release_version%\sed\*


rem cria o executável inicial, start.bat para .exe
echo creating started executable...
%path_bat2exe% -overwrite -invisible -bat %path_bat% -save %path_release_version%\%name%.exe -icon %path_icon% > nul


rem copia os binários
echo copy binaries...
copy %path_bin%\* %path_release_version%\bin > nul
copy %path_tools%\* %path_release_version%\tools > nul
if exist %path_release_version%\bin\package.json del %path_release_version%\bin\package.json


rem cria o zip www. substitui {{version}} em package.json
echo creating www.zip...
xcopy %path_www%\* %path_release_version%\www /s > nul
@cd %path_release_version%\sed
%path_sed% -i "s/{{version}}/%version%/g" %path_release_version%\www\package.json
@cd %path_release_version%\www
%path_7zip% a -tzip %path_release_version%\www.zip * > nul
@cd %mypath%
rd %path_release_version%\www /s /q
rd %path_release_version%\sed /s /q


rem cria um executável único com os arquivos www
echo creating application.exe...
cd %path_icon_resource%
Resourcer -op:upd -src:%path_release_version%\bin\nw.exe -type:14 -name:IDR_MAINFRAME -file:%path_icon% > nul
copy /b %path_release_version%\bin\nw.exe+%path_release_version%\www.zip %path_release_version%\bin\application.exe > nul


rem exclui alguns arquivos (limpeza)
del %path_release_version%\www.zip
del %path_release_version%\bin\nw.exe
if exist %path_release_version%\bin\start.bat del %path_release_version%\bin\start.bat
if exist %path_release_version%\bin\run.bat del %path_release_version%\bin\run.bat
if exist %path_release_version%\bin\credits.html del %path_release_version%\bin\credits.html
if exist %path_release_version%\bin\nwjc.exe del %path_release_version%\bin\nwjc.exe


rem cria o zip para atualização automática
echo creating bin update zip...
cd %path_release_version%\bin
%path_7zip% a -tzip %path_update_version%\%name%_%version%.zip * > nul
cd %mypath%


rem cria package.json
echo {"version":"%version%","location":"%updater_url%","file":"%name%_%version%.zip"} > %path_update_version%\package.json


<span style="color:green">rem cria o instalador</span>
echo creating installer...
md %path_release_version%\sed\
copy %path_inno_setup_scp% %path_release_version%\sed\script.iss > nul
@cd %path_release_version%\sed
%path_sed% -i "s/{{version}}/%version%/g" %path_release_version%\sed\script.iss
@cd %mypath%
%path_inno_setup_exe% /cc %path_release_version%\sed\script.iss
rd %path_release_version%\sed /s /q

:end
echo finish.
</pre>

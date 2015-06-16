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

rem cria o instalador
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

**start.bat**
<pre>
@echo off

cd %CD%

rem pausa por 3 segundos
if "%1" == "--restart" (
    ping -n 3 127.0.0.1 > nul
)

if exist bin_new (
    if exist bin (
        ren bin bin_old
    )
    
    ren bin_new bin
)

if exist bin_old (
    rd bin_old /s /q
)

start /I bin\application.exe
</pre>

**script.iss**

<pre>

#define MyAppName "MyAppName"
#define MyAppVersion "{{version}}"
#define MyAppPublisher "My Company, Inc."
#define MyAppURL "http://my.url.to.app/"
#define MyAppExeName "MyApp.exe"

[Setup]
AppId={{Unity@redeci.com.br}
AppName={#MyAppName}
AppVersion={#MyAppVersion}
AppVerName={#MyAppName}v{#MyAppVersion}
AppPublisher={#MyAppPublisher}
AppPublisherURL={#MyAppURL}
AppSupportURL={#MyAppURL}
AppUpdatesURL={#MyAppURL}
DefaultDirName=C:\{#MyAppName}
DefaultGroupName={#MyAppName}
OutputDir=C:\path\to\installer
OutputBaseFilename=Unity_v{#MyAppVersion}
Compression=lzma
SolidCompression=yes

[Languages]
Name: "brazilianportuguese"; MessagesFile: "compiler:Languages\BrazilianPortuguese.isl"

[Tasks]
Name: "desktopicon"; Description: "{cm:CreateDesktopIcon}"; GroupDescription: "{cm:AdditionalIcons}"; Flags: unchecked

[Files]
Source: "C:\path\to\dist\release\{#MyAppVersion}\bin\*"; DestDir: "{app}\bin"; Flags: ignoreversion
Source: "C:\path\to\dist\release\{#MyAppVersion}\tools\*"; DestDir: "{app}\tools"; Flags: ignoreversion
Source: "C:\path\to\dist\release\{#MyAppVersion}\Myapp.exe"; DestDir: "{app}"; Flags: ignoreversion

[Icons]
Name: "{group}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"
Name: "{group}\{cm:UninstallProgram,{#MyAppName}}"; Filename: "{uninstallexe}"
Name: "{commondesktop}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"; Tasks: desktopicon

[Run]
Filename: "{app}\{#MyAppExeName}"; Description: "{cm:LaunchProgram,{#StringChange(MyAppName, '&', '&&')}}"; Flags: nowait postinstall skipifsilent

</pre>

**Updater.js**

<pre>
/**
 * @class Updater
 * @version 0.0.1
 * @author Fábio Nogueira
 */

var Updater = (function(){
    
    var
        fs   = require('fs'),
        http = require('http'),
        path = require('path'),
        gui  = require('nw.gui'),
        PATH_ALL_USER = process.env.ALLUSERSPROFILE + '/.ld/', //HOMEDRIVE
        DATA = {
            status: 0,
            version: '',
            newVersion: '',
            releaseNote: '',
            downloadPercent: 0,
            available: false,
            err: ''
        },
        EXE, self,
        STATUS_NONE=0, STATUS_CHECKING=1, STATUS_DOWNLOADING=2,
        PATH_APP, FS, APP_NAME, Updater;

    FS = {
        copyFiles: function(sources, folder, next){
            var i=-1;
            
            function runCopy(){
                i++;
                if (i>=sources.length){
                    return next();
                }
                console.log(i, sources[i] + ' > ' + folder + '/' + path.basename(sources[i]))
                FS.copy(sources[i], folder + '/' + path.basename(sources[i]), runCopy);
            }
            
            runCopy();
        },
        copy: function(source, target, next) {
            // copia um arquivo para outro local sobrescrevendo o atual, se existir.
            
            var rd, wr, cbCalled = false;

            rd = fs.createReadStream(source);
            rd.on("error", done);

            wr = fs.createWriteStream(target);
            wr.on("error", done);
            wr.on("close", function(ex) {
                done();
            });
            rd.pipe(wr);

            function done(err) {
                if (!cbCalled) {
                    next(err);
                    cbCalled = true;
                }
            }
        },
        createPath: function(dir, next){
            fs.lstat(dir, function (err, stats){
                var
                    exists;

                if (err) {
                    exists = !(err.code==='ENOENT');
                }else{
                    exists = stats.isDirectory();
                }

                if (!exists){
                    fs.mkdir(dir, '0777', function(err){
                        next(err ? false : true);
                    });
                }else{
                    next(true);
                }
            });
        },
        removePath: function(path, next){
            var files = [];
            
            if( fs.existsSync(path) ) {
                files = fs.readdirSync(path);
                files.forEach(function(file,index){
                    var curPath = path + "/" + file;
                    if(fs.lstatSync(curPath).isDirectory()) { // recurse
                        FS.removePath(curPath);
                    } else { // delete file
                        fs.unlinkSync(curPath);
                    }
                });
                fs.rmdirSync(path);
                if (next) next(false);
            }
        },
        rename: function(oldName, newName, next, count){
            count = count===undefined ? 5 : count;
            fs.rename(oldName, newName, function(err){
                if (err){
                    count--;
                    if (count>0){
                        setTimeout(function(){FS.rename(oldName, newName, next, count);}, 400);
                    }else{
                        console.error(err);
                        next(false);
                    }
                }else{
                    next(true);
                }
            });
        },
        cmd: function(command, args, next){
            var 
                spawn = require('child_process').spawn,
                cmd;
            
            cmd = spawn(command, args, {detached: true});//, cwd:path.normalize(process.execPath+'/')});
            
            if (next){
                cmd.stdout.on('data', function(data) {
                    
                });
                cmd.on('exit', function(code){
                    if (next) next (code===0);
                });
                cmd.on('error', function(err){
                    if (next) next (false, err.message);
                });
            }
            
        },
        unzip: function(file, dest, next){
            var root = path.dirname(path.normalize(process.execPath+'/../')) + '/',
                command = root+'tools/unzip.exe',
                args    = ['-o', file, '-d', dest];
            
            command = command.replace(/\\/g, '/');
            file = file.replace(/\\/g, '/');
            dest = dest.replace(/\\/g, '/');
            
            this.cmd(command, args, next);
        }
    };
    
//private functions:
    function readFile(file, next) {
        fs.readFile(file, 'utf8', function(err, data) {
            if (err){
                dispatch(INSTALLER_CB, 'log', ' <span style="color:red">error, code: '+err.code+'</span>');
            }else{
                dispatch(INSTALLER_CB, 'log', ' OK');
            }
            next(err ? null : data);
        });
    }
    function writeFile(file, content, next){
        dispatch(INSTALLER_CB, 'log', '\nwrite file "' + file + '"...');
        fs.writeFile(PATH_APP + file, content, function (err){
            if (err){
                dispatch(INSTALLER_CB, 'log', ' <span style="color:red">error, code: '+err.code+'</span>');
            }else{
                dispatch(INSTALLER_CB, 'log', ' OK');
            }
            next ? next(err ? false : true) : null;
        });
    }
    function normalizeUrl(url){
        url = url.replace(/\\/g,'/');
        url = url.replace(/\/\//g,'/');
        url = url.replace(/http:\//g,'http://');
        
        return url;
    }
    
    Object.observe(DATA, function(){
        self.onChange(DATA);
    });

    self = {
        onChange: function(){},
        data: function(){
            return DATA;
        },
        createPaths: function(next){
            //cria os diret�rio: ALLUSERS/.livred/apps/[appname]/
            var
                f = ['apps', APP_NAME];
            
            dispatch(INSTALLER_CB, 'log', '\ncreate paths...');
            
            function create(path, index){
                FS.createPath(path, function(success){
                    if (success){
                        path += (f[index] + '/');
                        if (index < f.length){
                            index++;
                            create(path, index);
                        }else{
                            dispatch(INSTALLER_CB, 'log', ' OK');
                            next(true);
                        }
                    }else{
                        dispatch(INSTALLER_CB, 'log', ' <span color="red">error</span>');
                        next(false);
                    }
                });
            }

            create(PATH_ALL_USER, 0);  
        },
        checkForUpdate: function(urls, next) {
            var index = -1;
            
            function check(url){
                
                http.request(url, function(res, err) {
                    if (err || res.statusCode !== 200) {
                        DATA.err = err ? err.code : res.statusCode;
                        nextUrl();
                    } else {
                        res.on('data', function(chunk) {
                            var result;
                            
                            try{
                                result = JSON.parse(chunk.toString());
                                
                                if (result.version > DATA.newVersion){
                                    DATA.newVersion = result.version;
                                    result.location = result.location || url;
                                    nextUrl(result);
                                }else{
                                    nextUrl();
                                }
                            }catch(_e){
                                nextUrl();
                            }
                        });
                    }
                })
                .on('error', function(err) {
                    nextUrl();
                })
                .end();
            }
            
            function nextUrl(res){
                index++;
                
                if ( res || !urls[index] ) {
                    DATA.status = STATUS_NONE;
                    return next(res || null);
                }
                
                check(urls[index]);
            }
            
            nextUrl();
        },
        download: function(url, dest, next){
            var request;
            
            DATA.downloadPercent = 0;
            
            request = http.request(url, function (res, err) {
                var file, percent, oldPercent,
                    count = 0,
                    len = parseInt(res.headers['content-length'], 10);

                    if (err || res.statusCode!==200) {
                        DATA.err = 'download_error: (' + url + ') ' + (err ? err.code : res.statusCode);
                        next();
                    } else {
                        file = fs.createWriteStream(dest+'.part');
                        file.on('finish', function() {
                            DATA.downloadPercent = 100 ;
                            if (count===len){
                                FS.rename(dest+'.part', dest, function(success){
                                    if (success){
                                        next(true);
                                    }else{
                                        DATA.err = 'error';
                                        next();
                                    }
                                });
                            }else{
                                DATA.err = "download aborded.";
                                next();
                            }
                        });

                        res.pipe(file);

                        res.on('data', function (chunk) {
                            if (chunk.length){
                                count += chunk.length;
                                percent = parseInt((count*100) / len);
                                if (percent!==oldPercent){
                                    DATA.downloadPercent = percent;
                                }
                                oldPercent = percent;
                            }
                        });
                        
                        request.setTimeout(1200, function(){
                            request.abort(); //isso faz com que chame o file.on('finish', ...
                        });
                    }
                });
                
            request.on('error', function(err){
                DATA.err = 'download_error: (' + url + ') ' + err.code;
                next();
            });            
            request.end();
        },
        restart: function(){
            var child, child_process, win,
                executable = path.dirname(path.normalize(process.execPath+'/../')) + '/' + EXE;
        
            child_process = require("child_process");
            win = gui.Window.get();

            child = child_process.spawn(executable, ['--restart'], {detached: true, cwd:path.dirname(executable)});
            child.unref();
            
            win.hide();
            gui.App.quit();
        },
        init: function() {
            var manifest= gui.App.manifest,
                root = path.dirname(path.normalize(process.execPath+'/../')) + '/',
                urls, zipName;
            
            if (!manifest.updater) return;
            if (!manifest.updater.locations) return;
            
            manifest.updater.interval = manifest.updater.interval || 20000;
            
            EXE  = path.basename(process.execPath);
            zipName = root + EXE + '_update.zip';
            urls = manifest.updater.locations.split(';');
            
            DATA.version = DATA.newVersion = (manifest.version || "0.0.0");
            
            function nextCheck(){
                if (DATA.err !== ''){
                    DATA.newVersion = DATA.version;
                }
                
                DATA.status = STATUS_CHECKING;
                DATA.err = '';
                DATA.downloadPercent = 0;
                
                self.checkForUpdate(urls, function(res){
                    var urlFileDownload;
                    
                    if (res){
                        DATA.status = STATUS_DOWNLOADING;
                        urlFileDownload = normalizeUrl(res.location+'/') + (res.file || EXE.replace('.exe', '_v'+res.version+'.zip'));
                        
                        self.download(urlFileDownload, zipName, function(success){
                            DATA.status = STATUS_NONE;
                            
                            if (!success) {
                                DATA.newVersion = DATA.version;
                                DATA.downloadPercent = 0;
                                
                                setTimeout(nextCheck, manifest.updater.interval);
                            }else{
                                FS.unzip(zipName, root+'bin_new', function(success, err) {
                                    if (success) {
                                        DATA.available = true; //significa que tem uma nova vers�o em uma pasta bin_new
                                        
                                        //exclui o arquivo zipado
                                        fs.unlink(zipName, function(err) {
                                            setTimeout(nextCheck, manifest.updater.interval);
                                        });
                                    }else{
                                        DATA.err = err;
                                        
                                        setTimeout(nextCheck, manifest.updater.interval);
                                    }
                                });
                            }
                        });
                    }else{
                        DATA.status = STATUS_NONE;
                        setTimeout(nextCheck, manifest.updater.interval);
                    }
                });
            }
            
            nextCheck();
            
            return self;
        }
    };
    
    return self;
}());

</pre>

**index.html**

<code>
<!DOCTYPE html>
<html>
    <head>
        <title>Test</title>
        <meta charset="UTF-8">
        
        <script src="Updater.js" type="text/javascript"></script>
    </head>
    <body>
        
        <div id="log" style="display:none;position:absolute;bottom:0;left:0;right:0;height:200px;background:#fff"></div>
        

        <script>
            var title = 'Unity';
            
            function jsonToString(data){
                var i, html='<pre>{\n';
                for (i in data){
                    html += ('     "'+i+'": "'+data[i]+'"\n');
                }
                html+='}</pre>';
                return html;
            }

            Updater.onChange = function(data){
                document.getElementById('log').innerHTML = jsonToString(data)
            };
            
            Updater.init();
            
        </script>
    </body>
</html>

</code>
__This relates to the OSX version only.__

Sometimes you want to give your app to someone directly or via a download from your website. In that case it is preferable that you sign it so the recipient can more easily install it without running foul of Gatekeeper.

The requirements in this case are less than the ones necessary for delivery via the MAS.

*Prior to signing your app, you have to request and install certificates from the Apple Member Center. To do so, you can check [[this page|MAS: Requesting certificates]]. You only need the **Mac App Distribution** certificate - the one you will use to sign your .app file.*

The following assumes you have [packaged](http://docs.nwjs.io/en/latest/For%20Users/Package%20and%20Distribute/) your .app file and you have your Mac App Distribution certificate installed.

First, you need the chromium version number from the nw.js version you used to build your app. This can be found via `nw.process.versions.chromium`, or open the .app package and navigate to /Contents/Versions and you will see a folder with the version number. It will look something like 64.0.3282.186

You need to run the codesign commands below from the same folder as your .app file.

For convenience I run a .command file:

    cd path/to/myapp folder

    # The chromium version number you are using from: nw.process.versions.chromium
    chromium="64.0.3282.186"

    # Your app file name:
    app="myapp.app"

    # Your certificate identity
    identity="HEX_STRING_FROM_YOUR_CERTIFICATE"

    # Code sign your app using the variables above
    codesign --force --verify --verbose --sign "$identity" "$app/Contents/Versions/$chromium/nwjs Framework.framework/Helpers/crashpad_handler"
    codesign --force --verify --verbose --sign "$identity" "$app/Contents/Versions/$chromium/nwjs Framework.framework/libnode.dylib"
    codesign --force --verify --verbose --sign "$identity" "$app/Contents/Versions/$chromium/nwjs Framework.framework"
    codesign --force --verify --verbose --sign "$identity" "$app/Contents/Versions/$chromium/nwjs Helper.app"
    codesign --force --verify --verbose --sign "$identity" "$app"

    # Verify your app is signed correctly. This should output "valid on disk. satisfies its Designated Requirement"
    codesign --verify --verbose=4 "$app"

From there you can package into a .dmg using whatever tool you use:
eg [appdmg](https://github.com/LinusU/node-appdmg) (free)
or [DropDmg](https://c-command.com/dropdmg/) (not free)
or *(please add other solutions you are using)*


 
  
_This relates to the OSX version only._

Sometimes you want to give your app to someone directly or via a download from your website. In that case it is preferable that you sign it so the recipient can more easily install it without running foul of Gatekeeper.

The requirements in this case are less than the ones necessary for delivery via the Mac App Store (MAS).

*Prior to signing your app, you have to request and install certificates from the Apple Member Center. To do so, you can check [[this page|MAS: Requesting certificates]]. NOTE: Choose the **Production > Developer ID** certificate.*

Open Applications > Keychain Access, and look for your new certificate under the "My Certificates" side panel. You'll notice it says "Developer ID Application: YOUR NAME (XXXXXXXXXX)". The confidential string between the `(XXXXXXXXXX)` is the ID you'll use later.

---

The following assumes you have [packaged](http://docs.nwjs.io/en/latest/For%20Users/Package%20and%20Distribute/) your .app file and you have your Mac Developer ID Application certificate installed.

### Signing the .app

1. `cd` to the folder containing your .app
    ```bash
    cd path/to/folder
    ```

2. Perform the codesign

    _NOTE: Replace MAC_CERTIFICATE with the string `(XXXXXXXXXX)` from your developer certificate, and replace APP_NAME with the name of your app._

    ```bash
    codesign --force --deep --verbose --sign  "MAC_CERTIFICATE" APP_NAME.app
    ```

3. Verify it worked

    _NOTE: Replace APP_NAME with the name of your app._

    ```bash
    codesign --verify -vvvv APP_NAME.app & spctl -a -vvvv APP_NAME.app
    ```

4. You should see the following messages:

- APP_NAME: signed app bundle with Mach-O thin (x86_64)
- APP_NAME: valid on disk
- APP_NAME: satisfies its Designated Requirement
- APP_NAME: accepted

5. Congrats, you're all set to distribute your Mac app!

---

For convenience, you may create a .command file to keep these commands for later:

1. Create the .command file
    ```bash
    touch my-script.command
    ```

2. Set permissions to run the .command file
    ```bash
    chmod u+x /path/to/file.command
    ```

3. Edit your .command file, adding in:

    _NOTE: Replace the two variables with your own information._

    ```bash
    # Automatically change to current directory
    cd "$(dirname "$0")"

    # CHANGE ME: Relative path to your .app from where this 
    # .command file is (include ".app" after the name of your app)
    APP_PATH="My-app.app"

    # CHANGE ME: Developer certificate
    CERTIFICATE=XXXXXXXXXX

    # CodeSign
    codesign --force --deep --verbose --sign "$CERTIFICATE" $APP_PATH

    # Verify
    codesign --verify -vvvv $APP_PATH & spctl -a -vvvv $APP_PATH
    ```

---

From there, you can package into a .dmg using whatever tool you use:
eg [appdmg](https://github.com/LinusU/node-appdmg) (free)
or [DropDmg](https://c-command.com/dropdmg/) (not free)
or *(please add other solutions you are using)*
*Prior to signing your app, you have to request and install certificates from the Apple Member Center. To do so, you can check [[this page|MAS: Requesting certificates]].*

### Configuring the permissions

We have to sign the one helper, and then the main app:

* `YourApp.app/Contents/Versions/*/nwjs Helper.app`
* `YourApp.app`

To do so, we use two entitlement files ([more information](https://developer.apple.com/library/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html)). They describe what the application is allowed to do when sandboxed.

The child entitlements will be applied on the three helpers, and basically, say that they inherit from the parent.

Child entitlements:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.apple.security.app-sandbox</key>
	<true/>
	<key>com.apple.security.inherit</key>
	<true/>
</dict>
</plist>
```

The parent entitlements will be applied on the main app. 

Parent entitlements:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.apple.security.app-sandbox</key>
	<true/>
</dict>
</plist>
```

You have to add rules in this one to suit your app needs. For instance, if you want your app to connect to the internet, you will have to add:

```xml
<key>com.apple.security.network.client</key>
<true/>
```

Or, if you want to read and write files, once they have been selected by the user (through a file dialog, for instance):

```xml
<key>com.apple.security.files.user-selected.read-write</key>
<true/>
```

Or, if you want a read-only access to the `~/Movies` directory:

```xml
<key>com.apple.security.assets.movies.read-only</key>
<true/>
```

Complete list of rules is available [here](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html).

**Important note:** only add entitlements that you actually use in your app for a good reason. Apple is known to reject binaries that require more permissions than they obviously need. Also, it is a good practice to provide enough clarity about used entitlements when submitting your app, in a message to Apple.

### Signing commands

We assume that you have saved both files as `child.plist` and `parent.plist`. The -f allows you to replace the signature if one is already in place.

You can now run the signing commands:

```bash
# Do not forget to update the above vars
# IDENTITY is the string you have saved in the "Requesting certificates" step
export IDENTITY=LK12345678 
export PARENT_PLIST=/path/to/parent.plist
export CHILD_PLIST=/path/to/child.plist
export APP_PATH=/path/to/yourapp/YourApp.app

codesign --deep -s  $IDENTITY --entitlements $CHILD_PLIST $APP_PATH"/Contents/Versions/CHROMIUMVERSIONHERE/nwjs Helper.app" -f

codesign --deep -s $IDENTITY --entitlements $PARENT_PLIST $APP_PATH -f
```
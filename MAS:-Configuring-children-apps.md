`NW.js` uses three *children* apps to work on OS X, as listed below:

* `YourApp.app/Contents/Frameworks/nwjs Helper.app`
* `YourApp.app/Contents/Frameworks/nwjs Helper EH.app`
* `YourApp.app/Contents/Frameworks/nwjs Helper NP.app`

To prevent collisions with other existing apps, you will have to set a unique bundle identifier to each helper.

To do so, you can use your app identifier (`com.yourcompanyname.yourappname`) as a base, and perform the following changes.

---

In `YourApp.app/Contents/Frameworks/nwjs Helper.app/Contents/Info.plist`, replace:

```xml
<key>CFBundleIdentifier</key>
<string>io.nwjs.nw.helper</string>
```

With: 

```xml
<key>CFBundleIdentifier</key>
<string>com.yourcompanyname.yourappname.helper</string>
```

---

In `YourApp.app/Contents/Frameworks/nwjs Helper EH.app/Contents/Info.plist`, replace:

```xml
<key>CFBundleIdentifier</key>
<string>io.nwjs.nw.helper.EH</string>
```

With: 

```xml
<key>CFBundleIdentifier</key>
<string>com.yourcompanyname.yourappname.helper.EH</string>
```

---

In `YourApp.app/Contents/Frameworks/nwjs Helper NP.app/Contents/Info.plist`, replace:

```xml
<key>CFBundleIdentifier</key>
<string>io.nwjs.nw.helper.NP</string>
```

With: 

```xml
<key>CFBundleIdentifier</key>
<string>com.yourcompanyname.yourappname.helper.NP</string>
```

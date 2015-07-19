The `Info.plist` file (located in `YourApp.app/Contents`) is used on OS X to display your app information, and on the Mac App Store for identification purposes. You will need to update it.

You may start from this [sample file](Info.plist).

You can override the existing file (`YourApp.app/Contents/Info.plist`), and update the following fields:

| Field | Information
| --- | --- |
| `__name__` | Readable app name |
| `__bundle_identifier__` | App identifier, looks like `com.yourcompanyname.yourappname` (keep it as short as you possibly can to avoid [issues](https://github.com/alexeyst/node-webkit-macappstore/issues/4#issuecomment-113073816)|
| `__version__` | Readable app version, looks like `1.4.8` |
| `__bundle_version__` | A unique, numeric app version - you can use the version as a base, like `148` for `1.4.8` (this solution is more readable than starting from `1` and counting) (this is used on the MAS only, and *must* be updated every time you upload a new binary) |
| `__copyright__` | A readable copyright message, appears when doing a `CMD-I` on your app |
| `__app_category__` | The app category, looks like `public.app-category.entertainment` ([complete list](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/LaunchServicesKeys.html)) |
| `__app_sec_category__` | An additional category (same format as above) |

The sample file contains the minimum, required fields that you need on the MAS.

You may need to add other fields, to associate a file extension with your app, for instance.
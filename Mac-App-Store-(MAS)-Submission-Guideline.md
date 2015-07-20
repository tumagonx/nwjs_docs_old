_Starting with NW.js >= 0.12.3 NW.js introduces official Mac App Store support (in beta)_

![NW.js > Mac App Store](mas-screenshots/icon.jpg)
# Background

Mac App Store imposes numerous requirements to submitted applications, including _sandboxing_ — a technology to ensure the app won't affect crucial system parts when and if broken, as well as provide an extra level of security for the application users. 

Sandboxed applications have numerous limitations further discussed in the [About App Sandbox](https://developer.apple.com/library/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html) Apple's Guide.

Beyond that, Mac App Store verifies the list of APIs used by the application prior to letting it to the store. This review focuses on ensuring that no obsolete APIs and no undocumented / private APIs are used by an application — all to the benefit of the end user who always gets the best user experience available in the platform.

# NW.js and Mac App Store

NW.js uses Chromium, which in turn has many references to private and obsolete OS X APIs ([what is a private API](http://stackoverflow.com/questions/3000681/what-are-private-apis)), including QTKit. Chromium is available on many platforms and doesn't specifically focus on complying with Mac App Store requirements. 

For that reason, submission of NW.js apps to Mac App Store was complicated, though possible, before NW.js 0.12.3. 

NW.js already has wide exposure to Mac App Store thanks to many developers who've gone to great lengths in making their NW.js apps successfully accepted. 

This guideline offers a complete walkthrough of the steps required to get your app published on the Mac App Store the easy way. For clarity, NW.js provides:

* Official NW.js build accepted by Mac App Store
* Help and support for NW.js Mac App Store submitters via issues in the repository.

**Note:** Mac App Store support is currently in beta. We're working to make your apps submission as smooth as possible. Mac App Store may also change requirements without notice, in which case we will be updating NW.js to follow them as soon as we can.

Please also note, that Mac App Store require you to carefully fulfill its requirements when submitting an app, so your success depends on how carefully you follow the guides.

If you have a question or a request about Mac App Store submission, please open an issue and mention **[@alexeyst](https://github.com/alexeyst)** or **[@johansatge](https://github.com/johansatge)** who are glad to help as soon as they can.

Good luck with getting your app published!

# Contents

Use the links below to navigate Mac App Store guide. For convenience, Mac App Store documentation pages  start with 'MAS' prefix so you could easily find them using the *Sidebar* on the right, just type `MAS` into the search field, or see the complete list of Mac App Store related pages below.

## First steps

* [[MAS: Initial requirements]]
* [[MAS: Requesting certificates]]
* [[MAS: Registering a new app on the MAS]]

## Packaging

* [[MAS: Packaging your app]]
* [[MAS: Installing your app icon]]
* [[MAS: Configuring Info.plist]]
* [[MAS: Configuring children apps]]
* [[MAS: Signing the app]]

## Distributing

* [[MAS: Uploading the binary]]
* [[MAS: Publishing your app]]
* [[MAS: The validation process]]

# Credits

Official Mac App Store support and this guide were made possible thanks to these people:

* [Trevor Linton](https://github.com/trevorlinton) for his work on the compatibility between NW.js and the Mac App Store
* [Johan Satgé](https://github.com/johansatge) for his work in summarizing complete Mac App Store submission process, writing most of this guide and user support
* [Alexey Stoletny](https://github.com/alexeyst) for maintaining custom Mac App Store builds, troubleshooting, user support, migration and portions of this guide
* [Roger Wang](https://github.com/rogerwang) for maintaining NW.js and introducing official Mac App Store support

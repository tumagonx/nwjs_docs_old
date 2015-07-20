### Getting the custom NW.js app

NW.js offers a custom build that can be submitted to Mac App Store. This custom built version excludes the use of various obsolete and private APIs utilized by Chromium and WebKit. For more details, please see [[Mac App Store (MAS) Submission Guideline]].

Mac App Store NW.js build can be downloaded here: [0.12.3](http://buildbot-master.nwjs.io:8010/builders/mac64_nw12_mas) or [externally hosted 0.12.2](https://github.com/alexeyst/node-webkit-macappstore/archive/master.zip)).

*Note:* NW.js Mac App Store support is in beta. Links will be updated.

### Automatic builds

You can use [nwjs-macappstore-builder](https://github.com/johansatge/nwjs-macappstore-builder) by [Johan Satgé](https://github.com/johansatge).

### Custom build limitations

The custom build completely disables the use of QTKit, which is used by Chromium for video streaming. Until Chromium moves on from QTKit over to AVFoundation, WebRTC applications will not work on this custom build.

### Standard packaging

Package your app by following the regular NW.js procedure (if you don't know how to do it, check the [official documentation page](https://github.com/nwjs/nw.js/wiki/How-to-package-and-distribute-your-apps)).

Roughly, you have to:

* Rename your app root directory (the one containing the `package.json` file) to `app.nw`
* Move this directory to `nwjs.app/Contents/Resources`
* Rename the app to what you want - let's assume it is `YourApp.app`

### Additional steps

To ensure proper validation, you have to check those steps:

Delete the FFMPEG library:

```bash
rm "YourApp.app/Contents/Frameworks/nwjs Framework.framework/Libraries/ffmpegsumo.so"
```

*Note:* Currently, FFMPEG library cannot be submitted to Mac App Store as is. More porting work is required to make it available for Mac App Store. Please use [issues](https://github.com/nwjs/nw.js/issues) to request this, and it will be worked on in the future versions of NW.js.

Delete `.DS_Store` files that may have been generated when you were working on your app:

```bash
cd YourApp.app && find . -name "*.DS_Store" -type f -delete
```

Remove the `crash_inspector` file, if it exists in your app:

```bash
rm YourApp.app/Contents/Frameworks/crash_inspector
```
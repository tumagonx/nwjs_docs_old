Our compilation will follow `Chromium`'s standard, by using `gclient` and `gyp`, be sure to read following documentations before you continue, they contained required knowledge to build `node-webkit`.

* [Get the Code](http://www.chromium.org/developers/how-tos/get-the-code) - the usage of `gclient`
* [UsingNewGit ](http://code.google.com/p/chromium/wiki/UsingNewGit) - steps of syncing code 

And choose the instructions for your platform, they contained important building conventions.

* [Build Instructions for Windows](http://www.chromium.org/developers/how-tos/build-instructions-windows)
* [Build instructions for Linux](http://code.google.com/p/chromium/wiki/LinuxBuildInstructions)
* [Build instructions for Mac OS X](http://code.google.com/p/chromium/wiki/MacBuildInstructions)

## Prerequisite

1. [Get the Chromium depot_tools](http://www.chromium.org/developers/how-tos/install-depot-tools).
2. Setup building enviroment, see *Build Instructions* above.

## Get the Code

`node-webkit` is now a part of our custom `Chromium`, that means the way we get `node-webkit` is mostly the same with `Chromium`, following steps are indeed modified from the [Get the Code](http://www.chromium.org/developers/how-tos/get-the-code).

First find a place to put our code, it will take up about 14G disk place after compilation. Assume you store code under `node-webkit` folder, our final directory architecture will be like:

    node-webkit/
    |-- .gclient
    `-- src/
        |-- many-stuff
        |-- ...
        `-- content
            |-- ...
            `-- nw

Then create the `.gclient` file under `node-webkit`, its content should be:

    solutions = [
       { "name"        : "src",
         "url"         : "https://github.com/zcbenz/chromium.git@node",
         "deps_file"   : ".DEPS.git",
         "managed"     : True,
         "custom_deps" : {
           "src/third_party/WebKit/LayoutTests": None,
           "src/chrome_frame/tools/test/reference_build/chrome": None,
           "src/chrome_frame/tools/test/reference_build/chrome_win": None,
           "src/chrome/tools/test/reference_build/chrome": None,
           "src/chrome/tools/test/reference_build/chrome_linux": None,
           "src/chrome/tools/test/reference_build/chrome_mac": None,
           "src/chrome/tools/test/reference_build/chrome_win": None,
         },
         "safesync_url": "",
       },
    ]

Finally sync code under `node-webkit` directory (where `.gclient` resides), it would spend a few hours depending on your network condition:

    gclient sync

Note: if you're on Linux and you get any dependency errors during `gclient sync` (like 'nss' or 'gtk+-2.0'), run `./src/build/install-build-deps.sh`, then re-run gclient sync:

    cd /path/to/node-webkit
    # Do this to boot strap dependencies on Linux:
    gclient sync --nohooks
    ./src/build/install-build-deps.sh
    gclient sync

If you encountered other problems, see [UsingNewGit](http://code.google.com/p/chromium/wiki/UsingNewGit).

## Patch WebKit

We have done some hacks on WebKit to make it comfortable with local apps and node.js, currently you should manually apply our patch for WebKit.

    cd /path-to-node-webkit/src/third_party/WebKit
    git apply ../../content/nw/patches/webkit.patch

## Build

After the `gclient sync`, project files should have be prepared. If not, you should manually run:

    ./build/gyp_chromium

Then you can just compile the `nw` target:

* Windows - Open the Visual Studio solution file and build project `nw` under `content`. 
* Mac OS-X - Open the Xcode project file and build `nw`. 
* Linux - Run `make -j4 nw` from the Chromium `src` directory. 

Alternately, you can use `ninja` to build `node-webkit`, see [NinjaBuild](http://code.google.com/p/chromium/wiki/NinjaBuild). This method is also recommended since it's very fast and easy to use.

## Build Faster

* [Build Instructions for Windows](http://www.chromium.org/developers/how-tos/build-instructions-windows#TOC-Accelerating-the-build)
* [LinuxFasterBuilds](http://code.google.com/p/chromium/wiki/LinuxFasterBuilds)
* [WindowsPrecompiledHeaders](http://code.google.com/p/chromium/wiki/WindowsPrecompiledHeaders)
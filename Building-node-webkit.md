Our compilation will follow `Chromium`'s standard, by using `gclient` and `gyp`, be sure to read following documentations before you continue, they contained required knowledge to build `node-webkit`.

* [Get the Code](http://www.chromium.org/developers/how-tos/get-the-code) - the usage of `gclient`
* [UsingNewGit ](http://code.google.com/p/chromium/wiki/UsingNewGit) - steps of syncing code 
* [BranchesAndBuilding](http://code.google.com/p/chromiumembedded/wiki/BranchesAndBuilding) - how to build `CEF`

And choose the instructions for your platform, they contained important building conventions.

* [Build Instructions for Windows](http://www.chromium.org/developers/how-tos/build-instructions-windows)
* [Build instructions for Linux](http://code.google.com/p/chromium/wiki/LinuxBuildInstructions)
* [Build instructions for Mac OS X](http://code.google.com/p/chromium/wiki/MacBuildInstructions)

## Prerequisite

1. [Get the Chromium depot_tools](http://www.chromium.org/developers/how-tos/install-depot-tools).
2. Setup building enviroment, see *Build Instructions* above.

## Get the Code

Currently `node-webkit` is based on `CEF`, which is based on `Chromium`, so we need to get both projects' source code. Following steps are roughly same with `CEF`'s [building steps](http://code.google.com/p/chromiumembedded/wiki/BranchesAndBuilding).

### Get Chromium's Code

First find a place to put our code, it will take up about 14G disk place after compilation. Assume you store code under `node-webkit` folder, our
final directory architecture will be like:

    node-webkit/
    |-- .gclient
    `-- src/
        |-- many-stuff
        |-- ...
        `-- cef

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

`dbc6308eb7` is in fact the last commit hash of [my chromium repository](https://github.com/zcbenz/chromium), change it as you will.

Finally sync code under `node-webkit` (it would spend a few hours depending on your network condition):

    gclient sync

Note: if you're on Linux and you get any dependency errors during `gclient sync` (like 'nss' or 'gtk+-2.0'), run `./src/build/install-build-deps.sh`, then re-run gclient sync:

    cd /path/to/node-webkit
    # Do this to boot strap dependencies on Linux:
    gclient sync --nohooks
    ./src/build/install-build-deps.sh
    gclient sync

If you encountered other problems, see [UsingNewGit](http://code.google.com/p/chromium/wiki/UsingNewGit).

### Get CEF's code

Currently our project just patches `CEF` and is independent of `Chromium`, so you still need to put `CEF`'s code under `node-webkit/src`:

    cd /path/to/node-webkit/src
    git clone --recursive https://github.com/zcbenz/cef.git

## Build

Run the `cef_create_projects` script (.bat on Windows, .sh on OS-X and Linux) to generate the build files based on the GYP configuration.

    cd /path/to/node-webkit/src/cef
    ./cef_create_projects.sh

Compile `node-webkit`

* Windows - Open the Visual Studio solution file and build. 
* Mac OS-X - Open the Xcode project file and build. 
* Linux - Run `make -j4 nw` from the Chromium `src` directory. 

Alternately, the `build_projects` script (.bat on Windows, .sh on OS-X and Linux) can be used to build on the command line using the default system tools. 

    cd /path/to/chromium/src/cef/tools
    build_projects.sh Debug

## Build Faster

* [Build Instructions for Windows](http://www.chromium.org/developers/how-tos/build-instructions-windows#TOC-Accelerating-the-build)
* [LinuxFasterBuilds](code.google.com/p/chromium/wiki/LinuxFasterBuilds)
* [WindowsPrecompiledHeaders](http://code.google.com/p/chromium/wiki/WindowsPrecompiledHeaders)

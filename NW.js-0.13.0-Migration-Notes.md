*This page is subject to change during the development of NW.js 0.13.x*

## Migrate from NW.js 0.12.x or older

### Architecture Changes
1. `require('nw.gui')` does **NOT** exist. Instead, you can directly access NW.js API via `nw.*`.
1. Global Node.js objects, such as `require`, `process`, `Buffer` etc., do **NOT** exist. All Node.js objects are now loaded in `nw.*`. Just use `nw.require('http')` to require Node.js modules.
1. If NW.js is running under [[Mixed Context]] mode (boot NW.js with `--mixed-context` argument), `nw.*` is kind of mirror of `window.*`. In this mode, you **CANNOT** share variables among frames or windows by assigning it to Node context. So do **NOT** turn on Mixed Context mode if your application is heavily depending on this variable sharing feature.

### Node.js Changes
1. Node.js is bumped to 5.x in latest build. Check your NPM modules to make sure they support Node.js 5.x. If you have dependencies of native modules, check on [this list](https://github.com/nodejs/node/issues/2798) to see whether you have a compatible version.

### API Changes
1. Different build flavors support different set of APIs and capabilities. See [[Build Flavors]] to choose the right NW.js flavor or [build your own](Building nw.js).
1. `Shortcut` API does **NOT** map <kbd>Ctrl</kbd> modifier to <kbd>&#8984;</kbd> on Mac OS X. However 0.13.0 supports `Command` modifier in cross platform way. So it's your responsible to detect the OS and choose the right modifier when registering hotkeys. See [[Shortcut]].

## Migrate from Chrome Apps
TBD
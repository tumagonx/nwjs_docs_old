Every app package should contain a manifest file named `package.json`, it will tell `node-webkit` how to open the app and control how the browser behaves.

## Quick Start

```json
{
  "main": "index.html",
  "name": "nw-demo",
  "description": "demo app of node-webkit",
  "version": "0.1",
  "keywords": [ "demo", "node-webkit" ],
  "window": {
    "icon": "link.png",
    "toolbar": true,
    "width": 800,
    "height": 500,
    "position": "mouse",
    "min_width": 400,
    "min_height": 200,
    "max_width": 800,
    "max_height": 600
  },
  "webkit": {
    "plugin": true
  }
}
```

## Basic Format

`package.json` should be written in the format of [JSON](http://www.json.org/).

## Required Fields

Each package must provide all the following fields in its package descriptor file:

### main
*(string)* which page should be opened when `node-webkit` starts.

### name 
*(string)* the name of the package. This must be a unique, lowercase alpha-numeric name without spaces. It may include "." or "_" or "-" characters. It is otherwise opaque. 

`name` should be globally unique since `node-webkit` will store app's data under the directory named `name`.

## Features Control Fields

Following fields control which features `node-webkit` should provide and how `node-webkit` should open the main window.

### nodejs
*(boolean)* set `nodejs` to false will disable Node support in WebKit.

### node-main
*(string)* specify the path to a node.js script file, it will be executed on startup in Node context. This script will have same runtime environment with normal node.js binary. The script is ensured to run before the first DOM window load starting from v0.3.3. [See here](node-main) for more information. _(available after node-webkit v0.3.1)_

### single-instance
*(boolean)* by default node-webkit only allows one instance of your app if your app is a standalone package (packaged with instructions of [[How to package and distribute your apps]]), if you want to allow multiple instances of your app running at the same time, specify this to `false`.

### window
*(object)* controls how the main window looks, see _Window Subfields_ below.

### webkit
*(object)* controls what features of WebKit should be on/off, see _WebKit Subfields_ below.

### user-agent
_since v0.3.7_

*(string)* Override the `User-Agent` header in HTTP requests made from the application.
The following placeholders are available:

* `%name`: replaced by the `name` field in the manifest.
* `%ver`: replaced by the `version` field in the manifest, if available.
* `%nwver`: replaced by the version of node-webkit.
* `%webkit_ver`: replaced by the version of WebKit engine.
* `%osinfo`: replaced by the OS and CPU information you would see in browser's user agent string.

### node-remote
_since v0.3.7_

*(string)* Enable calling Node in remote pages. The value controls for which sites this feature should be turned on. The format is the same with the "proxy bypass rules" of the browser:

It's a rule set, which rules are separated by `,` or `;`. Each rule can be any of the following:

```CPP
  // (1) [ URL_SCHEME "://" ] HOSTNAME_PATTERN [ ":" <port> ]
  //
  //   Match all hostnames that match the pattern HOSTNAME_PATTERN.
  //
  //   Examples:
  //     "foobar.com", "*foobar.com", "*.foobar.com", "*foobar.com:99",
  //     "https://x.*.y.com:99"
  //
  // (2) "." HOSTNAME_SUFFIX_PATTERN [ ":" PORT ]
  //
  //   Match a particular domain suffix.
  //
  //   Examples:
  //     ".google.com", ".com", "http://.google.com"
  //
  // (3) [ SCHEME "://" ] IP_LITERAL [ ":" PORT ]
  //
  //   Match URLs which are IP address literals.
  //
  //   Conceptually this is the similar to (1), but with special cases
  //   to handle IP literal canonicalization. For example matching
  //   on "[0:0:0::1]" would be the same as matching on "[::1]" since
  //   the IPv6 canonicalization is done internally.
  //
  //   Examples:
  //     "127.0.1", "[0:0::1]", "[::1]", "http://[::1]:99"
  //
  // (4)  IP_LITERAL "/" PREFIX_LENGHT_IN_BITS
  //
  //   Match any URL that is to an IP literal that falls between the
  //   given range. IP range is specified using CIDR notation.
  //
  //   Examples:
  //     "192.168.1.1/16", "fefe:13::abc/33".
  //
  // (5)  "<local>"
  //
  //   Match local addresses. The meaning of "<local>" is whether the
  //   host matches one of: "127.0.0.1", "::1", "localhost".
```
### chromium-args
_since v0.4.0_

*(string)* Specify chromium command line arguments. It will be useful if you want to distribute the app with some custom chromium args. For example, if you want to disable the GPU accelerated video display, just add `"chromium-args" : "--disable-accelerated-video"`. If you want to add multiple arguments, separate each two arguments by space. This field can take a number of flags in one argument as well, via enclosing them in single quotation marks.

### js-flags
_since v0.4.1_

*(string)* Specify the flags passed to JS engine(v8). e.g. turn on Harmony Proxies and Collections feature:

```json
{
  "name": "nw-demo",
  "main": "index.html",
  "js-flags": "--harmony-proxies --harmony_collections"
}
```

## Window Subfields

### title
*(string)* the default title of window created by node-webkit, it's very useful if you want to show your own title when the app is starting.

### width/height
*(int)* the initial width/height of the main window.

### toolbar
*(boolean)* should the navigation toolbar be showed.

### icon
*(string)* path to window's icon

### position
*(string)* be `null` or `center` or `mouse`, controls where window will be put

### min_width/min_height
*(int)* minimum width/height of window

### max_width/max_height
*(int)* maximum width/height of window

### as_desktop
*(boolean)* show as desktop background window under X11 environment

### resizable
*(boolean)* whether window is resizable _(available after node-webkit v0.3.0)_

### always-on-top
*(boolean)* whether the window should always stay on top of other windows _(available after node-webkit v0.3.4)_

### fullscreen
*(boolean)* whether window is fullscreen _(available after node-webkit v0.3.0)_

### frame

*(boolean)* specify it to `false` to make the window frameless _(available after node-webkit v0.3.0)_

### show
*(boolean)* specify it to `false` if you want your app to be hidden on startup _(available after node-webkit v0.3.0)_

### kiosk
*(boolean)* whether to use `Kiosk` mode. In `Kiosk` mode, the app will be fullscreen and try to prevent users from leaving the app, so you should remember to provide a way in app to leave `Kiosk` mode. This mode is mainly used for presentation on public displays _(available after node-webkit v0.3.1)_

## WebKit Subfields

_Following fields requires node-webkit >= v0.3.0._

### plugin
*(boolean)* whether to load external browser plugins like Flash, default to false.

### java
*(boolean)* whether to load Java applets, default to false.

### page-cache

*(boolean)* whether to enable page cache, default to false.

## Other Fields

The [Packages/1.0](http://wiki.commonjs.org/wiki/Packages/1.0) standard specifies many other fields `package.json` should provide. currently we don't make use of them, but you provide them still.

### description 
a brief description of the package. By convention, the first sentence (up to the first ". ") should be usable as a package title in listings. 

### version 
a version string conforming to the [Semantic Versioning requirements](http://semver.org/). 

### keywords 
an Array of string keywords to assist users searching for the package in catalogs. 

### maintainers 
Array of maintainers of the package. Each maintainer is a hash which must have a "name" property and may optionally provide "email" and "web" properties. For example: 

````
"maintainers":[ {
   "name": "Bill Bloggs",
   "email": "billblogs@bblogmedia.com",
  " web": "http://www.bblogmedia.com",
}]
````

### contributors 
an Array of hashes each containing the details of a contributor. Format is the same as for author. By convention, the first contributor is the original author of the package. 

### bugs 
URL for submitting bugs. Can be mailto or http. 

### licenses 
array of licenses under which the package is provided. Each license is a hash with a "type" property specifying the type of license and a url property linking to the actual text. If the license is one of the official open source licenses the official license name or its abbreviation may be explicated with the "type" property. If an abbreviation is provided (in parentheses), the abbreviation must be used. 

````
"licenses": [
   {
       "type": "GPLv2",
       "url": "http://www.example.com/licenses/gpl.html",
   }
]
````

### repositories 
Array of repositories where the package can be located. Each repository is a hash with properties for the "type" and "url" location of the repository to clone/checkout the package. A "path" property may also be specified to locate the package in the repository if it does not reside at the root. For example: 

````
"repositories": [
       {
            "type": "git", 
            "url": "http://github.com/example.git",
            "path": "packages/mypackage"
       }
]
````
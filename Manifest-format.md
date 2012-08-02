Every app package should contain a manifest file named `package.json`, it will tell `node-webkit` how to open the app and control how the browser behaves.

## Quick Start

````
{
  "main": "index.html",
  "name": "demo",
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
    "webgl-disabled": true
  }
}
````

## Basic Format

`package.json` should be written in the format of [JSON](http://www.json.org/).

## Required Fields

Each package must provide all the following fields in its package descriptor file:

### main
*(string)* which page should be opened when `node-webkit` starts.

## Features Control Fields

Following fields control which features `node-webkit` should provide and how `node-webkit` should open the main window.

### window
*(object)* controls how the main window looks, see _Window Fields_ below.

### webkit
*(object)* controls what features should be on/off, see _WebKit Fields_ below.

## Window Sub Fields

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

## WebKit Sub Fields

Coming soon.

## Other Fields

The [Packages/1.0](http://wiki.commonjs.org/wiki/Packages/1.0) standard specifies many other fields `package.json` should provide. currently we don't make use of them, but you provide them still.

### name 
the name of the package. This must be a unique, lowercase alpha-numeric name without spaces. It may include "." or "_" or "-" characters. It is otherwise opaque. 

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

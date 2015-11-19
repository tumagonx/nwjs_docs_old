Since 0.12.0, NW.js supports multiple build flavors, such as Mac App Store (MAS), Native Client (NaCl), SDK and normal builds.

* **MAS flavor** is compatible with the requirements of Mac App Store. So you can submit your NW.js app to Mac App Store with the Mac flavor. *new for 0.12.0*
* **SDK flavor** has builtin support for DevTools and NaCl plugins. SDK flavor has the same capabilities as the builds before 0.13.0. *new for 0.13.0-alpha1*
* **NaCL flavor** supports Native Client (NaCl) plugins, but has no builtin DevTools. *new for 0.13.0-alpha1*
* **Normal flavor** is a minimum build without DevTools and NaCl plugin support. *new for 0.13.0-alpha1*
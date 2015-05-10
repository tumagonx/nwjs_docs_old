![nw13 arch](http://nwjs.io/images/arch.png)

Upstream is moving the extension mechanism towards the Content layer, as well as componentizing the browser modules (https://www.chromium.org/developers/design-documents/browser-components). NW.js will evolve with upstream towards the “app shell” architecture. In future we’ll split the browser components as separate loadable modules so the binary size can be shrunk significantly. 

We are also using it to refactor the implementation of ‘nw.gui’ library in 0.12. The extension mechanism provide a lightweight and elegant solution for JS API binding. With it we are able to eliminate the overhead of additional IPC messages used in previous version.
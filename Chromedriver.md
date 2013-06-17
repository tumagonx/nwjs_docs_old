Since v0.6.0 `chromedriver` is distributed with node-webkit binaries.

From [chromedriver project home page](http://code.google.com/p/chromedriver/):

> WebDriver is an open source tool for automated testing of webapps across many browsers. It provides capabilities for navigating to web pages, user input, JavaScript execution, and more. ChromeDriver is a standalone server which implements WebDriver's wire protocol for Chromium. It is being developed by members of the Chromium and WebDriver teams. 

You can use it with tools like [selenium](http://docs.seleniumhq.org/).

The only difference of the binary files from the official one is: `chromedriver2_server` will search for node-webkit binaries (nw, nw.exe) in the same directory (on OSX it's the same directory with `node-webkit.app`).

## Downloads
https://s3.amazonaws.com/node-webkit/v0.6.0/chromedriver2-nw-v0.6.0-win-ia32.zip

https://s3.amazonaws.com/node-webkit/v0.6.0/chromedriver2-nw-v0.6.0-linux-ia32.tar.gz

https://s3.amazonaws.com/node-webkit/v0.6.0/chromedriver2-nw-v0.6.0-linux-x64.tar.gz

https://s3.amazonaws.com/node-webkit/v0.6.0/chromedriver2-nw-v0.6.0-osx-ia32.zip

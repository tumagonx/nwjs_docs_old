Since 0.8.0 crash dump is supported. It means if node-webkit crashed, a `minidump` file will be saved. Users can include it in the bug report. It would be helpful to find out what's wrong with node-webkit in certain cases. Ideally node-webkit should not crash in all cases.

Crash dumping is enabled on all the 3 platforms and the minidump file will be saved in the following default directories:

 * Linux: /tmp
 * Windows: [System temporary directory](http://msdn.microsoft.com/en-us/library/windows/desktop/aa364992%28v=vs.85%29.aspx)
 * Mac: ~/Library/Breakpad/`product name`  (product name is defined in .plist file in the application bundle)

The dump location can be changed by an API: `App.setCrashDumpDir(dir)`. Note that if the program crashes before setting the new location, the dump file will still be saved in the default location.

## decoding the stack trace ##

To extract the stack trace from the minidump file, you need the `minidump_stackwalk` tool, symbols file of node-webkit binary and the minidump (.dmp) file generated from the crash. 

See http://www.chromium.org/developers/decoding-crash-dumps  http://code.google.com/p/google-breakpad/wiki/GettingStartedWithBreakpad

Symbols file of official node-webkit binary is provided staring from 0.8.0. It can be downloaded from:

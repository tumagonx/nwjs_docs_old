**OK the following is the "leet" solution ;)**

* Download ghex: sudo apt-get install ghex
* Open up nw executable: ghex nw
* Find and replace string udev.so.0 with udev.so.1: Ctrl-F + udev + CR + (replace 0 with 1)

Tip - type 'udev.so.0' into the right hand side of the search box. Press Enter to find this text. Now click on the text **highlighted in red** and change 'udev.so.0' to 'read udev.so.1' 

**=== End quickfix ===**

**The following solution is provided in the same way as Google Chrome is used in their product:**

Due to the removal of `libudev0` and its associated library `libudev.so.0`, node-webkit isn't able to run on newer distributions such as:

 * Ubuntu 13.04
 * Fedora 18, 19
 * Arch
 * Gentoo
 * Derivatives of the above

...and possibly others. Until node-webkit is updated to depend on the currently shipped version `libudev.so.1`, the following solutions *should* provide a stopgap measure for packaging your applications.

**1. Create local symlink to `libudev.so.1`**

Same as above install `libudev1` if needed. Now create a local symlink to `libudev.so.1`. On Ubuntu for example, run from the directory where nw files are extracted:

``` bash
# ln -s /lib/x86_64-linux-gnu/libudev.so.1 ./libudev.so.0
```

Then create a shell script to run nw:

``` bash
#!/bin/sh
LD_LIBRARY_PATH=/home/omi/nw:$LD_LIBRARY_PATH ./nw $*
```

As you are only modifying local contents of node-webkit directory, this option should not have an impact on the overall stability of your system.

**2. Use a wrapper shell script for your application.**
In this way, rename the binary executable file as `myapp-bin`, and then create a shell script named `myapp` as the following. Users run the `myapp` file.

``` shell
#!/bin/bash
export MYAPP_WRAPPER="`readlink -f "$0"`"

HERE="`dirname "$MYAPP_WRAPPER"`"

# Always use our versions of ffmpeg libs.
# This also makes RPMs find the compatibly-named library symlinks.
if [[ -n "$LD_LIBRARY_PATH" ]]; then
  LD_LIBRARY_PATH="$HERE:$HERE/lib:$LD_LIBRARY_PATH"
else
  LD_LIBRARY_PATH="$HERE:$HERE/lib"
fi
export LD_LIBRARY_PATH

exec -a "$0" "$HERE/myapp-bin"  "$@"
```
**Creating a symlink for your package in the postinstall script**

In the postinstall script of your DEB or RPM package, run the following script to create a local symlink. Use this together with the previous wrapper script.
```shell
#!/bin/sh
udevDependent=`which udisks 2> /dev/null` # Ubuntu, Mint
if [ -z "$udevDependent" ]
then
    udevDependent=`which systemd 2> /dev/null` # Fedora, SUSE
fi
if [ -z "$udevDependent" ]
then
    udevDependent=`which findmnt` # Arch
fi
udevso=`ldd $udevDependent | grep libudev.so | awk '{print $3;}'`
if [ -e "$udevso" ]; then
   ln -sf "$udevso" /opt/myapp/libudev.so.0
fi
```
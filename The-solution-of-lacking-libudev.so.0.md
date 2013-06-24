#### WARNING: This solution is dangerous and may destabilize your system. Use at own risk.

Due to the removal of `libudev0` and its associated library `libudev.so.0`, node-webkit isn't able to run on newer distributions such as:

 * Ubuntu 13.04
 * Fedora 18
 * Arch
 * Gentoo
 * Derivatives of the above

...and possibly others. Until node-webkit is updated to depend on the currently shipped version `libudev.so.1`, the following solutions *should* provide a stopgap measure for testing and development purposes (though their safety is not guaranteed):

**1. Create global symlink to `libudev.so.1` by hand.**

install the package `libudev1`, and there is `libudev.so.1` at `/lib/x86_64-linux-gun/libudev.so.1`. On Ubuntu for example, Run:

``` bash
# apt-get install libudev1
# cd /lib/x86_64-linux-gnu/
# ln -s libudev.so.1 libudev.so.0
```

**2. Create local symlink to `libudev.so.1`**

Same as above install `libudev1` if needed. Now create a local symlink to `libudev.so.1`. On Ubuntu for example, run from the directory where nw files are extracted:

``` bash
# ln -s /lib/x86_64-linux-gnu/libudev.so.1 ./libudev.so.0
```

Then create a shell script to run nw:

``` bash
#!bin/bash
LD_LIBRARY_PATH=/home/omi/nw:$LD_LIBRARY_PATH ./nw
```

As you are only modifying local contents of node-webkit directory, this option should not have an impact on the overall stability of your system.

Note that this does not pass the command line parameters along though. Update the script accordingly if you need that option.

**3. Modify the .deb or .rpm package file**

When install your app through the package. We can add some scripts for solving it. At the post install, create the symlink to `libudev.so.1`.  

Select the deb of Ubuntu for example. In the `DEBIAN/postinst` add these code. 

``` shell
get_lib_dir() {
  if [ "$DEFAULT_ARCH" = "i386" ]; then
    LIBDIR=lib/i386-linux-gnu
  elif [ "$DEFAULT_ARCH" = "amd64" ]; then
    LIBDIR=lib/x86_64-linux-gnu
  else
    echo Unknown CPU Architecture: "$DEFAULT_ARCH"
    exit 1
  fi
}

LIBUDEV_0=libudev.so.0
LIBUDEV_1=libudev.so.1

add_udev_symlinks() {
  get_lib_dir
  if [ -f "/$LIBDIR/$LIBUDEV_0" -o -f "/usr/$LIBDIR/$LIBUDEV_0" -o -f "/lib/$LIBUDEV_0" ]; then
    return 0
  fi

  if [ -f "/$LIBDIR/$LIBUDEV_1" ]; then
    ln -snf "/$LIBDIR/$LIBUDEV_1" "/$LIBDIR/$LIBUDEV_0"
  elif [ -f "/usr/$LIBDIR/$LIBUDEV_1" ];
  then
    ln -snf "/usr/$LIBDIR/$LIBUDEV_1" "/$LIBDIR/$LIBUDEV_0"
  else
    echo "$LIBUDEV_1" not found in "$LIBDIR" or "/usr/$LIBDIR".
    exit 1
  fi
}

```
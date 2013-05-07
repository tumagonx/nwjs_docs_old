Now Ubuntu 13.04 has remove the package `libudev0`, but chromium is depending the lib. So the node-webkit can't run on Ubuntu 13.04. The same problem is also appears on Fedora 18. There are some solutions:

*** 1. create the linker to `libudev.so.1` by hand. ***

install the package `libudev1`, and there is `libudev.so.1` at `/lib/x86_64-linux-gun/libudev.so.1`. On Ubuntu for example, Run:

````bash
$ apt-get install libudev1
$ cd /lib/x86_64-linux-gun/
$ ln -s libudev.so.1 libudev.so.0
````

*** 2. through the .deb .rpm. ***

When install your app through the package. We can add some scripts for solving it. At the post install, create the linker to `libudev.so.1`.  

Select the deb of Ubuntu for example. In the `DEBIAN/postinst` add these code. 

````shell
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

````
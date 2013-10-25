AuthenTech AES3500 driver for libfprint
=======================================

[libfprint][project] is the fingerprint library for Linux, which
offers a single API for application developers to access fingerprint
devices. It includes a handful of drivers to support a wide range of
devices.

This repo is setup to help further improve the driver for AES3500, as
well AES4000, both of which share a very similiar interface.

The documents, debugging output, as well as code patches are collected
and shared here for developers to easily pick it up and hack on it.

## About AuthenTech AES3500 and AES4000

AES3500 is a pretty old fingerprint device produced by AuthenTech. It
possesses a press-typed sensor of dimension in 128x128. While AES4000
has a dimension in 96x96.

![AES3500](http://h-wrt.com/pics/fprint.jpg)

The first driver code has been developed by Daniel Drake (the creator of
libfprint itself), for AES4000. Then the support for AES3500 is later
added by tunning params upon AES4000 driver (see `debug/UsbSnoop.log`).

At the moment, while there seems no problem scanning a fingerprint with
the driver, the verification rate is unsatifyingly low. Thus it is
considered *work in progress*. And your contributions are welcome.

Beyond libfprint, there was another (unmaintained) project
[biopod][biopod] trying to bring AES3500 to Linux. And there's the
attempt to install it [in OpenWrt][h-wrt].

## What is included

    |-- debug
    |   |-- lsusb.txt    # description of usb interface
    |   |-- spiked.log   # log translated to "readable" bytes
    |   `-- UsbSnoop.log # a sniffed log of usb transfer in Windows
    |-- doc
    |   |-- aes3500-datasheet.pdf
    |   `-- aes4000-datasheet.pdf
    |-- README.md
    `-- src
	`-- aes3500.patch # code patches by `git format-patch`

The `aes3500.patch` would be empty, if the patch get merged into
libfprint.

## Hacking

You need to build and compile [libfprint][repo] from latest code, with
`aes3500.patch` applied. Then build and compile
[fprint_demo][fprint_demo], which is a GUI application to test
fingerprint capture and verify.

You may need those dependencies, on Debian-based distributions:

* build-essential
* libpam0g-dev
* libmagick++-dev
* libusb-1.0-0-dev
* libgtk2.0-dev
* libnss3-dev

Please follow these commands (# are comments):

    $ # install the dependencies
    $ sudo aptitude install build-essential libpam0g-dev libmagick++-dev
    $ sudo aptitude install libusb-1.0-0-dev libgtk2.0-dev libnss3-dev

    $ # build and compile libfprint
    $ git clone https://github.com/juvenn/aes3500.git
    $ git clone git://anongit.freedesktop.org/libfprint/libfprint
    $ cd libfprint
    $ git checkout -b myhack
    $ # if there're patches
    $ git apply ../aes3500/src/aes3500.patch
    $ ./auotgen.sh
    $ make
    $ # install to /usr/local
    $ sudo make install # install to /usr/local

    $ # build and compile fprint_demo
    $ cd ..
    $ git clone https://github.com/dsd/fprint_demo
    $ cd fprint_demo
    $ ./autogen.sh
    $ # load libfprint library from /usr/local
    $ ./configure PKG_CONFIG_PATH=/usr/local:$PKG_CONFIG_PATH
    $ make
    $ sudo make install

Then you're ready to go:

    $ sudo fprint_demo

## Contribute

Please feel free to share documents, logs, etc that might be helpful for
developing the driver. Pull requests and issues are welcome.

For discussions, please join us on
[fprint@lists.freedesktop.org][mailinglist].

----

Thanks Rafael Toledo for the Windows driver and the help.

* Copyright (C) 2007-2008 [Daniel Drake][@dsd]
* Copyright (C) 2011 [Juvenn Woo][@juvenn]

Licensed under the GPL version 2, the same as libfprint.

[biopod]: http://ww2.cs.fsu.edu/~micsmith/devices/index.html "Biopod project"
[project]: http://www.freedesktop.org/wiki/Software/fprint
     "fprint page at freedesktop"
[mailinglist]: http://www.freedesktop.org/wiki/Software/fprint/Mailing%20list
     "fprint mailing list"
[fprint_demo]: https://github.com/dsd/fprint_demo
[h-wrt]: http://h-wrt.com/en/doc/fprint "Biopod in OpenWrt"
[repo]: http://cgit.freedesktop.org/libfprint/libfprint
[@dsd]: https://github.com/dsd
[@juvenn]: https://github.com/juvenn

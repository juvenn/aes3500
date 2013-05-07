AuthenTech AES3500 driver for libfprint
=======================================

AES3500 is a pretty old fingerprint device produced by AuthenTech. It
possesses a press-typed sensor of dimension in 128x128. Which is
similiar to AES4000, except the later is in 96x96.

This project tries to bring AES3500 to Linux, it is a driver
specifically prepared for libfprint, which enables a lot of fingerprint
devices to be usable in Linux. The driver is a direct fork of Daniel
Drake's AES4000 code, with parameters tuned by monitoring usb transfer
in Windows.

Beyond libfprint, there was another project [biopod][1] trying to bring
AES3500 to Linux.

At the moment, while there seems no problem scanning a fingerprint with
this driver, the verification rate is unsatifyingly low. Thus the driver
is considered *work in progress*. And your contributions are welcome.

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
	|-- aes3500.c    # the driver code readily to be replaced into
	|                # libfprint/drivers/aes3500.c
	`-- aes3500.diff # the git diff intended to merge into libfprint

## Get started to hack

The libfprint is being actively developed under [freedesktop][2], and
you can get the code at http://cgit.freedesktop.org/libfprint/libfprint.

Additionaly, `fprint_demo` is a GUI application that will help you test
your driver visually. Get the code at http://github.com/dsd/fprint_demo.

We need to compile and install `libfprint` and `fprint_demo` manually
first. Assume you've got source code of  `aes3500`, `libfprint`, and
`fprint_demo` in the current directory, following these steps:

    # merge the diff into the master branch of libfprint
    # and install libfprint (default to /usr/local/)
    $ cd libfprint
    $ git apply ../aes3500/src/aes3500.diff
    $ ./autogen.sh
    $ make
    $ sudo make install

    # build and install fprint_demo
    $ cd ../fprint_demo
    $ ./autogen.sh
    $ ./configure PKG_CONFIG_PATH=/usr/local:$PKG_CONFIG_PATH
    $ make
    $ sudo make install

    # everything is ready and try it with fprint_demo
    $ sudo fprint_demo

## Contribute

Please feel free to share documents, logs, etc that might be helpful for
developing the driver. Pull requests and issues are welcome.

For discussions, please join us on [fprint][3].

===

Thanks Rafael Toledo for the Windows driver and the help.

Copyright (C) 2011 Juvenn Woo
Copyright (C) 2007-2008 Daniel Drake

Licensed under the GPL version 2, the same as libfprint.

[1]: http://ww2.cs.fsu.edu/~micsmith/devices/index.html "Biopod project"
[2]: http://www.freedesktop.org/wiki/Software/fprint
     "fprint page at freedesktop"
[3]: http://www.freedesktop.org/wiki/Software/fprint/Mailing%20list
     "fprint mailing list"

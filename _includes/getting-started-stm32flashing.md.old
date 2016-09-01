The `dartino flash` command combines building and flashing in a single step.
This requires a bit of configuration:

* On Mac
  1. We need the libusb library, which we can get with homebrew. If you
 don't already have homebrew, follow the ['install homebrew'](http://brew.sh/)
 instructions.
  1. In a terminal run:
  <br>`brew install libusb`

* On Linux:
  1. We need to enable OpenOCD to use the USB port:
  <br>`sudo cp platforms/{{include.platformdir}}/config/49-stlinkv2-1.rules /etc/udev/rules.d`
  1. Unplug and replug the board to reload the rules

Then to get the program onto the board simply use:

```
dartino flash <filename>.dart
```

*Note*: If you get the error *"jtag status contains invalid mode value -
communication failure"* when flashing, your board likely has outdated flashing
firmware. You can update to version V2j27 or later using the [ST-LINK firmware
upgrader](http://www.st.com/content/st_com/en/products/embedded-software/development-tool-software/stsw-link007.html).

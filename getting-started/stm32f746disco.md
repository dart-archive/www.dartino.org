---
title: Dartino Getting Started
layout: page
---

# Getting started with an STM32746 Discovery

This page walks you through running your first Dartino programs on a F746
Discovery board.

* [What you will need](#what-you-will-need)
* [Installing the SDK](#installing-the-sdk)
* [Running your first program](#running-your-first-program)
* [Running on the board](#running-on-the-board)
* [Next steps](#next-steps)

## What you will need

To develop embedded programs with Dartino, you will need the following:

* A developer PC (running MacOS or Linux) where you write the programs

* A [STM32 F746 Discovery](http://www.st.com/stm32f7-discovery) development
board

* A MiniUSB cable to connect the F746 and your developer PC

* Optional: A breadboard and a collection of components for running some of
 the samples (will be discussed later)

## Installing the SDK

First download the SDK. This is available as a '.zip' archive; pick the one that
matches the OS of the PC you will be using for development:

* <a href="https://storage.googleapis.com/dartino-archive/channels/dev/release/latest/sdk/dartino-sdk-macos-x64-release.zip"
onclick="ga('send', 'event', 'Downloads', 'MacOS SDK');">Dartino SDK for MacOS (64-bit)</a>
* <a href="https://storage.googleapis.com/dartino-archive/channels/dev/release/latest/sdk/dartino-sdk-linux-x64-release.zip"
onclick="ga('send', 'event', 'Downloads', 'Linux SDK');">Dartino SDK for Linux (64-bit)</a>

Unzip the SDK, and add the 'dartino' command to the path by typing the below in
a terminal window (on Linux, use `dartino-sdk-linux-x64-release.zip`):

```
cd $HOME
unzip Downloads/dartino-sdk-macos-x64-release.zip
export "PATH=$PATH:$HOME/dartino-sdk/bin"
```

Test if the 'dartino' command works; it should print a version number to the
console:

```
dartino --version
```

## Running your first program

Let’s go ahead and run our first Dartino program. This is a simple program that
prints Hello. In your terminal type:

```
cd $HOME/dartino-sdk/
dartino run samples/general/hello.dart
```

This runs the program in hello.dart on your local machine. You should see output
resembling this:

```
Hello from Darwin running on michael-pc2.
```

Try to open `hello.dart` in your favorite editor (we recommend the [Atom
editor](https://atom.io/) by Github with the
[Dartino plugin](https://atom.io/packages/dartino)). Pretty easy to read, right?

## Running on the board

Let’s make our Hello program run again, this time on the F746 board. Dartino
uses the [GCC ARM toolchain](https://launchpad.net/gcc-arm-embedded) for
building and [OpenOCD](http://openocd.org/) for flashing, so we need to download
those. This can be done easily by running this command in your terminal (total
download size is roughly 120 MB):

```
dartino x-download-tools
```

On Linux (not Mac), we also need need 32-bit libc libraries:

```
sudo apt-get install libc6-i386
```

Now we can compile the program into a flashable image, and flash that image
to the board (on Linux, replace `/Volumes/` with `/media/<username>/`):

```
dartino build samples/general/hello.dart
cp samples/general/hello.bin /Volumes/DIS_F746NG/
```

After a few seconds you should see output resembling this on your board:

```
Hello from FreeRTOS.
```

The flash command compiled the hello.dart program to byte code, and linked that
with the Dartino run-time and the [FreeRTOS](http://www.freertos.org/) embedded
operating system. This created the image file hello.bin which you should see in
your filesystem next to hello.dart.

## Automated flashing

The `dartino flash` command combines building and flashing in a single step.
This requires a bit of configuration:

* On Mac
  1. We need the libusb library, which we can get with homebrew. If you
 don't already have homebrew, follow the ['install homebrew'](http://brew.sh/)
 instructions.
  1. In a terminal run:
  <br>`brew tap libusb`

* On Linux:
  1. We need to enable OpenOCD to use the USB port:
  <br>`sudo cp platforms/stm32f746g-discovery/config/49-stlinkv2-1.rules /etc/udev/rules.d`
  1. Unplug and replug the board to reload the rules

Then to get the program onto the board simply use:

```
dartino flash samples/general/hello.dart
```


## Next steps

Ready for some more fun? Take a look at our
[samples](/samples/stm32f746disco.html), and read more about the [dartino
command](/tool.html).

And don’t forget to send us some feedback, and ask some [questions](/faq.html).

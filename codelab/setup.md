---
title: Dartino codelab setup
layout: page
---

# Dartino Codelab setup

These instructions will help you prepare for the [Dartino codelab](index.html).

The current codelab uses Dartino SDK version 0.4.0.

## Step 0: Getting started

If you have a developer PC running Linux or MacOS you can install the SDK
directly on your PC. If you have a PC running Windows, or if you prefer not to
install things locally, you can use a pre-prepared Linux image running in
VirtualBox.

### Option 1: Using a pre-prepared image in VirtualBox

This option is supported on Windows and MacOS.

First download the pre-prepared image: <a href="https://storage.googleapis.com/dartino-archive/channels/dev/raw/0.4.0-dev.1.1/sdk/codelab.ova"
onclick="ga('send', 'event', 'Downloads', 'VirtualBox VM');">VirtualBox Image File</a>. NOTE: This is a very large file; approx.
2.3 GB.

Next install [VirtualBox](https://www.virtualbox.org/).

Finally, start VirtualBox and select `File->Import Appliance`. Select the VM
image file you downloaded. Then start that image.

Once Linux boots you will be logged in. If you need the user name it is
`dartino` and the password is `dar/tino.` (the . is part of the password).

Test that everything is working by opening a Terminal and typing:

```
dartino --version
```

You should see `0.4.0-dev.1.1` on the screen.

### Option 2: Using a local installation of the SDK

This option is supported only on MacOS, and Linux. It requires a fair bit of
configuration; if you find this too cumbersome, use the VirtualBox approach
described in option 1.

First download the SDK. This is available as a '.zip' archive; pick the one that
matches the OS of the PC you will be using for development:

* <a href="https://storage.googleapis.com/dartino-archive/channels/dev/raw/0.4.0-dev.1.1/sdk/dartino-sdk-macos-x64-release.zip"
onclick="ga('send', 'event', 'Downloads', 'MacOS SDK');">Dartino SDK for MacOS (64-bit)</a>
* <a href="https://storage.googleapis.com/dartino-archive/channels/dev/raw/0.4.0-dev.1.1/sdk/dartino-sdk-linux-x64-release.zip"
onclick="ga('send', 'event', 'Downloads', 'Linux SDK');">Dartino SDK for Linux (64-bit)</a>

Unzip the SDK, and add the 'dartino' command to the path by typing the following
in a terminal window (on Linux, use `dartino-sdk-linux-x64-release.zip`):

```
cd $HOME
unzip Downloads/dartino-sdk-macos-x64-release.zip
export "PATH=$PATH:$HOME/dartino-sdk/bin"
```

Install SDK dependencies. Note: this will download another 120 MB. In a Terminal
type:

```
dartino x-download-tools
```

Install required support libraries, and configure USB:

* On Mac
  1. We need the libusb library, which we can get with homebrew. If you
 don't already have homebrew, follow the ['install homebrew'](http://brew.sh/)
 instructions.
  1. Then in a terminal run:
  <br>`brew install libusb`

* On Linux:
  1. We need 32-bit support libraries; in a Terminal run: `sudo apt-get install libc6-i386 lib32stdc++6`
  1. We need to configure USB; in a Terminal run: `sudo cp dartino-sdk/platforms/stm32f746g-discovery/config/49-stlinkv2-1.rules /etc/udev/rules.d`
  1. Reload rules; in a Terminal run: `sudo udevadm control --reload-rules`

The Dartino tools are based on the [Atom editor](http://atom.io). This is a
highly customizable editor created by the team behind GitHub. Install Atom from
[http://atom.io](http://atom.io), and then install the [Dartino
plugin](https://atom.io/packages/dartino).

Next, test if the 'dartino' command works by typing the following in a
Terminal; it should print `0.4.0-dev.1.1` on the screen.

```
dartino --version
```


### Running a small test program

Finally, test the installation by running a small 'Hello World' program. In a
Terminal type:

```
dartino run dartino-sdk/samples/general/hello.dart
```

You should see `Hello from Linux running on ubox.` on the screen.

So what happened? `dartino run` compiled the code in `hello.dart`, and then ran
the compiled code in a local Virtual Machine (VM). The program read the
current OS name, and the network name of the PC, and then printed those to the
screen (note: thus you will see different output if you use a Mac).

If either of these do not work, something is wrong with your Dartino SDK
installation, and that should be resolved before proceeding.

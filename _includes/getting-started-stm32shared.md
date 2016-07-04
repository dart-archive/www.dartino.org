# Getting started with an {{include.boardname}}

This page walks you through running your first Dartino programs on a
{{include.boardname}} board.

* [What you will need](#what-you-will-need)
* [Installing the SDK](#installing-the-sdk)
* [Running your first program](#running-your-first-program)
* [Running on the board](#running-on-the-board)
* [Next steps](#next-steps)

## What you will need

To develop embedded programs with Dartino, you will need the following:

* A developer PC (running MacOS or Linux) where you write the programs

* A [{{include.boardname}}]({{include.boardurl}}) development board

* A MiniUSB cable to connect the board and your developer PC

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

Try to open `hello.dart` in your favorite editor (we recommend the [Dartino
IDE](/guides/atom)). Pretty easy to read, right?

## Running on the board

### A bit more configuration

Let’s make our Hello program run again, this time on the board. Dartino
uses the [GCC ARM toolchain](https://launchpad.net/gcc-arm-embedded) for
building and [OpenOCD](http://openocd.org/) for flashing, so we need to download
those. This can be done easily by running this command in your terminal (total
download size is roughly 120 MB):

```
dartino x-download-tools
```

On Linux (not Mac), we also need need 32-bit libc libraries:

```
sudo apt-get install libc6-i386 lib32stdc++6
```

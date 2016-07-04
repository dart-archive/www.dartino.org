---
title: Dartino Getting Started
layout: page
---

# Getting started with an Raspberry Pi 2

This page walks you through running your first Dartino programs on a Raspberry Pi 2.

* [What you will need](#what-you-will-need)
* [Installing the SDK](#installing-the-sdk)
* [Running your first program](#running-your-first-program)
* [Preparing your Raspberry Pi 2](#preparing-your-raspberry-pi-2)
* [Connecting your Raspberry Pi 2 to the network](#connecting-your-raspberry-pi-2-to-the-network)
* [Running on the Raspberry Pi 2](#running-on-the-raspberry-pi-2)
* [Next steps](#next-steps)

## What you will need

To develop embedded programs with Dartino, you will need the following:

* A developer PC (running MacOS or Linux) where you write the programs

* A Raspberry Pi 2 embedded device for running the programs

* A MicroSD card to hold the operating system for
 the Raspberry Pi 2, and some kind of SD card reader

* A network connection between the developer PC and the Raspberry Pi 2

* Optional: A breadboard and a collection of components for running some of
 the samples (will be discussed later)

![What you need photo](/images/setup-photo.jpg)

## Installing the SDK

First download the SDK. This is available as a '.zip' archive; pick the one that
matches the OS of the PC you will be using for development:

* <a href="https://storage.googleapis.com/dartino-archive/channels/dev/release/latest/sdk/dartino-sdk-macos-x64-release.zip"
onclick="ga('send', 'event', 'Downloads', 'MacOS SDK');">Dartino SDK for MacOS (64-bit)</a>
* <a href="https://storage.googleapis.com/dartino-archive/channels/dev/release/latest/sdk/dartino-sdk-linux-x64-release.zip"
onclick="ga('send', 'event', 'Downloads', 'Linux SDK');">Dartino SDK for Linux (64-bit)</a>

Unzip the SDK, and add the 'dartino' command to the path by typing the
below in a terminal window:

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
prints Hello. In your command line type:

```
cd $HOME/dartino-sdk/
dartino run samples/general/hello.dart
```

This runs the program in hello.dart on your local machine. You should see output
resembling this:

```
Hello from Darwin running on michael-pc2.
```

Try to open `hello.dart` in your favorite editor. We recommend the [Atom
editor](https://atom.io/) by Github with the [Dart
plugin](https://github.com/dart-atom/dartlang/). Pretty easy to read, right?
(Note: you will get some Analyzer warnings in Atom as we don't fully support it
yet. You can ignore those.)

### Dartino and sessions

But what actually happened when we asked the ```dartino``` command to run
`hello.dart`? When you ask ```dartino``` to run the program, it compiles the
program to byte code, and then passes the byte code to the local Dartino VM for
execution. The VM passes back the result, and Dartino prints it to your command
line.

![Dartino architecture diagram](/images/architecture-diagram.png)

Now let’s get things running in a *remote session* that is connected to your
Raspberry Pi 2!

## Preparing your Raspberry Pi 2

First we need to prepare an SD card with an image that contains the standard
Raspbian operating system, and the Dartino binaries for Raspberry Pi 2.

This step uses a script that automates the download of Raspbian, and performs
the needed configuration. If you prefer to configure the image manually, see the
[manual install instructions](/getting-started/manual-install/).

Start by opening a terminal window, and enter the following command (*note*: you
will be prompted to enter your password as the script performs system level
operations):

```
platforms/raspberry-pi2/flash-sd-card
```

The script will take you through the following steps:

1. *Specify which SD card to use*: You will be asked to remove all SD cards, and
then insert *just* the one you wish to use for Dartino. ***Important***:
Everything on this SD card will be erased during the installation process.

1. *Specify device name*: The name your Raspberry Pi 2 device will be given on
the network.

1. *Specify IP address*: If you are using automatically allocated IP addresses
assigned by a router with DHCP just press enter. To specify a static IP, for
example when using a direct connection (see below), enter the desired IP
address.

1. *Downloading*: Download of the Raspbian base image. This can take up to 10
minutes depending on the speed of your Internet connection. A progress indicator
will be shown.

1. *Flashing*: Flashing of the image onto the SD card. This can take up to 10
minutes depending on the speed of your SD card. A progress indicator
will be shown.

## Connecting your Raspberry Pi 2 to the network

Once the steps in the script are completed, and you see the ```Finished flashing
the SD card``` message, remove the SD card from your PC and insert it into the
Raspberry Pi 2.

Next, we need to ensure your PC can communicate with the Raspberry over the
network.

### Option 1: Router-based connection

In this option, both your PC and Raspberry are connected to the same networking
router.

1. Connect the Raspberry to your router with an Ethernet cable.

1. Connect your PC to the same router.

### Option 2: Direct connection

If you do not wish to use a router, or if the Raspberry is not permitted to join
your network, you can connect it directly to your PC.

1. Connect the Raspberry to your PC with an Ethernet cable. If your PC does not
have an empty Ethernet port, you can use an USB Ethernet adapter.

### Testing the connection

After connecting using option 1 or 2, test the connection by running this
command:

```
dartino show devices
```

If the connection is working, you should see output resembling ```Device at
192.168.1.2     dartinopi.local```.

## Running on the Raspberry Pi 2

Let’s make our Hello program run again, this time on the Raspberry Pi 2. All we
need is to specify to the ```dartino run``` command that we want to run in a
remote session. Type the following on your local developer PC:

```
dartino run samples/general/hello.dart in session remote
```

The first time you run in the remote session you will be asked to specify the IP
address. Select from the list of devices discovered on the network, or manually
enter the IP address, e.g. ```192.168.2.2```.

You should see output resembling this on your screen:

```
Hello from Linux running on dartinopi.
```

Did you notice the difference from when we ran in the local session? As before
Dartino compiled the hello.dart program to byte code, but this time rather than
passing it to the local VM via the local session it passed it to the Raspberry
Pi via the remote session. On the Raspberry Pi, the Dartino VM Agent made sure
that a VM (Virtual Machine) was spun up, and the program was executed on it. The
result of the program (the printing to the console) was passed back by the VM to
the 'dartino' command on the developer PC, and it was printed to the local
console.

## Next steps

Ready for some more fun? Take a look at our [samples](/samples/), and read
more about the [dartino command](/guides/tool/).

And don’t forget to send us some feedback, and ask some [questions](/faq/).

## Security notes

Note that on the Raspberry Pi 2, the 'dartino' command manages a VM
Agent on the attached Raspberry Pi. Please be aware that in the current
experimental implementation this agent runs with full admin privileges. VMs
spawned by the agent run with similar privileges, and it will listen to all
incoming network traffic. We recommend you run the Raspberry Pi on an isolated
network.

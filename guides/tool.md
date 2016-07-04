---
title: Dartino tool details
layout: page
---

# Dartino tool details

The `dartino` tool is a command line program that offers all the features
you need to develop including tasks such as compiling and running code, flashing
devices, and [debugging](/guides/debugging/).

It is complemented by the [Dartino Atom IDE](/guides/atom/).

### General:

* Run a program on the local PC:<br>
`dartino run <program>`
* See the version of Dartino:<br>
`dartino --version`
* Analyze a program for error and warnings::<br>
`dartino analyze <program>`
* See all available commands:<br>
`dartino help`

### Micro-controllers only (e.g. ST32F746):

* Install dependencies:<br>
`dartino x-download-tools`
* Build a flashable image file:<br>
`dartino build <program>`
* Build and flash a program to a device:<br>
`dartino flash <program>`

### Micro-processors only (e.g. Raspberry Pi 2):

* Run a program on a connected device:<br>
`dartino run <program> in session remote`
* Debug a program on a connected device:<br>
`dartino debug <program> in session remote`

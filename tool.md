---
title: Dartino tool details
layout: page
---

# Dartino tool details

## Main commands

The following are the main commands of the `dartino` tool.

### General:

* Run a program on the local PC:<br>
`dartino run <program>`
* See the version of Dartino:<br>
`dartino --version`
* See all available commands:<br>
`dartino help`

### Micro-controllers only (e.g. ST32F746):

* Install dependencies:<br>
`dartino x-download-tools`
* Build a flashable image file:<br>
`dartino build <program>`
* Flash a program to a device:<br>
`dartino flash <program>`

### Micro-processors only (e.g. Raspberry Pi 2):

* Run a program on a connected device:<br>
`dartino run <program> in session remote`
* Debug a program on a connected device:<br>
`dartino debug <program> in session remote`

## Debugging details

Dartino for the Raspberry Pi 2 supports an early preview of our debugger. A
later release will support debugging on all device types, and will have a
debugger UI in Atom.

Let's try to debug the Knight Rider sample. Start by running the
following command in your terminal:

```
dartino debug $HOME/dartino-sdk/samples/raspberry_pi/basic/knight-rider.dart in session remote
```

You should see the terminal change to:

```
Starting session. Type 'help' for a list of commands.

>
```

Let's set a breakpoint in the setLeds method, and start the execution of the
program:

```
b _setLeds
r
```

We are now inside the setLeds method. Let's see what the initial state is:
Type ```p```. You should see this output

```
ledToEnable: 0
this: Instance of 'Lights'
>
```

Try to step a few more times (with the ```s``` command), and then print out the
local variable again (with the ```p``` command). You should see ledToEnable
increment up to the number of LEDs you have, and then you should see it start
decrementing. Pretty neat right!?

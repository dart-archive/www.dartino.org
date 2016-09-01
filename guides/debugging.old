---
title: Dartino tool details
layout: page
---

# Dartino debugging

In traditional embedded development you spend vast amounts of time debugging
memory layouts, fixing pointer errors, and other low-level fiddling. In Dartino
you write high-level application code and debug high-level application code!
This is facilitated by the Dartino debugger, which is available both as a
textual debugger in the terminal and a UI-based debugger in the IDE.

Let's try to debug one of the samples!

## Debugging using the textual debugger

### Enabling debugging in an image

To enable debugging, you need to flash an image that has debugging enabled. This
is easily done with an option, and requires no change to your source code:

```
dartino flash --debugging-mode --no-wait samples/stm32f746g-discovery/disco-light.dart
```

### Attaching the debugger

Next we need to attach the debugger to the running image. This uses a serial
connection over USB. Use the following command (note, on a Mac use
/dev/tty.usbmodemXXXXX):

```
dartino debug samples/stm32f746g-discovery/disco-light.dart on tty /dev/ttyACM0
```

The debugger should start, and show output like this:

```
Attached to /dev/ttyACM0
Starting session. Type 'help' for a list of commands.

### Process is running. Press Ctrl + \ to interrupt it.
```

### Stopping and starting

To stop the execution of the program, press `Ctrl + \`. The program will stop,
and the debugger will print the location in the application code where the
execution was halted:

```
^\samples/stm32f746g-discovery/disco-light.dart:30:44
30         display.writeText(10, 10, 'Rendered $lineCounter lines');
```

To continue the execution, use the command `continue`. You can stop and continue
multiple times. You can press 'enter' to repeat the previous command, such as
`continue`.

### Breakpoints and stepping

To stop the execution at a specific location use breakpoints. You can add a
breakpoint to a method entry using `b [method name]` or an absolute position
using `bf <file> [line] [column]`. Let's add one right before a new line is
drawn on the screen:

```
bf disco-light.dart 24
```

We can now step through the code using `c`, which caused the program to run
until the breakpoint is hit again. You should see a new line being drawn for
each `c` command.

### Inspecting state

When the execution is stopped you can inspect the local state using the `p`
command. This prints all the local variables:

```
> p
color: Instance of 'Color'
lineCounter: 112709
rnd: Instance of '_Random'
h: 272
w: 480
display: Instance of 'FrameBuffer'
discoBoard: Instance of 'STM32F746GDiscovery'
>
```

To see additional details for a variable, use `p <variable name>` for simple
variables, `p <name>.<subname>` for a nested member, and `p * <name>` for
compound types:

```
> p color.r
220
> p *color
Instance of 'Color' {
  Color.r: 220
  Color.g: 114
  Color.b: 67
}
```

Pretty neat right!?

## Debugging using the IDE debugger

The debugger UI in the IDE is currently under development, and is planned to
support all the same functionality, and can be more convenient for managing
breakpoints and inspecting state.

Stay tuned for updates!

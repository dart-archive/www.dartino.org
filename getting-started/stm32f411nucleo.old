---
title: Dartino F411 Getting Started
layout: page
---

{% include getting-started-stm32shared.md boardname="STM32F411RE Nucleo"  boardurl="http://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mcu-eval-tools/stm32-mcu-eval-tools/stm32-mcu-nucleo/nucleo-f411re.html" %}

### Selecting a target

The SDK is configured to use the [STM32F746](/getting-started/stm32f746disco/)
board per default. We need to change to the STM32F411RE board:

  * Open the file `<dartino sdk location>/internal/.dartino-settings` in a text editor.
  * Uncomment the line `// "device": "stm32f411re-nucleo",`.
  * Save the file.

### Building and flashing

Now we can build and flash a program to the STM32F411RE (on Linux, replace
`/Volumes/` with `/media/<username>/`):

```
dartino build samples/stm32f411re-nucleo/blinky.dart
cp samples/stm32f411re-nucleo/blinky.bin /Volumes/NODE_F411RE/
```

After a few seconds you should see the LED on the board blinking!

The build command compiled the blinky.dart program to byte code, and linked that
with the Dartino run-time and the [FreeRTOS](http://www.freertos.org/) embedded
operating system. This created the image file blinky.bin which you should see in
your filesystem next to blinky.dart.

## Automated flashing

{% include getting-started-stm32flashing.md platformdir="stm32f411re-nucleo" %}

## Next steps

Ready for some more fun? Take a look at our
[samples](/samples/stm32f411nucleo/), and read more about the [dartino
command](/guides/tool/).

And don’t forget to send us some feedback, and ask some [questions](/faq/).

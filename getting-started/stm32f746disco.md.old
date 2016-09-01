---
title: Dartino F746 Getting Started
layout: page
---

{% include getting-started-stm32shared.md boardname="STM32F746 Discovery"  boardurl="http://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mcu-eval-tools/stm32-mcu-eval-tools/stm32-mcu-discovery-kits/32f746gdiscovery.html" %}

### Building and flashing

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

The build command compiled the hello.dart program to byte code, and linked that
with the Dartino run-time and the [FreeRTOS](http://www.freertos.org/) embedded
operating system. This created the image file hello.bin which you should see in
your filesystem next to hello.dart.

## Automated flashing

{% include getting-started-stm32flashing.md platformdir="stm32f746g-discovery" %}

## Next steps

Ready for some more fun? Take a look at our
[samples](/samples/stm32f746disco/), and read more about the [dartino
command](/guides/tool/).

And don’t forget to send us some feedback, and ask some [questions](/faq/).

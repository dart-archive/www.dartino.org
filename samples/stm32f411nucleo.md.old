---
title: Dartino F411 samples
layout: page
---

# Dartino samples for STM32F411RE Nucleo

We have a number of STM32F411RE sample programs available in the
`samples/stm32f411re-nucleo` folder. Let’s take a look at the code, and get
familiar with the platform.

* [blinky.dart](#Blinky)
  * The 'hello world' of embedded: Blink an LED
  * Requires a STM32F411RE Nucleo board

## Blinky

Embedded devices are most commonly used to collect data and perform some kind of
control task via attached sensors and output devices such as LEDs. Take a look
at the `blinky.dart` program located in the `samples/raspberry_pi2/basic/`
folder. This blinks the on-board LED on the Nucleo board.



First the program initializes a STM32F411RENucleo helper object, and an output
PIN:

```dart
STM32F411RENucleo board = new STM32F411RENucleo();
GpioOutputPin pin = board.gpio.initOutput(STM32F411RENucleo.LED2);
```

Next, it simply loops and toggles the state of the LED:

```dart
while (true) {
  pin.toggle();
  sleep(500);
}
```

Pretty easy, right!?

---
title: Why Dartino
layout: page
---

<h1 class="why">Why Dartino?</h1>

Dartino is an **experimental** open-source project that enables you to write
software for embedded systems with much higher productivity than existing native
approaches.

<h2 class="why">Highly productive and familiar language</h2>

Dartino is powered by the [Dart
language](https://www.dartlang.org/docs/dart-up-and-running/ch02.html), a modern
programming language. The Dart language uses a familiar syntax with clean
semantics, so you can probably already read and even write Dart code!

```dart
import 'package:gpio/gpio.dart';
import 'package:stm32f746g_disco/stm32f746g_disco.dart';

main() {
  // Initialize board and configure LED1 as an output pin
  STM32F746GDiscovery board = new STM32F746GDiscovery();
  GpioOutputPin ledPin = board.gpio.initOutput(STM32F746GDiscovery.LED1);

  // Set the pin to a true/high state
  ledPin.state =  true;
}
```

<h2 class="why">Highly productive</h2>

Dartino has a rich set of libraries which do all the heavy lifting. For
example, send HTTP requests using secure SSL/TLS with just a few lines of code:

```dart
var socket = new TLSSocket.connect("httpbin.org", 443);
var request = new HttpRequest("/ip");
request.headers["Host"] = "httpbin.org";
var response = new HttpConnection(socket).send(request);
```

And then easily parse the response using a powerful JSON library:

```dart
Map data = JSON.decode(new String.fromCharCodes(response.body));
String ip = data["origin"];
print("Response: $ip");
```

<h2 class="why">Get started quickly</h2>

Dartino includes the popular FreeRTOS embedded OS, and board support packages
and configurations for several popular embedded development boards. With that,
you will have code running on actual hardware in less than a minute:

```
dartino flash myprogram.dart
```

<h2 class="why">Fast and lean runtime</h2>

When you call the `dartino flash` command, we compile your program and bundle
that in the image with the fast and safe Dartino runtime. Thus Dartino code runs
much faster than other high-level embedded programming languages, yet is much
easier to use than traditional embedded C.

<h2 class="why">Free and open</h2>

Dartino is free and open-source
([license](https://github.com/dartino/sdk/blob/master/LICENSE.md)). This
includes all key parts: Dartino developer tools, IDE, libraries, and runtime.
The development is facilitated [on GitHub](https://github.com/dartino/sdk).

---
title: Dartino codelab
layout: page
---

# Dartino Codelab

## What you will need

To develop embedded programs with Dartino in this codelab, you will need the
following:

* A developer PC (running MacOS, Linux, or Windows) where you write the programs

* A [STM32F746G Discovery](http://www.st.com/stm32f7-discovery) development
board

* A MiniUSB cable to connect the development board and your developer PC

## Step 0: Getting started

Please follow the steps in [the setup instructions](setup.html).

## Step 1: Getting acquainted with Dartino programs

We will do some very basic programming to get started. Create a directory to
hold your codelab programs (e.g. `~/codelab/`) and in that dir create a new file
called `fib.dart`, and open the file for editing:

```
cd ~/codelab
touch fib.dart
nano fib.dart
```

Next add a main function:

```
main() {
  print('Fib(1) is: ');
}
```

Save the file (use `CTRL-O` + `CTRL-X`), and run it locally (`dartino run
fib.dart` in a Terminal) and make sure it prints. Try to change the string that
is printed, and run it again to make sure it still works.

Next, we will try to calculate [Fibonacci
numbers](https://en.wikipedia.org/wiki/Fibonacci_number). Add a new function
in fib.dart with this interface:

```
int fib(int f) {
  // TODO: Calculate fib of f
  return 0;
}
```

Next, fill in the implementation of fib. And then call it from main. You can
print variables like this:

```
main() {
  int f = 3;
  int r = fib(f);
  print('Fib($f) is: $r');
}
```

Then add a
[for-loop](https://www.dartlang.org/dart-tips/dart-tips-ep-8.html) in the main
program that calls fib on a range of numbers and prints the result. Run it.

### Experiments

Next, you can experiment with both a recursive variant, and a non-recursive one.
You will find that the non-recursive function is much faster.

## Step 3: Getting acquainted with the Atom editor

The Dartino tools are based on the [Atom editor](http://atom.io). This is a
highly customizable editor created by the team behind GitHub.

To open Atom, go to the Terminal and cd to your codelab dir:

```
cd ~/codelab/
```

Next, we need to tell Atom that this dir holds Dartino programs. Currently this
is indicated by the presence of a file called `dartino.yaml`. The file can be
empty, so just create one:

```
touch dartino.yaml
```

Next, start Atom with that dir as the project directory:

```
atom .
```

You should see a file tree on the left, and inside that fib.dart. Try to click
that file. You should see your program, and there should be syntax highlighting.

Look at the bottom status bar. It should say `Analyzing source...`. When
that is completed, the IDE will be validating your program in the background as
you type. Try to make and error in the program (like remove one of the braces).
You should get feedback that there is an error.

Next, try to use the code navigation functionality. In the main function where
fib is invoked, right-click in `fib` and select 'Go to Declaration'. The cursor
should navigate to where fib is defined. You can also navigate to system
libraries. Try to go to the declaration of print! The keyboard shortcut for this
is `cmd-alt-down`, and you can navigate back with `cmd-alt-up`.

Next, try to mess up the formatting of your code, and then select `cmd-alt-b`.
Your code is now nicely formatted!

In steps below, if you wonder how some system library API works, you can use the
go to declaration functionality to inspect the API.

## Step 4: Running on the development board

So far we have run the program locally on the PC using `dartino run`. When this
is invoked, the program is compiled and then executed using a Dartino VM running
on your local PC. This way of running is fast, and great for programs that do
not depend on hardware access. However, to test programs that communicate with
the actual hardware on the development board, we need to run the program on the
board itself. Let's try that.

Connect the board to your PC using the USB cable. You need to use the MiniUSB
connector on the board.

Then, in a terminal, type:

```
dartino flash samples/general/hello.dart
```

You should see the program being compiled, and then once flashing starts you
should see the LED next to the MiniUSB connector on the board flashing green and
red. Once the flashing stops, the board will reboot and run the program. Flip
the board over, and on the display of the board you should see `Hello from
FreeRTOS`.

Note how the text printed is different from when we ran locally -- can you guess
why? If you are curious you can [read more about
FreeRTOS](http://www.freertos.org/).

### Experiments

 - You can also flash directly from the Atom IDE by clicking the 'Play' icon in
 the toolbar. Try to change the hello program, and flash from Atom! Note that
 you need to select which is the 'active' program in the drop-down next to the
 play button.

## Step 5: GPIO and basic hardware access

Embedded devices are most commonly used to collect data and perform some kind of
control task via attached sensors and output devices such as LEDs.

We will be communicating with the components on the board using a
[GPIO](https://en.wikipedia.org/wiki/General-purpose_input/output) (general
purpose input/output) interface. For custom hardware you would usually prototype
this using a
[breadboard](http://www.instructables.com/id/How-to-use-a-breadboard/), but in
this codelab we will just use the components that are already soldered onto the
development board.

Create a new file `gpio.dart` in your `codelab` directory, and add the following
code:

```
import 'dart:dartino';
import 'package:stm32/stm32f746g_disco.dart';
import 'package:gpio/gpio.dart';

main() {
  STM32F746GDiscovery discoBoard = new STM32F746GDiscovery();
}

```

Go to Declaration on the `STM32F746GDiscovery` type that is in front of
`discoBoard`. Towards the top you will see that constants named `LED1` and
`Button1` have been defined for the on-board green LED and blue button. Flip
over your board and locate these -- the blue button is in one corner, and the
green LED is marked 'LD1', and is right below the black button. If we had a
custom breadboard we could have defined similar constants ourselves mapping a
logical name to a GPIO pin.

Let's start with a small program that turns on the LED:

```
main() {
  STM32F746GDiscovery discoBoard = new STM32F746GDiscovery();
  GpioOutputPin led1 = discoBoard.gpio.initOutput(STM32F746GDiscovery.LED1);
  led1.high();
}
```

GPIO pins can either be set to a high or low signal. When a pin mapped to an LED
is high, the LED is powered. Flash the program and verify that the LED is turned
on.

### Experiments

Next, experiment with the following:

1. Add a loop where you alternate between turning the LED on and off to create a
blinking effect. You can use a `while (true)`-loop, `led1.high()`, `led1.low()`,
and `sleep(1000)` statements. You can also try `led1.toogle()`;

1. Try to read from GPIO. You need a `GpioInputPin` mapped to the blue button.

1. Combine the state of the LED and button pins so that the LED is on when the
button is pressed, and off when not.

## Step 6: Rendering graphics on the display

Next, lets use that nice big display that is on the front of the board. Here is
some boilerplate code -- put this in a new file `graphics.dart`:

```
import 'package:stm32/stm32f746g_disco.dart';
import 'package:stm32/lcd.dart';

main() {
  // Initialize display, and get display dimensions.
  STM32F746GDiscovery discoBoard = new STM32F746GDiscovery();
  var display = discoBoard.frameBuffer;
  display.backgroundColor = Color.black;
  int w = display.width;
  int h = display.height;

  // Draw some graphics.
  display.clear(Color.black);
  var color = new Color(200, 100, 200);
  display.drawLine(10, 10, 100, 100, color);
  display.writeText(10, 200, 'HELLO :-)');
}
```

Flash it and see what it does.

### Experiments

1. Look at the graphics API and see what else you can do.

1. Try to create utility functions that draw shapes like squares and triangles.

1. Try to capture touch events. Checkout
`~/dartino-sdk/samples/stm32f746g-discovery/touch.dart` for inspiration.

## Step 7: Turtle graphics (!)

[Turtle graphics](https://en.wikipedia.org/wiki/Turtle_graphics) were popular in
the early days of computing in the Logo programming, and have since found their
way to just about any platform with a display. Let's get them on our embedded
device! Here is a starting point -- paste this into a new file `turtle.dart`:

```
import "dart:math";
import 'package:stm32/stm32f746g_disco.dart';
import 'package:stm32/lcd.dart';

main() {
  // Initialize display, and Turtle with initial location.
  STM32F746GDiscovery discoBoard = new STM32F746GDiscovery();
  Turtle t = new Turtle(discoBoard.frameBuffer);

  // Draw an angle.
  t.forward(100);
  // t.turnRight(90);
  // t.forward(100);

  // Sleep eternally (to keep the display contents fixed)
  while (true) { };
}

class Turtle {
  FrameBuffer _display;
  int _x;
  int _y;
  int _direction = 90;
  Color penColor = Color.red;

  /// Create a new Turtle, and set initial location.
  Turtle(this._display, {xPosition: 0, yPosition: 0}) {
    _display.clear();
    _x = 100;
    _y = 100;
  }

  /// Move forward [distance] pixels in the current direction with the pen down.
  forward(int distance) {
    double r = _direction * (PI / 180.0);
    int dx = (distance * sin(r)).toInt();
    int dy = (distance * cos(r)).toInt();

    _display.drawLine(_x, _y, _x + dx, _y + dy, penColor);

    this._x += dx;
    this._y += dy;
  }

  /// Turn left [degrees] degrees.
  turnLeft(int degrees) {
    // TODO
  }

  /// Turn right [degrees] degrees.
  turnRight(int degrees) {
    // TODO
  }

  /// Move to [x, y] with the pen up.
  flyTo(int x, int y) {
    // TODO
  }
}
```

### Experiments

Experiment with the following:

1. Complete the missing implementations of the `turn` and `flyTo` functions.

1. Make the turtle do more in the `main` method.

1. Add any other turtle functions you see fit.

1. Have the turtle draw advanced graphics, such as Hilbert curves.

## Step 8: HTTP web service requests

It's time to exercise the 'I' in 'IoT' (Internet of Things)!

Create a new file `weather.dart` in your `~/codelab/` dir. Paste in the
following code to get started:

```
import 'dart:convert';
import 'dart:dartino.ffi';
import 'dart:dartino' show sleep;
import 'package:socket/socket.dart';
import 'package:http/http.dart';
import 'package:stm32/ethernet.dart';

// TODO: Add your own APPID from openweathermap.org here.
var appId = '';
// TODO: Change this to the location you are interested in.
var location = 'Aarhus';

main() {
  // If we are running on a dev board, we need to initialize the network.
  if (Foreign.platform == Foreign.FREERTOS) {
    initializeNetwork();
  }

  // Get the weather for
  WeatherInfo wd = new WeatherInfo(location);
  wd.update();
  print(wd);
}

class WeatherInfo {
  String location;
  int temperature;

  WeatherInfo(this.location);

  String toString() {
    String result = '';
    result += "Current weather in '$location':\n";
    result += " - temperature: $temperature\n";
    return result;
  }

  update() {
    // Create and send the http request.
    var uri = new Uri(
      path: "/data/2.5/weather",
      queryParameters: {
        'q': location,
        'APPID': appId,
        'units': 'metric'
      }
    );
    var host = 'api.openweathermap.org';
    var socket = new Socket.connect(host, 80);
    var http = new HttpConnection(socket);
    var request = new HttpRequest(uri.toString());
    request.headers["Host"] = host;
    print("Sending an ${uri.toString()} request to $host:80");
    var response = http.send(request);

    // If we got back status OK/200, parse the JSON formatted response.
    if (response.statusCode == HttpStatus.OK) {
      Map data = JSON.decode(new String.fromCharCodes(response.body));
      temperature = data['main']['temp'];
    }
    socket?.close();
  }
}


// Initialize the network stack and wait until the network interface has either
// received an IP address using DHCP or given up and used the provided
// fallback [address].
const fallbackAddress = const InternetAddress(const <int>[192, 168, 0, 10]);
const fallbackNetmask = const InternetAddress(const <int>[255, 255, 255, 0]);
const fallbackGateway = const InternetAddress(const <int>[192, 168, 0, 1]);
const fallbackDnsServer = const InternetAddress(const <int>[8, 8, 8, 8]);

void initializeNetwork({
  InternetAddress address: fallbackAddress,
  InternetAddress netmask: fallbackNetmask,
  InternetAddress gateway: fallbackGateway,
  InternetAddress dnsServer: fallbackDnsServer}) {

  if (!ethernet.InitializeNetworkStack(address, netmask, gateway, dnsServer)) {
    throw "Failed to initialize network stack";
  }

  while (NetworkInterface.list().isEmpty) {
    sleep(10);
  }
}
```

The program requires a unique client ID (`APPID`), see the notes at the top of
the code. After adding that try to run the program. You should get back the
current temperature in Aarhus.

### Experiments

Experiment with the following:

1. Try a different location.

1. Get back additional details such as pressure, weather description, etc. You
will need to extend the `WeatherData` class to have variable holding those,
extend the JSON parsing, and extend the printing to the screen.

1. Get a forecast from the [5 day / 3 hour forecast
API](http://openweathermap.org/forecast5), or one of the other [weather
APIs](http://openweathermap.org/api) from OpenWeatherMap.

1. Call any other web service you are interested in.

## Step 9: Concurrent programs

Writing concurrent programs can be hard. Writing concurrent programs on embedded
systems can be especially daunting when using C programming, OS threading, and
low level synchronization primitives. But, Dartino has great support for this,
so it's much more feasible, and hopefully more portable across devices as more
Devices get Dartino support.

In this codelab we will try out Fibers, a easy to use concurrency mechanism that
supports co-operative multitasking, a form of multitasking where several Fibers
run in sequence using explicit delegation to the next Fiber.

Create a new file `touch.dart` with the following contents:

```
import 'dart:dartino';
import 'package:stm32/lcd.dart';
import 'package:stm32/stm32f746g_disco.dart';
import 'package:stm32/ts.dart';

var disco = new STM32F746GDiscovery();
var touchScreen = disco.touchScreen;
var display = disco.frameBuffer;

void main() {
  display.clear(Color.white);

  Fiber.fork(handleCounter);
  Fiber.fork(handleTouch);
  handleMain();
}

void handleMain() {
  while (true) {
    // Main doesn't do anything in this sample, so we just sleep.
    // This implicitly yields.
    sleep(100);
  }
}

void handleCounter() {
  int i = 0;
  while (true) {
    i++;
    display.writeText(10, 20, 'Counter: $i');
    Fiber.yield();
  }
}

void handleTouch() {
  while (true) {
    TouchState t = touchScreen.state;
    if (t != null && t.count >= 1) {
      display.writeText(10, 50, 'Touch registered at ${t.x[0]}, ${t.y[0]}    ');
    }
    Fiber.yield();
  }
}
```

### Experiments

Experiment with the following:

1. Try to combine some of your earlier code into a concurrent program. For
example, you could have a program that listens for touch events, controls a
turtle, and makes web requests all concurrently

## Step 10: Free form experimentation

You have now tried all the major components of an embedded app:

* Communicating with hardware components using GPIO

* Rendering graphics on the display

* Making HTTP web requests

If you have more time, try to combine things. For example:

1. Have multiple boards communicate. Currently Dartino has no socket server
support, so you will need a PC to play the role of server.

1. Connect the board to a mobile phone app. For example, can you control the LED
from a phone app?

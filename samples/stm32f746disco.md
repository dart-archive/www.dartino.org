---
title: Dartino project samples
layout: page
---

# Dartino samples for STM32F746

We have a number of STM32F746 sample programs available in the
`samples/stm32f746g-discovery` folder. Let’s take a look at the code, and get
familiar with the platform.

* [disco-light.dart](#disco-light)
  * Basic LCD sample rending a disco-light effect on the display
  * Requires a STM32F746 Discovery board

* [/turtle-graphics/turtle-lcd.dart](#turtle-graphics)
  * LCD sample rending Turtle graphics on the display
  * Requires a STM32F746 Discovery board

## Disco light ##

Take a look at the `disco-light.dart` program located in the
`samples/stm32f746g-discovery/` folder. This sample shows basic rendering of
graphics to the LCD screen on the board.

First the program uses the board support class to retrieve the FrameBuffer
controlling the screen. It also gets the width and height of the display.

```dart
STM32F746GDiscovery discoBoard = new STM32F746GDiscovery();
var display = discoBoard.frameBuffer;
display.backgroundColor = Color.black;
int w = display.width;
int h = display.height;
```

Next, we run in an eternal loop where we draw lines starting at the middle of
the screen (i.e., x and y coordinates of half width/height). We also use the
Random class to generate colors with arbitrary RGB values:

```dart
// Loop eternally drawing random lines.
Random rnd = new Random(42);
int lineCounter = 0;
while (true) {
  // Render a line of random color from the centre to a random location.
  var color = new Color(rnd.nextInt(255), rnd.nextInt(255), rnd.nextInt(255));
  display.drawLine(w ~/ 2, h ~/ 2, rnd.nextInt(w), rnd.nextInt(h), color);
```

Finally, we keep count of how many lines we have drawn, and for every 100 lines
(checked using modulo) we clear the screen, and update a status text stating how
many lines that have been drawn so far:

```dart
lineCounter += 1;
if ((lineCounter % 100) == 0) {
  display.clear(Color.black);
  display.writeText(10, 10, 'Rendered $lineCounter lines');
}
```

Remember, to run this program on your board just use the `dartino flash
samples/stm32f746g-discovery/disco-light.dart` command. Wasn't it easy to do
basic rendering on the screen?

## Turtle graphics ##

Turtle graphics is a fun way of rendering 2D graphics. Let's render some fun
shapes like this:

![Photo of Turtle graphics](/images/turtle-graphics.jpg)

We start by modeling the Turtle itself. As this might be used for several
purposes, we put this code in a seperate file,
`/samples/stm32f746g-discovery/turtle-graphics/turtle.dart`. This contains the
Turtle class, which has a single constructor that takes in a display
FrameBuffer, and optionally also initial x and y coordinates:

```dart
Turtle(this._display, {xPosition: 0, yPosition: 0}) {
  _display.clear();
  _x = xPosition;
  _y = yPosition;
}
```

The primary method of the Turtle -- move forward -- calculates the x and y
deltas using the `math` library, and then draws a line using the `lcd` library:

```dart
forward(int distance) {
  double r = _direction * (PI / 180.0);
  int dx = (distance * sin(r)).toInt();
  int dy = (distance * cos(r)).toInt();

  _display.drawLine(_x, _y, _x + dx, _y + dy, penColor);

  this._x += dx;
  this._y += dy;
}
```

The main program itself (located in
`/samples/stm32f746g-discovery/turtle-graphics/turtle-lcd.dart`), uses the
turtle class to render graphics such as a triangle:

```dart
import 'turtle.dart' show Turtle;

main() {
  // Initialize display, and Turtle with initial location.
  STM32F746GDiscovery discoBoard = new STM32F746GDiscovery();
  Turtle t = new Turtle(discoBoard.frameBuffer, xPosition: 50, yPosition: 50);

  // Draw a triangle.
  for (int i = 0; i < 3; i++) {
    t.forward(20);
    t.turnRight(120);
  }
  ...
}
```

Finally, we illustrate the use of a helper class, TurtleDriver, for rendering
more advanced graphics. This class draws Hilbert curves using recursion:

```dart
class TurtleDriver {
...
  /// Draw an [order] deep Hilbert curve with [sideLength] long sides.
  void hilbert(int sideLength, int order) {
    _hilbertRecur(sideLength, order, 90);
  }
...
}
```

We suggest you take a look at the full implementation of the TurtleDriver class
in `turtle-lcd.dart`, and experiment with drawing larger Hilbert curves by
changing the main call to that class: `td.hilbert(10, 4);`. (Note: if you change
the level, for example 4 to 6, make sure to make the side length smaller also,
for example 10 to 3, so that the curve fits on the screen.)

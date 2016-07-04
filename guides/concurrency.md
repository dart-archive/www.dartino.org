---
title: Dartino concurrency
layout: page
---

# Concurrency support in Dartino

Writing concurrent programs can be very difficult in traditional embedded stack
as it usually requires low-level OS primitives. In Dartino several high-level
forms of concurrency are supported in the stack itself:

* *Fibers*: Light-weight, co-operatively scheduled multitasking.

* *Processes*: pre-emptively scheduled isolated threads of execution
 (similar to OS Threads), which may only share immutable data or communicate
 through ports.

A standard Dartino program is executed as a single Process with a single Fiber
running the `main()` method. As a developer you can create more Fibers and
Processes.

# Fibers

The most simple concurrency construct is the “Fiber”. These are very
light-weight execution threads that all run in a shared process with shared
memory.

Only one Fiber will run concurrently, but execution can easily be delegated to
the next Fiber by explicitly calling `Fiber.yield()`, or implicitly when a Fiber
sleeps or is waiting on a channel.

## Counting with Fibers

Consider the following piece of code (available in the SDK as
`/samples/general/concurrency-fiber-counter.dart`). This forks off two Fibers
each running the `entry()` method. The main program itself is implicitly running
in a Fiber, so a total of three Fibers will be running.

```dart
import 'dart:dartino';
// A shared counter incremented by all Fibers.
int sharedCounter = 0;
// An `id` counter used to identify each Fiber.
int fiberId = 0;

void main() {
  Fiber.fork(entry);  // Fiber 1.
  Fiber.fork(entry);  // Fiber 2.
  entry();            // Current Fiber, i.e. Fiber 3.
}

void entry() {
  int currentFiber = fiberId++;
  print('Fiber $currentFiber spawned');

  while (sharedCounter < 10) {
    print("Fiber ${currentFiber}: shared counter is ${sharedCounter}");
    sharedCounter++;

    print('Fiber $currentFiber yielding');
    Fiber.yield();
  }
}
```

The program illustrates how the Fibers share state allocated in the global
scope (the `sharedCounter` variable), but each have their own state for
variables defined at the method scope (the `currentFiber` variable). It produces
the following output:

```
$ dartino run fiber-counting.dart
Fiber 0 spawned
Fiber 0: shared counter is 0
Fiber 0 yielding
Fiber 1 spawned
Fiber 1: shared counter is 1
Fiber 1 yielding
Fiber 2 spawned
Fiber 2: shared counter is 2
Fiber 2 yielding
Fiber 0: shared counter is 3
Fiber 0 yielding
Fiber 1: shared counter is 4
Fiber 1 yielding
Fiber 2: shared counter is 5
Fiber 2 yielding
Fiber 0: shared counter is 6
Fiber 0 yielding
Fiber 1: shared counter is 7
Fiber 1 yielding
Fiber 2: shared counter is 8
Fiber 2 yielding
Fiber 0: shared counter is 9
Fiber 0 yielding
$
```

## Fibers for code isolation

Fibers can also be used to isolate individual pieces of an app. Consider the
following example (available in the SDK as
`/samples/stm32f746g-discovery/concurrency-fiber-touch-count.dart`), which
structures an app into three pieces:

1. A counter handler which prints a count.
1. A touch handler which prints a touch location.
1. A main program handler (in this sample left empty for simplicity).

```dart
void main() {
  display.clear(Color.white);

  Fiber.fork(handleCounter);
  Fiber.fork(handleTouch);
  handleMain();
}
```

The counter handler simple counts and prints:

```dart
void handleCounter() {
  int i = 0;
  while (true) {
    // Update and print counter.
    i++;
    display.writeText(10, 20, 'Counter: $i');

    // Yield to the next Fiber.
    Fiber.yield();
  }
}
```

The touch handler reads the touch location, and prints the coordinates to the
screen:

```dart
void handleTouch() {
  while (true) {
    // Read and print the touch location.
    TouchState t = touchScreen.state;
    if (t != null && t.count >= 1) {
      display.writeText(10, 50, 'Touch registered at ${t.x[0]}, ${t.y[0]}    ');
    }

    // Yield to the next Fiber.
    Fiber.yield();
  }
}

```

The main handler is left empty here for simplicity, but this could be the main
app handler in a larger app. Note that the sleep call implicitly yields to the
next Fiber.

```dart
void handleMain() {
  while (true) {
    sleep(100);
  }
}
```

The net result is an app that is able to concurrently handle multiple tasks, and
has good code separation where all each tasks needs to do is to yield, and has
no concrete knowledge of what the other Fibers are doing.

# Processes

The Fiber example above works as long as each Fiber "behaves responsibly", and
does not "hog the execution" from the other Fibers. In some cases this is
difficult as the execution duration might now be known, or might vary a lot.
For these cases Dartino supports Processes which are more similar to OS Threads
in that these are fully independent execution threads that are scheduled
pre-emptively. To avoid complicated race conditions, they are only allowed to
share immutable state, or communicate using channels.

The following sample (available in the SDK as
`/samples/general/concurrency-process-fibonacci.dart`) concurrently calculates
running numbers and Fibonacci numbers:

```dart
import 'dart:dartino';

void main() {
  // Spawn a separate process that will calculate Fibonacci numbers.
  Process.spawn(doFibonacci);

  // Run doCount in the the main fiber of the main process.
  doCount();
}

void doCount() {
  int i = 0;
  while (i < 30) {
    i++;
    print("I'm still alive $i");
    sleep(1000);
  }
}

void doFibonacci() {
  int f = 25;
  while (true) {
    f++;
    int result = fibonacci(f);
    print("Fibonacci of $f is $result");
  }
}
```

As the input to Fibonacci grows, that calculation takes increasing longer and
eventually longer than the one second the counter sleeps. However, as we are
using Processes the Fibonacci process is pre-empted and the counter is allowed
to still get a chance to run and update it's counter. The produced output
demonstrates this:

```
$ dartino run concurrency-process-fibonacci.dart
I'm still alive 1
Fibonacci of 26 is 121393
Fibonacci of 27 is 196418
Fibonacci of 28 is 317811
Fibonacci of 29 is 514229
Fibonacci of 30 is 832040
Fibonacci of 31 is 1346269
Fibonacci of 32 is 2178309
Fibonacci of 33 is 3524578
I'm still alive 2
Fibonacci of 34 is 5702887
Fibonacci of 35 is 9227465
I'm still alive 3
Fibonacci of 36 is 14930352
I'm still alive 4
I'm still alive 5
Fibonacci of 37 is 24157817
I'm still alive 6
I'm still alive 7
I'm still alive 8
Fibonacci of 38 is 39088169
I'm still alive 9
I'm still alive 10
...
```

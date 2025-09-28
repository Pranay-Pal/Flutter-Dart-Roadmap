# Topic 8.2: Advanced Asynchrony: Streams and Isolates

[⬅ Previous](topic-8-1-core-concepts.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-9-1-understanding-the-system.md)

While `Future` is perfect for single, one-off asynchronous results, some operations involve a sequence of events over time. For this, Dart provides **Streams**. Additionally, for truly CPU-intensive work that could still freeze your app, Dart offers **Isolates** for parallel processing.

This topic covers these two advanced but essential concepts.

---

### 1. Streams: Handling Sequences of Asynchronous Events

A **`Stream`** is a sequence of asynchronous events. If a `Future` is a promise for a single value in the future, a `Stream` is like a conveyor belt that delivers multiple values over time.

Streams are perfect for:
- Receiving data from a server in chunks (e.g., downloading a large file).
- Listening to user events like mouse clicks or text input.
- Receiving updates from a database or sensor.

A stream can emit three types of events:
1. **Data Events:** The actual values in the sequence.
2. **Error Events:** If something goes wrong during the stream.
3. **"Done" Event:** A signal that the stream has closed and will not produce any more events.

#### Listening to a Stream with `await for`

The easiest way to listen to a stream is with an asynchronous `for` loop (`await for`). The loop will wait for the next data event from the stream, process it, and continue until the stream is closed.

```dart
// This function returns a Stream that emits an integer every second.
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    // Simulate a delay.
    await Future.delayed(Duration(seconds: 1));
    // 'yield' adds a value to the stream.
    yield i;
  }
}

void main() async {
  print('Starting the count...');
  // The 'await for' loop listens to the stream.
  // It will process each number as it arrives.
  await for (final number in countStream(5)) {
    print(number);
  }
  print('Count finished!');
}
```
In this example, the numbers 1, 2, 3, 4, and 5 will be printed, one per second. The `async*` keyword marks the function as an **asynchronous generator**, which is a simple way to create a stream.

#### Transforming Streams

Like `Iterable`, `Stream` has methods like `map`, `where`, and `take` that let you create a new, modified stream from an existing one.

```dart
void main() async {
  Stream<int> numberStream = Stream.fromIterable([1, 2, 3, 4, 5, 6]);

  // Create a new stream that only contains the squares of the even numbers.
  Stream<int> transformedStream = numberStream
      .where((number) => number % 2 == 0) // Only let even numbers pass through.
      .map((evenNumber) => evenNumber * evenNumber); // Square them.

  print('Squares of even numbers:');
  await for (final value in transformedStream) {
    print(value); // Outputs: 4, 16, 36
  }
}
```

---

### 2. Isolates: True Parallelism for CPU-Intensive Work

Dart is a single-threaded language. The event loop does a great job of handling I/O-bound tasks (like network requests) without blocking. However, if you have a truly CPU-intensive task (like parsing a massive JSON file, processing an image, or performing a complex mathematical calculation), it can still freeze your app's UI because it hogs the single thread.

The solution is **Isolates**. An isolate is an independent worker with its own memory and its own event loop. This allows you to run heavy computations on a separate CPU core, leaving your main UI isolate free to remain responsive.

**Key Concept:** Isolates **do not share memory**. They are completely isolated from each other. They can only communicate by passing messages.

#### Simple Parallelism with `Isolate.run()`

The easiest way to use an isolate is with `Isolate.run()`. It runs a function in a new isolate and returns a `Future` with the result. This is perfect for offloading a single, heavy function.

```dart
import 'dart:isolate';

// A function that simulates a very heavy, CPU-bound task.
// Running this on the main thread would freeze the app.
String performHeavyCalculation(String data) {
  print('Isolate: Starting heavy calculation...');
  // Simulate intense work.
  for (int i = 0; i < 2000000000; i++) {
    // This loop keeps the CPU busy.
  }
  print('Isolate: Finished heavy calculation.');
  return 'Processed: $data';
}

void main() async {
  print('Main: Offloading heavy calculation to an isolate.');

  // Isolate.run() takes a function and its argument, runs it in a new isolate,
  // and returns a Future with the result.
  String result = await Isolate.run(() => performHeavyCalculation('My Data'));

  print('Main: Isolate finished. Result: $result');
  print('Main: The UI was responsive while the isolate was working.');
}
```

By using `Isolate.run()`, you can ensure your application remains smooth and responsive, even when performing the most demanding tasks. For more complex, long-running communication between isolates, you can use `Isolate.spawn()` with `SendPort` and `ReceivePort`, but `Isolate.run()` covers a majority of common use cases.

[⬅ Previous](topic-8-1-core-concepts.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-9-1-understanding-the-system.md)

# Topic 13.5: Advanced Asynchronous Programming (`dart:async`)

[⬅ Previous](topic-13-4-data-conversion.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-6-io-files-processes.md)

While `async`/`await` simplifies handling individual `Future`s, the `dart:async` library provides a deeper level of control for managing complex asynchronous scenarios. This includes orchestrating multiple futures, creating and controlling data streams, and scheduling timed events.

- [x] `Future` Management: `Future.wait`, `Future.any`, `Future.microtask`, `Future.sync`.
- [x] Stream Control: `StreamController` (single-subscription vs. broadcast).
- [x] Transforming Streams: `StreamTransformer` and built-in transformers.
- [x] Scheduling and Controlling Execution: `Timer` and `Zone`.

---

### 1. `Future` Management

- **`Future.wait()`**: Waits for multiple futures to complete. If all succeed, it returns a list of their results. If *any* future fails, the entire `Future.wait()` fails immediately.
- **`Future.any()`**: Waits for the *first* future in a collection to complete with a value and returns that value.
- **`Future.microtask()`**: Schedules a function in the high-priority microtask queue, which runs before the next event loop cycle. Use for very short tasks that need to run before any other events.
- **`Future.sync()`**: Creates a future that completes synchronously with the result of a given function. If the function throws, the future completes with an error.

```dart
import 'dart:async';

Future<String> fetchData(String source, Duration delay, {bool fail = false}) {
  return Future.delayed(delay, () {
    if (fail) throw 'Error from $source';
    return 'Data from $source';
  });
}

void main() async {
  // 1. Future.wait() - Success Case
  print('--- Future.wait() ---');
  try {
    final results = await Future.wait([
      fetchData('source1', Duration(seconds: 2)),
      fetchData('source2', Duration(seconds: 1)),
    ]);
    print('Wait results: $results'); // [Data from source1, Data from source2]
  } catch (e) {
    print('Caught error in wait: $e');
  }

  // 2. Future.any()
  print('\n--- Future.any() ---');
  final firstResult = await Future.any([
    fetchData('sourceA', Duration(seconds: 3)),
    fetchData('sourceB', Duration(seconds: 1)),
  ]);
  print('First result: $firstResult'); // 'Data from sourceB'

  // 3. Microtask vs. Event Queue
  print('\n--- Microtask Demo ---');
  Future(() => print('Event Queue: From a regular Future')); // Low priority
  Future.microtask(() => print('Microtask Queue: High priority task')); // High priority
  print('Main: Code after scheduling futures');
  // Execution Order: Main -> Microtask -> Event Queue
}
```

---

### 2. Stream Control with `StreamController`

A `StreamController` is the primary way to create your own streams. It gives you a `Stream` to provide to consumers and a `StreamSink` to add data and errors to.

- **Single-Subscription (Default)**: Only one listener is ever allowed. Events are buffered until a listener subscribes. Good for one-time data flows like a file download.
- **Broadcast**: Created via `StreamController.broadcast()`. Allows any number of listeners. Events are fired immediately and are lost if there are no listeners at that moment. Good for repeated events like button clicks.

```dart
import 'dart:async';

void main() async {
  // Use a broadcast controller to allow multiple listeners.
  final controller = StreamController<int>.broadcast();
  
  // The stream is the public-facing part you give to consumers.
  final stream = controller.stream;

  // The sink is the private part you use to control the stream.
  final sink = controller.sink;

  stream.listen((data) => print('Listener 1 got: $data'));
  stream.where((data) => data.isEven).listen((data) => print('Listener 2 (evens only) got: $data'));

  print('Adding 1 to the stream...');
  sink.add(1);
  await Future.delayed(Duration(milliseconds: 10));

  print('Adding 2 to the stream...');
  sink.add(2);
  await Future.delayed(Duration(milliseconds: 10));

  print('Adding an error...');
  sink.addError('Something went wrong!');

  // Always close the controller when done to release resources.
  controller.close();
}
```

---

### 3. Transforming Streams with `StreamTransformer`

A `StreamTransformer` is a reusable "recipe" for modifying a stream. It takes one stream as input and produces another as output, with the data transformed in some way.

```dart
import 'dart:async';
import 'dart:convert';

void main() {
  final numbers = Stream.fromIterable([1, 2, 3, 4, 5]);

  // 1. Create a custom transformer to filter and map data.
  final transformer = StreamTransformer<int, String>.fromHandlers(
    handleData: (data, sink) {
      if (data.isOdd) {
        sink.add('Odd number: $data');
      }
    },
    handleDone: (sink) {
      sink.add('All done!');
      sink.close();
    },
  );

  // 2. Apply the transformer to the stream.
  print('--- Custom Transformer ---');
  numbers.transform(transformer).listen(print);

  // 3. Chain built-in transformers (e.g., from dart:convert).
  final dataStream = Stream.fromIterable(['line 1', 'line 2', 'line 3']);
  print('\n--- Chained Transformers ---');
  dataStream
      .transform(utf8.encoder) // String to List<int> (UTF-8 bytes)
      .transform(base64.encoder) // List<int> to String (Base64)
      .listen(print);
}
```

---

### 4. Scheduling and Controlling Execution: `Timer` and `Zone`

- **`Timer`**: Executes a function after a duration or at a regular interval.
- **`Zone`**: A `Zone` represents a dynamic context for a piece of code. It can be used to override default behaviors like `print()`, handle all uncaught errors in a specific region of code, or store contextual data.

```dart
import 'dart:async';

void main() {
  // --- Timer Example ---
  print('Scheduling a one-shot timer...');
  Timer(Duration(seconds: 2), () => print('One-shot timer fired!'));

  int counter = 0;
  final periodicTimer = Timer.periodic(Duration(seconds: 1), (timer) {
    counter++;
    print('Periodic timer tick #$counter');
    if (counter >= 3) {
      timer.cancel(); // IMPORTANT: Always cancel periodic timers to prevent memory leaks.
      print('Periodic timer canceled.');
    }
  });

  // --- Zone Example ---
  // Create a new zone with a custom error handler.
  runZonedGuarded(
    () {
      print('\nRunning code inside a guarded zone...');
      // This error would normally crash the program, but the zone will catch it.
      Future.delayed(Duration(seconds: 4), () => throw 'Uncaught async error!');
    },
    (error, stack) {
      // The custom error handler for this zone.
      print('Zone caught an error: $error');
    },
  );
}
```

[⬅ Previous](topic-13-4-data-conversion.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-6-io-files-processes.md)

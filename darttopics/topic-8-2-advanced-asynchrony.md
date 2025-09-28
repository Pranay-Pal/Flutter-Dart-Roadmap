# Topic 8.2: Advanced Asynchrony

[⬅ Previous](topic-8-1-core-concepts.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-9-1-understanding-the-system.md)

    * [ ] Handling data sequences with `Stream`
    * [ ] True parallelism with `Isolates`
    * [ ] Lazy value generation with `sync*`/`async*` (Generators)

#### Handling Data Sequences with `Stream`

```dart
import 'dart:async';

Stream<String> progressUpdates() async* {
  final steps = ['Connecting', 'Uploading', 'Processing', 'Done'];

  for (final step in steps) {
    await Future.delayed(const Duration(milliseconds: 400));
    yield step;
  }
}

Future<void> main() async {
  print('Starting upload...');

  await for (final update in progressUpdates()) {
    print('Status → $update');
  }

  // Transforming streams
  final doubled = Stream<int>.periodic(const Duration(milliseconds: 300), (count) => count)
      .take(5)
      .map((value) => value * 2);

  await for (final value in doubled) {
    print('Transformed stream value: $value');
  }
}
```

Use stream transformations like `map`, `where`, `take`, and `asyncExpand` to shape data as it flows through your app.

#### True Parallelism with `Isolate`

```dart
import 'dart:isolate';

int heavyComputation(int n) {
  if (n <= 1) return n;
  return heavyComputation(n - 1) + heavyComputation(n - 2);
}

Future<void> main() async {
  print('Spawning background isolate...');

  final result = await Isolate.run(() => heavyComputation(35));

  print('Result from isolate: $result');

  // Using a ReceivePort with Isolate.spawn for streamed updates
  final receivePort = ReceivePort();
  await Isolate.spawn(_generateNumbers, receivePort.sendPort);

  await for (final value in receivePort) {
    if (value == null) {
      break;
    }
    print('Isolate stream value: $value');
  }

  receivePort.close();
  print('All isolate work complete.');
}

void _generateNumbers(SendPort sendPort) {
  var counter = 0;
  Timer.periodic(const Duration(milliseconds: 200), (timer) {
    counter++;
    sendPort.send(counter);
    if (counter >= 5) {
      timer.cancel();
      sendPort.send(null); // Signal completion
      Isolate.exit();
    }
  });
}
```

Isolates run in parallel memory heaps, making them perfect for CPU-bound tasks. `Isolate.run` provides a simple request/response pattern, while `Isolate.spawn` lets you exchange messages over ports for continuous updates.

#### Lazy Value Generation with `sync*`/`async*`

```dart
import 'dart:async';

Iterable<int> fibonacci(int length) sync* {
  var current = 0;
  var next = 1;
  for (var i = 0; i < length; i++) {
    yield current;
    final sum = current + next;
    current = next;
    next = sum;
  }
}

Stream<String> pollServerStatus() async* {
  for (var attempt = 1; attempt <= 3; attempt++) {
    await Future.delayed(Duration(milliseconds: attempt * 500));
    yield 'Attempt #$attempt → ${DateTime.now().toIso8601String()}';
  }
}

Future<void> main() async {
  print('First 10 Fibonacci numbers:');
  for (final value in fibonacci(10)) {
    print(value);
  }

  print('\nPolling server status:');
  await for (final status in pollServerStatus()) {
    print(status);
  }
}
```

The `sync*` keyword lazily yields values to an `Iterable`, while `async*` returns a `Stream`. Both allow you to create infinite or on-demand sequences without allocating large collections upfront.

### **Module 9: Null Safety**

[⬅ Previous](topic-8-1-core-concepts.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-9-1-understanding-the-system.md)

# Topic 13.8: True Concurrency with `dart:isolate`

[⬅ Previous](topic-13-7-io-networking.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-9-developer-tools.md)

Dart is a single-threaded language. This means that if you run a computationally expensive task (like parsing a huge JSON file or performing a complex image manipulation), your application's UI will freeze because the single main thread is blocked.

**Isolates** are Dart's model for true, parallel concurrency. An isolate is an independent worker with its own memory and event loop, running on a separate CPU core if available. They are the key to building responsive, high-performance applications.

- [x] Understanding Isolate Memory Isolation.
- [x] Spawning Isolates: `Isolate.spawn` and `Isolate.run`.
- [x] Two-Way Communication: `ReceivePort` and `SendPort`.
- [x] Handling Errors Across Isolates.

---

### 1. Understanding Isolate Memory Isolation

Think of each isolate as a separate program running in its own "room."
- **No Shared Memory**: Isolates do not share memory. The only way they can interact is by passing messages to each other. This design completely prevents many common concurrency issues like race conditions and the need for locks.
- **Message Passing**: An isolate sends a message to another isolate through a `SendPort`. The receiving isolate listens for messages on a `ReceivePort`. The message data is **copied** (or sometimes transferred, for specific types), not shared.
- **Independent Event Loops**: Each isolate has its own event loop to process events, ensuring that a long-running task in one isolate does not block another.

---

### 2. Spawning Isolates: `Isolate.spawn` and `Isolate.run`

#### `Isolate.spawn`
`Isolate.spawn()` is the fundamental way to create a new isolate. It gives you full control over the isolate's lifecycle and communication channels.

It takes two arguments:
1. The top-level function or static method to execute in the new isolate.
2. The message to send to the new isolate as its initial argument.

```dart
import 'dart:isolate';

// This function will run in the new isolate.
// It must be a top-level function or a static method.
void entryPoint(String message) {
  print('Isolate received: "$message"');
}

void main() {
  print('Main: Spawning a new isolate...');
  
  // Spawn a new isolate that runs the entryPoint function.
  Isolate.spawn(entryPoint, 'Hello from Main!');
  
  print('Main: Continues execution without waiting.');
  
  // Allow time for the isolate to run before the main function exits.
  Future.delayed(Duration(seconds: 1), () => print('Main: Exiting.'));
}
```

#### `Isolate.run` (Dart 2.19+)
`Isolate.run()` is a modern, higher-level API that simplifies the common pattern of "run this function in an isolate and give me the result." It handles the creation of ports, message passing, and isolate shutdown for you.

```dart
import 'dart:isolate';

// A function that performs a heavy computation.
int heavyComputation(int limit) {
  int total = 0;
  for (int i = 1; i <= limit; i++) {
    total += i;
  }
  return total;
}

void main() async {
  print('[Main] Offloading heavy computation to an isolate...');
  
  // Isolate.run() spawns an isolate, runs the function, and returns its result.
  final result = await Isolate.run(() => heavyComputation(1000000000));

  print('[Main] Received result: $result');
}
```

---

### 3. Two-Way Communication with Ports

When you need ongoing communication between isolates (not just a single result), you must manage the ports manually.

1. **Main Isolate**: Creates a `ReceivePort`.
2. **Main Isolate**: Gets the `SendPort` from its `ReceivePort`.
3. **Main Isolate**: Spawns the new isolate, passing its `SendPort` as the initial message.
4. **New Isolate**: Receives the `SendPort` and uses it to send messages back to the main isolate.
5. **Main Isolate**: Listens on its `ReceivePort` for incoming messages.

```dart
import 'dart:isolate';
import 'dart:async';

// The function for the spawned isolate.
void isolateEntryPoint(SendPort mainSendPort) {
  final isolateReceivePort = ReceivePort();
  
  // 1. Send the isolate's own SendPort back to the main isolate.
  mainSendPort.send(isolateReceivePort.sendPort);

  // 2. Listen for messages from the main isolate.
  isolateReceivePort.listen((message) {
    print('[Isolate] Received: "$message"');
    if (message == 'PING') {
      mainSendPort.send('PONG');
    }
  });
}

void main() async {
  final mainReceivePort = ReceivePort();
  
  // Spawn the isolate and pass it our main SendPort.
  await Isolate.spawn(isolateEntryPoint, mainReceivePort.sendPort);

  // The first message from the isolate will be its own SendPort.
  final isolateSendPort = await mainReceivePort.first as SendPort;

  // Listen for further messages from the isolate.
  mainReceivePort.listen((message) {
    print('[Main] Received: "$message"');
  });

  print('[Main] Sending PING to isolate...');
  isolateSendPort.send('PING');
  
  await Future.delayed(Duration(seconds: 1));
  print('[Main] Exiting.');
  mainReceivePort.close(); // Clean up the port.
}
```

---

### 4. Handling Errors Across Isolates

You can send error information between isolates, and `Isolate.run` handles this automatically. For manual management, you can use `Isolate.addErrorListener`.

```dart
import 'dart:isolate';

void failingIsolate(SendPort sendPort) {
  throw 'This isolate has failed!';
}

void main() async {
  final receivePort = ReceivePort();
  
  // Listen for uncaught errors from the spawned isolate.
  final errorPort = ReceivePort();
  errorPort.listen((error) {
    print('[Main] Caught an error from the isolate: $error');
  });

  print('[Main] Spawning an isolate that will fail...');
  await Isolate.spawn(
    failingIsolate,
    receivePort.sendPort,
    onError: errorPort.sendPort,
  );

  await Future.delayed(Duration(seconds: 1));
  print('[Main] Exiting.');
  receivePort.close();
  errorPort.close();
}
```

[⬅ Previous](topic-13-7-io-networking.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-9-developer-tools.md)

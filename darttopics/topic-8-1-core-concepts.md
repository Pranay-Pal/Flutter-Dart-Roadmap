# Topic 8.1: Core Concepts

[⬅ Previous](topic-7-2-modern-oop-features.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-8-2-advanced-asynchrony.md)

    * [ ] Understanding the event loop
    * [ ] Working with `Future` for one-time async results
    * [ ] Using `async` and `await` for clean code

#### Understanding the Event Loop

```dart
import 'dart:async';

void main() {
  print('main start');

  scheduleMicrotask(() => print('microtask #1'));

  Future(() => print('event queue future #1'));
  Future(() => print('event queue future #2'))
      .then((_) => print('event queue future #2 → then callback'));

  Future.microtask(() => print('Future.microtask runs in microtask queue'));

  Future.delayed(Duration.zero, () => print('zero-delay future'));

  print('main end');
}
```

**Expected order:**
```
main start
main end
microtask #1
Future.microtask runs in microtask queue
event queue future #1
event queue future #2
event queue future #2 → then callback
zero-delay future
```

The Dart event loop always finishes the current synchronous work, drains the microtask queue, and only then processes futures from the event queue. Understanding this ordering helps you predict when asynchronous callbacks run.

#### Working with `Future` for One-Time Results

```dart
import 'dart:async';
import 'dart:math';

Future<String> fetchUserData(String userId) {
  return Future.delayed(const Duration(milliseconds: 800), () {
    if (Random().nextBool()) {
      return 'User data for user $userId';
    }
    throw Exception('Network failure for $userId');
  });
}

void main() {
  print('Requesting profile...');

  fetchUserData('101')
      .then((data) => print('Success → $data'))
      .catchError((error) => print('Error → $error'))
      .whenComplete(() => print('Request finished'));

  print('UI stays responsive while the future resolves.');
}
```

Use `then`, `catchError`, and `whenComplete` to react to a future's completion without blocking the main isolate.

#### Using `async` and `await` for Clean Code

```dart
import 'dart:async';
import 'dart:math';

Future<String> fetchUserData(String userId) async {
  print('Fetching user data for $userId...');
  await Future.delayed(const Duration(seconds: 1));

  if (Random().nextBool()) {
    return 'User data for $userId';
  } else {
    throw Exception('Failed to fetch user data');
  }
}

Future<List<String>> fetchMultipleUsers(List<String> userIds) async {
  final results = <String>[];

  for (final id in userIds) {
    try {
      final userData = await fetchUserData(id);
      results.add(userData);
    } catch (e) {
      results.add('Error: $e');
    }
  }

  return results;
}

Future<void> main() async {
  print('Starting async operations...');

  try {
    final user = await fetchUserData('123');
    print('Received: $user');

    final users = await fetchMultipleUsers(['1', '2', '3']);
    for (final entry in users) {
      print(entry);
    }
  } catch (e) {
    print('Top-level error: $e');
  }

  print('Program completed');
}
```

The `async` keyword marks a function that returns a `Future`, while `await` pauses execution until the future completes. This leads to imperative-style asynchronous code that's easier to read and maintain.

### **Module 8: Asynchronous Programming**

[⬅ Previous](topic-7-2-modern-oop-features.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-8-2-advanced-asynchrony.md)

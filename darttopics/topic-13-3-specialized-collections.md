# Topic 13.3: Specialized Collections (`package:collection`)

[⬅ Previous](topic-13-2-math-and-data.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-4-data-conversion.md)

While Dart's core collections (`List`, `Set`, `Map`) are versatile, the officially supported `package:collection` provides a treasure trove of more powerful and specialized collection types, algorithms, and wrappers. It is an essential dependency for almost any non-trivial Dart application.

First, add it to your `pubspec.yaml` and run `dart pub get`:
```yaml
dependencies:
  collection: ^1.18.0 # Check for the latest version on pub.dev
```

- [x] Collection Equality: `ListEquality`, `DeepCollectionEquality`.
- [x] Advanced `Iterable` Manipulation: `firstWhereOrNull`, `groupListsBy`, `sorted`, `sum`.
- [x] Specialized Data Structures: `Queue`, `PriorityQueue`.
- [x] Unmodifiable Views: `UnmodifiableListView`.

---

### 1. Collection Equality

By default, `==` on collections checks for *identity* (are they the same object?), not *equality* (do they have the same contents?). `package:collection` fixes this.

- **`ListEquality`**, **`SetEquality`**, **`MapEquality`**: Compare collections element by element.
- **`DeepCollectionEquality`**: Recursively compares collections that contain other collections, which is essential for complex data structures.

```dart
import 'package:collection/collection.dart';

void main() {
  final list1 = [1, 2, [3, 4]];
  final list2 = [1, 2, [3, 4]];

  print('Default comparison (identity): ${list1 == list2}'); // false

  // A shallow equality check fails because the inner lists are different instances.
  final shallowEquals = ListEquality().equals(list1, list2);
  print('Shallow equality check: $shallowEquals'); // false

  // DeepCollectionEquality recursively compares all nested elements.
  final deepEquals = DeepCollectionEquality().equals(list1, list2);
  print('Deep equality check: $deepEquals'); // true

  // For unordered collections, use DeepCollectionEquality.unordered().
  final list3 = [1, [4, 3], 2];
  final deepUnorderedEquals = DeepCollectionEquality.unordered().equals(list1, list3);
  print('Deep unordered equality check: $deepUnorderedEquals'); // true
}
```

---

### 2. Advanced `Iterable` Manipulation

The package adds dozens of powerful extension methods to `Iterable`, simplifying common data manipulation tasks.

- **`firstWhereOrNull`**: Safely finds the first element matching a condition, returning `null` if none is found (unlike `firstWhere`, which throws an error).
- **`groupListsBy`** / **`groupSetsBy`**: Groups elements into a map based on a key.
- **`sorted`** / **`sortedBy`**: Returns a new sorted list without modifying the original.
- **`sum`**, **`average`**, **`min`**, **`max`**: Convenient numerical operations.
- **`chunked`**: Splits an iterable into chunks of a specific size.

```dart
import 'package:collection/collection.dart';

class User {
  final String name;
  final String role;
  final int score;
  User(this.name, this.role, this.score);
  @override
  String toString() => 'User(name: $name, role: $role, score: $score)';
}

void main() {
  final users = [
    User('Alice', 'admin', 95),
    User('Bob', 'editor', 88),
    User('Charlie', 'editor', 100),
    User('David', 'viewer', 75),
  ];

  // 1. Safely find an element
  final admin = users.firstWhereOrNull((user) => user.role == 'admin');
  print('Admin user: $admin');

  // 2. Group elements into a map
  final usersByRole = users.groupListsBy((user) => user.role);
  print('\nUsers grouped by role:');
  usersByRole.forEach((role, userList) => print('  - $role: $userList'));
  
  // 3. Sort without modifying the original list
  final sortedByScore = users.sortedBy<num>((user) => user.score);
  print('\nUsers sorted by score: $sortedByScore');
  print('Original list remains unchanged: $users');

  // 4. Numerical operations
  final scores = users.map((user) => user.score);
  print('\nTotal score of all users: ${scores.sum}');
  print('Average score: ${scores.average.toStringAsFixed(2)}');

  // 5. Chunking a list
  final numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
  final chunks = numbers.slices(3);
  print('\nNumbers split into chunks of 3: $chunks'); // [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
}
```

---

### 3. Specialized Data Structures

- **`Queue`**: A collection that allows efficient adding and removing from both ends (FIFO - First-In, First-Out). `ListQueue` is a common implementation.
- **`PriorityQueue`**: A queue where elements are ordered based on a comparator. The element with the "highest" priority is always the first one removed.

```dart
import 'dart:collection';
import 'package:collection/collection.dart';

void main() {
  // 1. Using a standard Queue (FIFO)
  final taskQueue = Queue<String>();
  taskQueue.addLast('Task 1');
  taskQueue.addLast('Task 2');
  taskQueue.addFirst('High-priority Task');
  print('Task Queue: $taskQueue');
  
  String nextTask = taskQueue.removeFirst();
  print('Processing "$nextTask", queue is now: $taskQueue');

  // 2. Using a PriorityQueue
  // Elements are ordered by the provided comparator.
  // Here, we create a min-priority queue (smaller numbers have higher priority).
  final priorityQueue = PriorityQueue<int>((a, b) => a.compareTo(b));
  
  priorityQueue.add(50);
  priorityQueue.add(10);
  priorityQueue.add(100);
  priorityQueue.add(30);
  
  print('\nPriority Queue (elements are not stored in order): ${priorityQueue.unorderedElements}');
  print('Removing elements in priority order:');
  while (priorityQueue.isNotEmpty) {
    print('  - Removed: ${priorityQueue.removeFirst()}'); // 10, 30, 50, 100
  }
}
```

---

### 4. Unmodifiable Views

An unmodifiable view is a wrapper around a collection that prevents it from being changed through the view. This is a core principle of defensive programming—it allows you to share internal state without letting external code modify it.

```dart
import 'package:collection/collection.dart';

class Settings {
  final List<String> _supportedLanguages = ['en', 'es', 'fr'];

  // Expose a read-only view of the internal list.
  UnmodifiableListView<String> get supportedLanguages =>
      UnmodifiableListView(_supportedLanguages);
  
  void addLanguage(String lang) {
    _supportedLanguages.add(lang);
    print('Internal list updated: $_supportedLanguages');
  }
}

void main() {
  final settings = Settings();
  final languages = settings.supportedLanguages;

  print('Supported languages: $languages');

  // This is allowed because it modifies the internal list via a class method.
  settings.addLanguage('de');
  print('View reflects the change: $languages');

  // This will throw an UnsupportedError at runtime.
  try {
    languages.add('it');
  } catch (e) {
    print('\nError trying to modify the unmodifiable view directly:');
    print(e);
  }
}
```

[⬅ Previous](topic-13-2-math-and-data.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-4-data-conversion.md)

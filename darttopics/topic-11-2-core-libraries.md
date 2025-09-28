# Topic 11.2: Core Libraries

[⬅ Previous](topic-11-1-libraries-and-packages.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-12-1-the-professional-toolchain.md)

The Dart SDK comes with a powerful set of **core libraries** that provide essential functionalities for everyday programming tasks. You don't need to install any external packages to use them—they are always available. Mastering these libraries is key to becoming a proficient Dart developer.

This topic provides an overview of some of the most important core libraries and what they are used for.

- [x] `dart:core`: Essential classes for numbers, collections, strings, and more.
- [x] `dart:async`: Support for asynchronous programming with `Future` and `Stream`.
- [x] `dart:math`: Mathematical constants and functions.
- [x] `dart:convert`: Encoders and decoders for converting between data formats (e.g., JSON).
- [x] `dart:io`: File, socket, and HTTP support for server-side and command-line apps.

---

### 1. `dart:core`

The `dart:core` library is **automatically imported** into every Dart file, so you never need to import it yourself. It contains the most fundamental classes and functions that you use constantly.

**Key Classes**:
- **`String`**, **`int`**, **`double`**, **`num`**, **`bool`**: The basic data types.
- **`List`**, **`Set`**, **`Map`**: The core collection types.
- **`DateTime`** and **`Duration`**: For working with dates, times, and time spans.
- **`RegExp`**: For creating and using regular expressions.
- **`Uri`**: For parsing and manipulating Uniform Resource Identifiers.
- **`Object`** and **`Type`**: The base of the Dart class hierarchy and for runtime type information.
- **`print()`**: The global function for printing to the console.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=d2d2d2d2d2d2d2d2d2d2d2d2d2d2d2d2)
```dart
// import 'dart:core'; // This import is implicit and not needed.

void main() {
  // Using DateTime and Duration
  final now = DateTime.now();
  final oneWeekFromNow = now.add(const Duration(days: 7));
  print('One week from now will be: ${oneWeekFromNow.toIso8601String()}');

  // Using RegExp for simple email validation
  final emailRegex = RegExp(r'^\S+@\S+\.\S+');
  print('Is "test@example.com" valid? ${emailRegex.hasMatch("test@example.com")}');

  // Using Uri to parse a URL
  final uri = Uri.parse('https://dart.dev/guides/libraries/core');
  print('Scheme: ${uri.scheme}, Host: ${uri.host}, Path: ${uri.path}');
}
```

---

### 2. `dart:async`

This library is crucial for any asynchronous programming in Dart. It provides the `Future` and `Stream` classes, which are the foundation of handling operations that don't complete immediately.

**Key Classes**:
- **`Future`**: Represents a potential value or error that will be available at some time in the future.
- **`Stream`**: Represents a sequence of asynchronous events. You can think of it as an "asynchronous Iterable."
- **`Timer`**: For executing code after a delay or periodically.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=e3e3e3e3e3e3e3e3e3e3e3e3e3e3e3e3)
```dart
import 'dart:async';

Future<String> fetchData() {
  // Simulate a network request that takes 2 seconds.
  return Future.delayed(const Duration(seconds: 2), () => 'Data fetched successfully!');
}

void main() async {
  print('Fetching data...');
  final data = await fetchData();
  print(data); // This will print after a 2-second delay.

  // Example of a Timer
  Timer(const Duration(seconds: 3), () {
    print('This message appears after 3 seconds.');
  });
}
```

---

### 3. `dart:math`

The `dart:math` library provides common mathematical functionalities. It's good practice to import it with a prefix to avoid name clashes with other functions (e.g., your own `max` function).

**Key Features**:
- **Constants**: `pi`, `e`.
- **Functions**: `sin()`, `cos()`, `sqrt()` (square root), `pow()` (power), `max()`, `min()`.
- **`Random`**: A generator for pseudo-random numbers.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=f4f4f4f4f4f4f4f4f4f4f4f4f4f4f4f4)
```dart
import 'dart:math' as math;

void main() {
  // Generate a random number
  final random = math.Random();
  final randomNumber = random.nextInt(100); // A random integer between 0 and 99.
  print('Random number: $randomNumber');

  // Mathematical functions
  print('The square root of 16 is ${math.sqrt(16)}');
  print('2 to the power of 5 is ${math.pow(2, 5)}');
  print('The maximum of 10 and 20 is ${math.max(10, 20)}');
}
```

---

### 4. `dart:convert`

This library is essential for working with different data representations, especially **JSON**, which is the de facto standard for web APIs.

**Key Functions**:
- **`jsonEncode()`**: Converts a Dart object (like a `Map` or `List`) into a JSON string.
- **`jsonDecode()`**: Parses a JSON string into a Dart object.
- **`utf8`**: An encoder/decoder for UTF-8 strings, often used when working with files or network streams.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a)
```dart
import 'dart:convert';

void main() {
  final user = {
    'name': 'Alice',
    'age': 30,
    'email': 'alice@example.com'
  };

  // 1. Encode the Dart object to a JSON string.
  final jsonString = jsonEncode(user);
  print('Encoded JSON: $jsonString');
  // Output: Encoded JSON: {"name":"Alice","age":30,"email":"alice@example.com"}

  // 2. Decode the JSON string back into a Dart object.
  final decodedMap = jsonDecode(jsonString) as Map<String, dynamic>;
  print('Decoded name: ${decodedMap['name']}'); // Output: Decoded name: Alice
}
```

---

### 5. `dart:io`

The `dart:io` library provides APIs to deal with files, directories, processes, sockets, and HTTP clients/servers. **Note**: This library is only available in server-side and command-line environments (like a terminal script or a backend server), **not in the browser**.

**Key Classes**:
- **`File`**: Represents a file on the file system.
- **`Directory`**: Represents a directory on the file system.
- **`HttpClient`**: For making HTTP requests from a non-web environment.
- **`Process`**: For running external processes.
- **`stdin`, `stdout`, `stderr`**: Standard I/O streams.

```dart
// This code must be run in a non-web environment (e.g., via `dart run`).
import 'dart:io';

Future<void> main() async {
  // Create a new file and write to it.
  final file = File('log.txt');
  await file.writeAsString('App started at ${DateTime.now()}\n', mode: FileMode.append);

  // Read the file's content.
  final content = await file.readAsString();
  print('Log file content:\n$content');

  // Get information about the current directory.
  final currentDir = Directory.current;
  print('Current directory: ${currentDir.path}');
}
```

[⬅ Previous](topic-11-1-libraries-and-packages.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-12-1-the-professional-toolchain.md)

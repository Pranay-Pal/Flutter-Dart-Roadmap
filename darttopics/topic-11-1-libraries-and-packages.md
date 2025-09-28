# Topic 11.1: Libraries and Packages

[⬅ Previous](topic-10-2-error-handling.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-11-2-core-libraries.md)

No application is built in isolation. The Dart ecosystem provides a powerful way to organize your code into **libraries** and share reusable code through **packages**. Understanding this system is essential for building scalable and maintainable applications.

This topic will guide you through organizing code into libraries and using the `pub` package manager to leverage the vast ecosystem of open-source packages.

- [x] Organizing code with `library`, `import`, and `export`
- [x] Controlling visibility with underscores (`_`)
- [x] Using advanced import options (`as`, `show`, `hide`)
- [x] Using the `pub` package manager and `pubspec.yaml`

---

### 1. The Library System: Organizing Your Code

A **library** is a unit of code that can be imported and reused. In Dart, **every file is implicitly a library**. You don't need a special `library` keyword for simple cases. You use libraries to control scope, manage privacy, and structure your project.

**Key Directives**:
- **`import`**: Makes the code from another library available. You can import from Dart's core libraries, packages you've downloaded, or your own project files.
- **`export`**: Allows a library to re-export the public API of another library, creating a new, consolidated API. This is useful for creating a single, easy-to-use entry point for a set of related functionalities.

#### Controlling Visibility with Underscores (`_`)

In Dart, privacy is managed at the **library level**, not the class level. Any identifier (class, function, variable) that starts with an underscore (`_`) is **private** to its library and cannot be accessed from outside the file it's defined in.

**Example: A Private Helper Function**

```dart
// In lib/src/string_utils.dart

/// A public function that can be imported.
bool isNullOrEmpty(String? s) {
  return s == null || s.isEmpty;
}

/// A private helper function, only visible within this file.
String _formatMessage(String msg) {
  return '[INFO] $msg';
}

void logMessage(String message) {
  if (!isNullOrEmpty(message)) {
    print(_formatMessage(message)); // We can use _formatMessage here.
  }
}
```
If you tried to import this file and call `_formatMessage()`, you would get a compile-time error.

#### Advanced Import Options

Dart provides keywords to handle potential naming conflicts and to be more selective about what you import.

- **`as` (Prefixing)**: If you import two libraries that have conflicting identifiers, you can use `as` to create a prefix for one or both.
- **`show`**: Lets you import *only* specific names from a library.
- **`hide`**: Lets you import a library but *exclude* specific names.

**Example: Using `as`, `show`, and `hide`**

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=c1c1c1c1c1c1c1c1c1c1c1c1c1c1c1c1)
```dart
// Assume we have two libraries with a 'calculate' function.
// lib/math_a.dart
// int calculate() => 1;

// lib/math_b.dart
// int calculate() => 2;

// We'll simulate this in one file for DartPad.
import 'dart:math' as math; // Prefixing the entire library.
import 'dart:convert' show json, utf8; // Only importing json and utf8.
// import 'dart:io' hide Platform; // Example of hiding.

void main() {
  // Using the prefixed library.
  print(math.pi);

  // Using the shown members.
  final encoded = utf8.encode('Hello, JSON!');
  final decoded = json.decode('{"key": "value"}');
  print(decoded['key']); // Output: value
}
```

---

### 2. The `pub` Package Manager and `pubspec.yaml`

**Packages** are the foundation of code sharing in Dart. A package is a directory containing a `pubspec.yaml` file and one or more libraries. You can find thousands of open-source packages on [pub.dev](https://pub.dev), the official Dart package repository.

The **`pubspec.yaml`** file is the heart of your package. It defines metadata and, most importantly, the package's dependencies.

**Key Sections of `pubspec.yaml`**:
- **`name`**: The name of your package (must be unique on pub.dev if published).
- **`description`**: A brief description of what your package does.
- **`version`**: The current version of your package, following semantic versioning.
- **`environment`**: Specifies the Dart SDK version constraints for your package.
- **`dependencies`**: A list of packages that your code needs to run.
- **`dev_dependencies`**: A list of packages needed for development and testing (e.g., `test`, `lints`), but not for the final application.

**Example `pubspec.yaml`**

```yaml
# pubspec.yaml
name: my_awesome_app
description: An example application showing how to use packages.
version: 1.0.0

environment:
  sdk: '>=3.0.0 <4.0.0'

# Packages required for the application to run.
dependencies:
  http: ^1.1.0 # For making HTTP requests
  intl: ^0.18.0 # For internationalization and formatting

# Packages only needed for development (e.g., testing, code generation).
dev_dependencies:
  test: ^1.24.0
  lints: ^2.1.0
```

#### Managing Dependencies with `dart pub`

You use the `dart pub` command-line tool to manage your dependencies:
- **`dart pub get`**: Downloads all the dependencies listed in your `pubspec.yaml` into a central cache and creates a `pubspec.lock` file to ensure consistent builds.
- **`dart pub add <package_name>`**: Adds a new dependency to your `dependencies` section and runs `pub get`.
- **`dart pub add --dev <package_name>`**: Adds a new dependency to your `dev_dependencies`.
- **`dart pub remove <package_name>`**: Removes a dependency and runs `pub get`.

**Example: Using an External Package (`http`)**

First, add `http` to your `pubspec.yaml` by running `dart pub add http`. Then, you can use it in your code.

```dart
// This example requires the `http` package.
// Run `dart pub add http` in your terminal first.

import 'package:http/http.dart' as http;

Future<void> main() async {
  try {
    final url = Uri.parse('https://jsonplaceholder.typicode.com/todos/1');
    final response = await http.get(url);
    
    if (response.statusCode == 200) {
      print('API Response: ${response.body}');
    } else {
      print('Request failed with status: ${response.statusCode}.');
    }
  } catch (e) {
    print('An error occurred while fetching data: $e');
  }
}
```
// ... existing code ...
This example demonstrates how easily you can pull in powerful, community-maintained code to accelerate your development.

---

### 3. Advanced Library Features: `part` and `deferred as`

For very large libraries or for performance-critical web applications, Dart provides more advanced directives.

#### Splitting a Library with `part` and `part of`

Sometimes, a single library can become too large to manage in one file. The `part` and `part of` directives allow you to split a library's implementation across multiple files.

**How it works:**
1.  The main library file uses the `part` directive to include other files.
2.  The other files use the `part of` directive to declare which library they belong to.
3.  **Important**: All `part` files share the same private scope as the main library file. They are treated as a single, large library.

**Example:**

```dart
// In lib/my_library.dart (The main library file)
library my_library; // Optional, but good practice for clarity.

// Include the other files as parts of this library.
part 'src/helpers.dart';
part 'src/api_service.dart';

// Public API of the library.
void main() {
  // We can call a function from a part file.
  fetchData();
  // We can even access private members from other parts.
  print(_internalHelper()); // Output: This is an internal helper.
}
```

```dart
// In lib/src/api_service.dart
part of my_library; // This file is part of my_library.

void fetchData() {
  print('Fetching data...');
  // Can access private members from other parts.
  print(_internalHelper()); 
}
```

```dart
// In lib/src/helpers.dart
part of my_library; // This file is also part of my_library.

String _internalHelper() {
  return 'This is an internal helper.';
}
```
**Note**: Modern Dart style prefers using `import` and `export` to organize code over `part` and `part of`. The `part` system is now mostly used for generated code.

#### Lazy Loading with `deferred as`

In web applications, it's important to minimize the initial download size for a fast startup. **Deferred loading** allows you to load a library only when it's needed.

You use the `deferred as` keyword when importing a library. This tells Dart to compile the library into a separate unit that can be loaded on demand.

**How it works:**
1.  **Import with `deferred as`**: `import 'package:heavy_library/heavy_library.dart' deferred as heavy;`
2.  **Load the library**: When you need to use the library, you must first call `loadLibrary()` on its prefix. This returns a `Future`.
3.  **Use the library**: Once the `Future` completes, the library is loaded, and you can call its functions.

```dart
// Import a library that we want to load lazily.
import 'package:greetings/greetings.dart' deferred as greetings;

// A placeholder function to simulate a button click or other user action.
Future<void> onActionButtonPressed() async {
  print('User pressed the action button. Loading the greetings library...');
  
  // 1. Load the library. This triggers a network request in a web app.
  await greetings.loadLibrary();
  
  print('Library loaded!');
  
  // 2. Now we can use the functions from the library.
  print(greetings.sayHello());
}

void main() async {
  print('Application started. The greetings library is not yet loaded.');
  await onActionButtonPressed();
}
```
This technique is crucial for optimizing the performance of large web applications by splitting code into smaller, manageable chunks.

[⬅ Previous](topic-10-2-error-handling.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-11-2-core-libraries.md)


[⬅ Previous](topic-10-2-error-handling.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-11-2-core-libraries.md)

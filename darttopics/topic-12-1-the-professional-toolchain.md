# Topic 12.1: The Professional Toolchain

[⬅ Previous](topic-11-2-core-libraries.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-12-2-platform-interoperability.md)

Beyond the language itself, Dart provides a powerful, unified set of tools to help you develop, test, and maintain high-quality applications. Mastering this toolchain is key to becoming a productive and professional Dart developer, as it ensures consistency, improves code quality, and boosts efficiency.

This topic covers the essential command-line tools, the testing framework, and the debugging and profiling suite.

- [x] Using the Dart command-line tools (`run`, `compile`, `format`, `analyze`)
- [x] Writing and running tests with `package:test`
- [x] Debugging and profiling with Dart DevTools

---

### 1. The Dart Command-Line Interface (CLI)

The `dart` command is your entry point to the Dart toolchain. It provides a consistent way to perform essential development tasks right from your terminal.

**Common Commands**:
- **`dart create <directory>`**: Creates a new Dart project with a standard, best-practice directory structure.
- **`dart run <file.dart>`**: Executes a Dart file using the Dart VM.
- **`dart compile <subcommand> <file.dart>`**: Compiles your Dart code. Common subcommands include:
    - `exe`: Creates a self-contained native executable for Windows, macOS, or Linux.
    - `js`: Compiles your code to JavaScript for the web.
- **`dart format .`**: Formats all Dart files in the current directory according to official Dart style guidelines. This is crucial for maintaining readable and consistent code, especially in team projects.
- **`dart analyze`**: Statically analyzes your code for potential errors, style violations, and other issues defined in your `analysis_options.yaml` file. Running this frequently helps catch bugs before you even run your code.
- **`dart pub <subcommand>`**: Manages packages (e.g., `get`, `add`, `remove`).

**Example Professional Workflow**

This workflow is typical for a command-line application.

```bash
# 1. Create a new console application
dart create my_cli_app

# 2. Navigate into the new project
cd my_cli_app

# 3. Add a dependency (e.g., the http package)
dart pub add http

# 4. Run the main application file during development
dart run bin/my_cli_app.dart

# 5. Check for any issues and format the code
dart analyze
dart format .

# 6. Compile the app into a self-contained executable named 'my_app'
dart compile exe bin/my_cli_app.dart -o my_app

# 7. Run the compiled executable from anywhere
# On macOS/Linux: ./my_app
# On Windows: .\my_app.exe
```

---

### 2. Writing and Running Tests with `package:test`

Automated testing is a cornerstone of professional software development. The `dart test` command, combined with the `test` package, provides a rich framework for writing unit, integration, and other types of tests.

First, add the `test` package to your `dev_dependencies` in `pubspec.yaml`:
```bash
dart pub add --dev test
```

**Key Concepts**:
- **`test()`**: Defines a single, isolated test case. It takes a description and a function containing the test logic.
- **`group()`**: Groups related tests together for better organization and readability.
- **`expect()`**: The core assertion function. It compares an `actual` value to an `expected` value using a `Matcher`.
- **Matchers**: Pre-defined constants for common assertions (e.g., `equals()`, `isTrue`, `isNotNull`, `throwsA<T>()`).

**Example: Testing a Simple Calculator**

```dart
// In lib/calculator.dart
class Calculator {
  int add(int a, int b) => a + b;
  int subtract(int a, int b) => a - b;
  int? divide(int a, int b) {
    if (b == 0) return null;
    return a ~/ b; // Integer division
  }
}

// In test/calculator_test.dart
import 'package:my_cli_app/calculator.dart'; // Adjust import as needed
import 'package:test/test.dart';

void main() {
  group('Calculator', () {
    final calculator = Calculator();

    test('add should sum two numbers correctly', () {
      expect(calculator.add(2, 3), 5);
      expect(calculator.add(-1, 1), 0);
    });

    test('subtract should find the difference between two numbers', () {
      expect(calculator.subtract(5, 3), 2);
    });

    test('divide should perform integer division', () {
      expect(calculator.divide(10, 2), 5);
    });

    test('divide should return null when dividing by zero', () {
      expect(calculator.divide(5, 0), isNull);
    });
  });
}
```

To run all tests in your project, simply execute `dart test` in your terminal.

---

### 3. Debugging and Profiling with Dart DevTools

**Dart DevTools** is a suite of performance and debugging tools for Dart and Flutter. It runs in your browser and provides deep insights into your running application, helping you find and fix complex bugs and performance issues.

**Key Features**:
- **Debugger**: Set breakpoints, step through code, and inspect variables and call stacks.
- **Logging View**: View structured log messages from your app using `dart:developer`'s `log()` function.
- **CPU Profiler**: Analyze where your app is spending its time and identify performance bottlenecks.
- **Memory Inspector**: Visualize memory allocation, track down memory leaks, and understand your app's memory usage.

**How to Launch and Use DevTools (for a command-line app)**:

1.  Run your app with the VM service enabled via the `--observe` flag:
    ```bash
    dart run --observe bin/my_cli_app.dart
    ```
2.  The console will print a URL for the Dart VM service. It looks like `http://127.0.0.1:8181/abcdefg=/`. **Copy this entire URL.**
3.  In a **new terminal window**, launch DevTools:
    ```bash
    dart devtools
    ```
4.  DevTools will open in your browser. Paste the VM service URL into the connection dialog to connect to your running app.

**Example: Using `debugger()` and `log()`**

The `dart:developer` library provides tools to interact with DevTools.

```dart
// This code is best experienced when run with DevTools attached.
import 'dart:developer';
import 'dart:io';

void main() {
  print('App starting. Attach DevTools now and press Enter to continue.');
  stdin.readLineSync(); // Pause until user presses Enter.

  // This will pause execution if the debugger in DevTools is attached.
  debugger(message: 'Pausing before the loop. Press resume in DevTools.');

  for (int i = 0; i < 5; i++) {
    // Send a structured log message to the DevTools Logging view.
    log('Processing item $i', name: 'my_app.loop', level: 800);
    print('Processing item $i...');
    sleep(const Duration(milliseconds: 500)); // Simulate work.
  }

  log('Finished processing', name: 'my_app.main');
  print('App finished.');
}
```

[⬅ Previous](topic-11-2-core-libraries.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-12-2-platform-interoperability.md)

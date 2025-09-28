# Topic 12.1: The Professional Toolchain

[⬅ Previous](topic-11-2-core-libraries.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-12-2-platform-interoperability.md)

    * [ ] Using command-line tools (`run`, `compile`, `format`, `analyze`)
    * [ ] Writing and running tests with `dart test`
    * [ ] Debugging and profiling with Dart DevTools

#### Command Line Tools

```bash
# Running Dart code
dart run main.dart

# Formatting code
dart format .

# Analyzing code
dart analyze

# Compiling to executable
dart compile exe main.dart

# Running tests
dart test

# Getting dependencies
dart pub get

# Creating new project
dart create my_project
```

#### Testing Example

```dart
// test/calculator_test.dart
import 'package:test/test.dart';
import 'package:my_project/calculator.dart';

void main() {
  group('Calculator tests', () {
    test('addition works correctly', () {
      expect(Calculator.add(2, 3), equals(5));
      expect(Calculator.add(-1, 1), equals(0));
    });
    
    test('multiplication works correctly', () {
      expect(Calculator.multiply(4, 5), equals(20));
      expect(Calculator.multiply(0, 10), equals(0));
    });
  });
}
```

#### Debugging and Profiling with Dart DevTools

```bash
# Start your app with the VM service enabled
dart run --observe bin/main.dart

# In another terminal, launch DevTools
dart devtools
```

```dart
// bin/main.dart
import 'dart:developer';

void main() {
  debugger(when: () => true); // Execution pauses here when DevTools is attached

  final items = List.generate(5, (index) => index * index);
  log('Computed items', name: 'app.lifecycle', error: null, sequenceNumber: 1, time: DateTime.now(), level: 800);

  print('Items: $items');
}
```

Open `http://127.0.0.1:9100` to inspect memory, CPU usage, and the debugger with breakpoints and logging insights.

### **Module 12: Tooling & Platform Integration**

[⬅ Previous](topic-11-2-core-libraries.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-12-2-platform-interoperability.md)

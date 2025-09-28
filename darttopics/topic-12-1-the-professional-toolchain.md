# Topic 12.1: The Professional Toolchain

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
import '../lib/calculator.dart';

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

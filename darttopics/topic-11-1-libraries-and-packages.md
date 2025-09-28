# Topic 11.1: Libraries and Packages

    * [ ] Organizing code with `library`, `import`, and `export`
    * [ ] Using the `pub` package manager and `pubspec.yaml`

#### Libraries and Package Management

```yaml
# pubspec.yaml example
name: my_dart_project
description: A sample Dart project
version: 1.0.0

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  http: ^1.1.0
  json_annotation: ^4.8.1

dev_dependencies:
  test: ^1.24.0
  build_runner: ^2.4.7
```

```dart
// lib/utils/math_utils.dart
library math_utils;

export 'src/calculator.dart';
export 'src/geometry.dart';

// lib/utils/src/calculator.dart
class Calculator {
  static double add(double a, double b) => a + b;
  static double multiply(double a, double b) => a * b;
}

// main.dart
import 'package:http/http.dart' as http;
import 'lib/utils/math_utils.dart';

void main() async {
  // Using imported package
  var response = await http.get(Uri.parse('https://api.example.com/data'));
  print('Response status: ${response.statusCode}');
  
  // Using local library
  double result = Calculator.add(5.0, 3.0);
  print('5 + 3 = $result');
}
```

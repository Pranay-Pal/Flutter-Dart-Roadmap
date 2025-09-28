# Topic 11.2: Core Libraries

[⬅ Previous](topic-11-1-libraries-and-packages.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-12-1-the-professional-toolchain.md)

    * [ ] Exploring key libraries: `dart:core`, `dart:math`, `dart:convert`, `dart:io`

#### Core Libraries Usage

```dart
import 'dart:convert';
import 'dart:core';
import 'dart:io';
import 'dart:math' as math;

Future<void> main() async {
  // dart:core
  final now = DateTime.now();
  final uri = Uri.parse('https://example.com/api?search=dart&limit=10');
  final buffer = StringBuffer()
    ..writeln('Report generated at $now')
    ..writeln('Authority: ${uri.authority}')
    ..writeln('Query params: ${uri.queryParameters}');
  print(buffer.toString());

    final isValidId = RegExp(r'^[A-Z]{2}-\d{4}$').hasMatch('AB-2024');
  print('ID valid: $isValidId');

  // dart:math
  final rng = math.Random();
  final samples = List.generate(3, (_) => rng.nextDouble());
  print('Random samples: ${samples.map((v) => v.toStringAsFixed(3)).toList()}');
  print('Hypotenuse (3, 4): ${math.sqrt(math.pow(3, 2) + math.pow(4, 2))}');

  // dart:convert
  final user = {'name': 'John', 'age': 30};
  final jsonString = jsonEncode(user);
  final decoded = jsonDecode(jsonString) as Map<String, dynamic>;
  print('JSON: $jsonString');
  print('Decoded name: ${decoded['name']}');

  final csv = 'alice,24\nbob,31';
  final bytes = utf8.encode(csv);
  print('CSV bytes: $bytes');
  print('CSV decoded: ${utf8.decode(bytes)}');

  // dart:io
  final file = File('example.txt');
  await file.writeAsString('Hello, Dart!');
  final content = await file.readAsString();
  print('File content: $content');
  await file.delete();

  final directory = Directory.current;
  print('Working directory: ${directory.path}');
}
```

### **Module 12: Tooling & Platform Integration**

[⬅ Previous](topic-11-1-libraries-and-packages.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-12-1-the-professional-toolchain.md)

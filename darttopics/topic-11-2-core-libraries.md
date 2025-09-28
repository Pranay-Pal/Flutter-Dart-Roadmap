# Topic 11.2: Core Libraries

    * [ ] Exploring key libraries: `dart:core`, `dart:math`, `dart:convert`, `dart:io`

#### Core Libraries Usage

```dart
import 'dart:math' as math;
import 'dart:convert';
import 'dart:io';

void main() async {
  // dart:math
  print('Random number: ${math.Random().nextInt(100)}');
  print('Sin(45°): ${math.sin(math.pi / 4)}');
  print('Max of 5 and 10: ${math.max(5, 10)}');
  
  // dart:convert
  Map<String, dynamic> user = {'name': 'John', 'age': 30};
  String jsonString = jsonEncode(user);
  Map<String, dynamic> decoded = jsonDecode(jsonString);
  print('JSON: $jsonString');
  print('Decoded: $decoded');
  
  // dart:io (example - file operations)
  File file = File('example.txt');
  await file.writeAsString('Hello, Dart!');
  String content = await file.readAsString();
  print('File content: $content');
  await file.delete();
}
```

### **Module 12: Tooling & Platform Integration**

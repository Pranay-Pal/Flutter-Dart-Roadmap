# Topic 13.4: Data Encoding and Decoding (`dart:convert`)

[⬅ Previous](topic-13-3-specialized-collections.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-5-advanced-async.md)

Applications constantly need to convert data between different formats, whether it's for sending over a network, storing in a database, or interacting with a web service. The `dart:convert` library is Dart's primary tool for this, providing codecs for standard formats like JSON and UTF-8, and a framework for creating custom data transformations.

- [x] Mastering JSON: `jsonEncode`, `jsonDecode`, and `JsonCodec`.
- [x] Working with Character Sets: `utf8`, `latin1`, and `base64`.
- [x] Streaming Data Conversion: `LineSplitter` and codec transformers.
- [x] Building Custom Converters with the `Converter` class.

---

### 1. Mastering JSON

JSON (JavaScript Object Notation) is the de facto standard for data exchange on the web. `dart:convert` makes working with it seamless.

- **`jsonEncode()`**: Converts a Dart object (a `Map` or `List`) into a JSON string.
- **`jsonDecode()`**: Parses a JSON string into a Dart object (`Map<String, dynamic>` or `List<dynamic>`).

```dart
import 'dart:convert';

void main() {
  // A typical Dart object (a Map) to be converted to JSON.
  final user = {
    'name': 'Alice',
    'email': 'alice@example.com',
    'id': 123,
    'isActive': true,
    'roles': ['admin', 'editor'],
    'address': null
  };

  // 1. Encode the Dart object into a JSON string.
  // Use an `Indent` to make the output human-readable (pretty-printing).
  final prettyJsonEncoder = JsonEncoder.withIndent('  ');
  String jsonString = prettyJsonEncoder.convert(user);
  print('Encoded JSON string (pretty):\n$jsonString');

  // 2. Decode the JSON string back into a Dart object.
  // The result is a Map<String, dynamic> by default.
  Map<String, dynamic> decodedUser = jsonDecode(jsonString);
  print('\nDecoded Dart Map:\n$decodedUser');
  print('User name: ${decodedUser['name']}'); // Accessing a value
}
```

#### Handling Custom Objects with `toJson()`

`jsonEncode` can automatically call a `toJson()` method on an object if it exists. This is the standard pattern for making your classes JSON-serializable.

```dart
import 'dart:convert';

class User {
  final String name;
  final String email;
  User(this.name, this.email);

  // This method is automatically called by jsonEncode.
  Map<String, dynamic> toJson() => {
    'name': name,
    'email': email,
  };
}

void main() {
  final users = [
    User('Bob', 'bob@example.com'),
    User('Charlie', 'charlie@example.com'),
  ];

  // jsonEncode will call toJson() for each User object in the list.
  String jsonString = jsonEncode(users);
  print('Encoded list of User objects: $jsonString');
  // Output: [{"name":"Bob","email":"bob@example.com"},{"name":"Charlie","email":"charlie@example.com"}]
}
```

---

### 2. Working with Character Sets and Base64

The `dart:convert` library provides codecs for converting between strings and their binary (byte) representations.

- **`utf8`**: The standard for web text, supporting all Unicode characters.
- **`latin1`**: A common 8-bit character set for Western European languages.
- **`base64`**: An encoding that represents binary data in a pure ASCII string format, useful for embedding binary data in text-based formats like JSON or XML.

```dart
import 'dart:convert';
import 'dart:typed_data';

void main() {
  String message = 'Hello, Dart! 🚀';

  // 1. Encode a String into bytes using UTF-8.
  Uint8List utf8Bytes = utf8.encode(message);
  print('Original message: "$message"');
  print('UTF-8 encoded bytes: $utf8Bytes');

  // 2. Decode bytes back into a String.
  String decodedMessage = utf8.decode(utf8Bytes);
  print('Decoded message: "$decodedMessage"');
  print('Are they the same? ${message == decodedMessage}'); // true

  // 3. Encode binary data into a Base64 string.
  String base64String = base64.encode(utf8Bytes);
  print('\nBase64 encoded string: $base64String');

  // 4. Decode a Base64 string back into bytes.
  Uint8List decodedBytes = base64.decode(base64String);
  print('Base64 decoded bytes: $decodedBytes');
  print('Final decoded message: ${utf8.decode(decodedBytes)}');
}
```

---

### 3. Streaming Data Conversion

When working with large data sources (like big files or network responses), it's inefficient to load everything into memory at once. `dart:convert` provides `StreamTransformer`s that can be "piped" together to process data in chunks.

- **`LineSplitter`**: A transformer that splits a stream of string data into individual lines.

```dart
import 'dart:convert';
import 'dart:async';

void main() async {
  // Simulate a stream of data chunks from a network or file.
  final dataStream = Stream.fromIterable([
    '{"name": "A"}\n{"name":',
    '"B"}\n{"name": "C"}\n',
  ]);

  print('Processing stream of JSON objects...');
  
  await for (final jsonLine in dataStream.transform(utf8.decoder).transform(LineSplitter())) {
    if (jsonLine.isNotEmpty) {
      final Map<String, dynamic> data = jsonDecode(jsonLine);
      print('  - Received object: ${data['name']}');
    }
  }
}
```
This pipeline efficiently decodes UTF-8 bytes, splits them into lines, and parses each line as a separate JSON object, all without holding the entire dataset in memory.

---

### 4. Building Custom Converters

The `Converter` abstract class is the foundation of `dart:convert`. You can extend it to create your own reusable data transformations.

Let's create a simple converter that transforms a CSV (Comma-Separated Values) string into a `Map`.

```dart
import 'dart:convert';

/// A custom converter that transforms a CSV string "key,value" into a Map.
class CsvToMapConverter extends Converter<String, Map<String, String>> {
  const CsvToMapConverter();

  @override
  Map<String, String> convert(String input) {
    final parts = input.split(',');
    if (parts.length != 2) {
      throw FormatException('Invalid CSV format. Expected "key,value".');
    }
    return {parts[0]: parts[1]};
  }
}

void main() {
  final converter = const CsvToMapConverter();
  
  final csvString = 'name,Alice';
  final map = converter.convert(csvString);
  
  print('Converted CSV to Map: $map'); // {name: Alice}

  // You can also chain converters. For example, from a stream of bytes:
  // stream.transform(utf8.decoder)
  //       .transform(LineSplitter())
  //       .transform(converter)
  //       .listen((map) => print(map));
}
```

[⬅ Previous](topic-13-3-specialized-collections.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-5-advanced-async.md)

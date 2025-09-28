# Topic 13.1: Core Essentials (`dart:core`)

[⬅ Previous](topic-12-2-platform-interoperability.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-2-math-and-data.md)

The `dart:core` library is the bedrock of the Dart language, automatically imported into every program. It provides the fundamental classes, functions, and types that underpin all Dart code. While you use parts of it implicitly every day (like `String`, `int`, `List`, and `print()`), this topic explores some of its most powerful and essential utilities.

- [x] Working with Time: `DateTime` and `Duration`.
- [x] Parsing and Building URIs: The `Uri` class.
- [x] Advanced Pattern Matching with `RegExp`.
- [x] Measuring Code Performance with `Stopwatch`.
- [x] Understanding `Object`, `Error`, and `Exception`.

---

### 1. Working with Time: `DateTime` and `Duration`

Handling time is a common requirement in applications, from scheduling tasks to displaying timestamps.

- **`DateTime`**: Represents a single moment in time, with millisecond precision. It can be timezone-aware (local or UTC).
- **`Duration`**: Represents a span of time, such as 10 minutes, 5 hours, or 30 seconds. It is essential for date and time arithmetic.

```dart
/// Demonstrates how to work with DateTime and Duration.
void main() {
  // 1. Get the current date and time in the local timezone.
  final now = DateTime.now();
  print('Current time (local): $now');

  // 2. Create a specific date in UTC.
  // Best practice for server-side or consistent time representation.
  final moonLanding = DateTime.utc(1969, 7, 20, 20, 18, 04);
  print('Moon landing (UTC): $moonLanding');

  // 3. Parse a date from an ISO 8601 string (a common data interchange format).
  final specificDate = DateTime.parse('2024-01-01T14:30:00Z'); // 'Z' denotes UTC
  print('Parsed date: $specificDate');
  print('Parsed date (local): ${specificDate.toLocal()}');

  // 4. Create a Duration representing a span of time.
  final tenMinutes = Duration(minutes: 10);
  final oneHourThirtyMinutes = Duration(hours: 1, minutes: 30);
  print('A duration of ten minutes: $tenMinutes');
  print('A complex duration: $oneHourThirtyMinutes');

  // 5. Perform date arithmetic to find a future or past moment.
  final tenMinutesFromNow = now.add(tenMinutes);
  print('Ten minutes from now: $tenMinutesFromNow');

  // 6. Calculate the difference between two DateTime objects.
  final difference = tenMinutesFromNow.difference(now);
  print('Difference in seconds: ${difference.inSeconds}'); // Should be 600
  print('Is the difference exactly 10 minutes? ${difference == tenMinutes}'); // true
}
```

---

### 2. Parsing and Building URIs: The `Uri` Class

A Uniform Resource Identifier (URI) is a standard way to identify resources. The `Uri` class provides a safe and reliable way to parse and construct URIs, preventing common security vulnerabilities and formatting errors that arise from manual string manipulation.

```dart
/// Demonstrates parsing and building URIs.
void main() {
  // 1. Parse a full URI string to deconstruct it.
  final uri = Uri.parse('https://dart.dev/guides/libraries/library-tour?q=dart#uri');
  
  print('Scheme: ${uri.scheme}');       // "https"
  print('Host: ${uri.host}');           // "dart.dev"
  print('Path: ${uri.path}');           // "/guides/libraries/library-tour"
  print('Fragment: ${uri.fragment}');   // "uri"
  print('Query: ${uri.query}');         // "q=dart"
  print('Query Parameters: ${uri.queryParameters}'); // {q: dart}

  // 2. Build a URI from its components for safety and correctness.
  // This correctly handles URL encoding for special characters.
  final apiUri = Uri(
    scheme: 'https',
    host: 'api.example.com',
    path: '/users/search',
    queryParameters: {
      'name': 'John Doe', // Contains a space that needs encoding
      'role': 'user/admin' // Contains a slash
    },
  );
  
  print('\nBuilt API URI: $apiUri');
  // Encoded result: https://api.example.com/users/search?name=John+Doe&role=user%2Fadmin
}
```

---

### 3. Advanced Pattern Matching with `RegExp`

Regular expressions (`RegExp`) are a powerful mini-language for finding, validating, and manipulating text based on complex patterns.

```dart
/// Demonstrates the use of regular expressions.
void main() {
  // A regular expression to find words followed by one or more digits.
  // \w+ matches one or more word characters (letters, numbers, underscore).
  // \s+ matches one or more whitespace characters.
  // (\d+) is a capturing group for one or more digits.
  final wordAndNumberRegExp = RegExp(r'(\w+)\s+(\d+)');

  final text = 'I have 3 apples, 15 oranges, and 12 bananas.';

  // 1. Find all matches in the string.
  final allMatches = wordAndNumberRegExp.allMatches(text);
  print('Found ${allMatches.length} matches.');

  for (final match in allMatches) {
    // group(0) is the full match.
    // group(1) is the first capturing group (the word).
    // group(2) is the second capturing group (the number).
    print('  - Full match: "${match.group(0)}", Word: "${match.group(1)}", Number: ${match.group(2)}');
  }

  // 2. Check if a string contains a match.
  final hasMatch = wordAndNumberRegExp.hasMatch('hello world');
  print('\n"hello world" contains a word-number pattern: $hasMatch'); // false

  // 3. Replace all matches using a function to customize the replacement.
  final replacedText = text.replaceAllMapped(wordAndNumberRegExp, (match) {
    final word = match.group(1)!;
    final number = int.parse(match.group(2)!);
    return '$word (${number * 2})'; // Double the number
  });
  print('Replaced text: "$replacedText"'); 
  // "I have apples (6), oranges (30), and bananas (24)."
}
```

---

### 4. Measuring Code Performance with `Stopwatch`

When you need to benchmark or optimize code, `Stopwatch` provides a simple and accurate way to measure execution time. It is far more suitable for this task than using `DateTime`.

```dart
/// Demonstrates how to measure code execution time.
void main() {
  // 1. Create and start a new Stopwatch.
  // The cascade operator (..) allows us to call start() on the new instance.
  final stopwatch = Stopwatch()..start();

  // 2. Run the code you want to measure.
  // Using StringBuffer is much more efficient for building strings in a loop.
  final buffer = StringBuffer();
  for (int i = 0; i < 100000; i++) {
    buffer.write('a');
  }
  final result = buffer.toString(); // Final string

  // 3. Stop the stopwatch to record the elapsed time.
  stopwatch.stop();

  print('Building a string with StringBuffer took:');
  print('  - ${stopwatch.elapsedMilliseconds} milliseconds');
  print('  - ${stopwatch.elapsedMicroseconds} microseconds');
  print('  - Ticks: ${stopwatch.elapsedTicks}'); // Machine-level precision

  // You can reset and reuse the stopwatch for another measurement.
  stopwatch.reset();
  stopwatch.start();
  // ... measure something else ...
  stopwatch.stop();
  print('\nAnother measurement took ${stopwatch.elapsedMicroseconds} microseconds.');
}
```

---

### 5. Understanding `Object`, `Error`, and `Exception`

In Dart, all types inherit from the `Object` class. `Error` and `Exception` are special subclasses used for signaling problems.

- **`Object`**: The base class for all Dart objects except `null`. It defines default methods like `toString()`, `hashCode`, and the `==` operator, which you can override in your own classes for custom behavior.

- **`Exception`**: Intended for expected, catchable errors that a program can reasonably handle. Examples include `FormatException` when parsing invalid data or `IOException`. You should `catch` exceptions.

- **`Error`**: Intended for programmatic, unrecoverable errors that indicate a bug in the code. Examples include `ArgumentError` (invalid method argument) or `StateError` (object is in a wrong state for an operation). You should not typically `catch` errors; instead, you should fix the underlying code bug.

```dart
class Point {
  final int x;
  final int y;

  Point(this.x, this.y);

  // 1. Override `toString()` for a readable representation.
  @override
  String toString() => 'Point(x: $x, y: $y)';

  // 2. Override `==` to define value equality.
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is Point && other.x == x && other.y == y;
  }

  // 3. Override `hashCode` whenever you override `==`.
  @override
  int get hashCode => x.hashCode ^ y.hashCode;
}

void main() {
  // Demonstrating Object overrides
  final p1 = Point(1, 2);
  final p2 = Point(1, 2);
  print(p1); // Uses the overridden toString() -> "Point(x: 1, y: 2)"
  print('p1 == p2: ${p1 == p2}'); // Uses overridden == -> true

  // Demonstrating Exception vs. Error
  try {
    // This is an expected failure, so it throws an Exception.
    throw FormatException('Could not parse the input.');
  } on FormatException catch (e) {
    print('\nCaught an expected exception: $e');
  }

  try {
    // This indicates a programming error and throws an Error.
    throw ArgumentError('The provided value must be positive.');
  } on Error catch (e) {
    // You normally wouldn't catch this. You'd fix the code that passed a bad value.
    print('Caught a programmatic error (for demonstration): $e');
  }
}
```

[⬅ Previous](topic-12-2-platform-interoperability.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-2-math-and-data.md)

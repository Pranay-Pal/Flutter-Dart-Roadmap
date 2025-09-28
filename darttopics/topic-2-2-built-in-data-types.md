# Topic 2.2: Built-in Data Types

[⬅ Previous](topic-2-1-variables-data.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-2-3-operators.md)

Dart is a type-safe language, and it comes with a set of fundamental data types that are used to represent numbers, text, and true/false values. Understanding these core types is essential for building any Dart application.

This topic explores the most common built-in data types you'll encounter in everyday Dart programming.

---

### 1. Numbers (`int`, `double`, and `num`)

Dart provides three main numeric types to handle whole numbers, decimal numbers, and cases where either might be used.

#### `int` - For Whole Numbers

An `int` represents an integer value. It can be positive, negative, or zero.

```dart
void main() {
  // Declaring integer variables
  int score = 100;
  int temperature = -5;
  int altitude = 0;

  print('Player score: $score');
  print('Temperature outside: $temperature°C');

  // Integers have a fixed size, but it's large enough for most use cases.
  // On native platforms, values can be from -2^63 to 2^63 - 1.
  // When compiled to JavaScript, they are standard JavaScript numbers.
}
```

#### `double` - For Decimal Numbers

A `double` represents a 64-bit floating-point number, which means it can hold values with a decimal point.

```dart
void main() {
  // Declaring double variables
  double pi = 3.14159;
  double price = 19.99;
  double scientific = 1.23e5; // Represents 1.23 * 10^5, or 123000.0

  print('The value of pi is approximately $pi.');
  print('The item costs $$price.');
  print('A large number: $scientific');
}
```

#### `num` - For Any Number Type

`num` is a special type that can hold both `int` and `double` values. This is useful when you need to write a function or a variable that can accept either type of number.

```dart
void main() {
  // `num` can be an integer.
  num myNumber = 42;
  print('myNumber is an ${myNumber.runtimeType}'); // Output: myNumber is an int

  // It can also be a double.
  myNumber = 3.14;
  print('myNumber is now a ${myNumber.runtimeType}'); // Output: myNumber is now a double

  // This is useful for functions that work with both types.
  void printAnyNumber(num number) {
    print('The number is $number.');
  }

  printAnyNumber(100);    // Works with int
  printAnyNumber(99.5); // Works with double
}
```

#### Converting Between Number Types and Parsing from Strings

You often need to convert data between types, especially when handling user input, which usually comes as a `String`.

```dart
void main() {
  // 1. String to int
  var one = int.parse('1');
  print('Parsed string "1" to int: $one');

  // 2. String to double
  var onePointOne = double.parse('1.1');
  print('Parsed string "1.1" to double: $onePointOne');

  // 3. int to String
  String oneAsString = 1.toString();
  print('Converted int 1 to string: "$oneAsString"');

  // 4. double to String, with a specific number of decimal places
  String piAsString = 3.14159.toStringAsFixed(2);
  print('Pi as a string with 2 decimal places: "$piAsString"'); // "3.14"
}
```

> **Error Handling:** If you try to parse a string that isn't a valid number (e.g., `int.parse('hello')`), your program will crash. For safer parsing, use `tryParse`, which returns `null` on failure.
>
> ```dart
> var result = int.tryParse('1a'); // Returns null
> print('Result of trying to parse "1a": $result');
> ```

---

### 2. Strings (`String`) and Interpolation

A `String` is a sequence of characters. In Dart, you can create strings using single (`'`) or double (`"`) quotes.

```dart
void main() {
  String greeting = 'Hello, Dart!';
  String message = "It's a beautiful day.";

  print(greeting);
  print(message);
}
```

#### String Interpolation

String interpolation is the recommended way to embed the values of variables or expressions inside a string. Use `$variableName` for simple variables and `${expression}` for more complex expressions.

```dart
void main() {
  String name = 'Maria';
  int items = 5;
  double totalCost = 75.50;

  // Simple interpolation
  String message = 'Hello, $name! You have $items items in your cart.';
  print(message);

  // Interpolation with an expression
  String summary = 'The total cost is \$${totalCost.toStringAsFixed(2)}.';
  print(summary);

  // You can even call methods inside the expression
  print('Your name in uppercase is ${name.toUpperCase()}.');
}
```

#### Multi-Line Strings

To create a string that spans multiple lines, use triple quotes (either `'''` or `"""`).

```dart
void main() {
  String multiLinePoem = '''
A world of code,
In Dart it's told,
Simple and clear,
A story unfolds.
''';
  print(multiLinePoem);
}
```

#### Raw Strings

A "raw" string (prefixed with `r`) treats all characters literally, ignoring special characters like `\n` (newline) or `\t` (tab). This is useful for regular expressions or file paths.

```dart
void main() {
  // A regular string with a newline character
  String withNewline = 'First line\nSecond line';
  print('Regular string:\n$withNewline');

  // A raw string ignores the \n
  String rawString = r'First line\nSecond line';
  print('\nRaw string:\n$rawString');
}
```

---

### 3. Booleans (`bool`)

The `bool` type represents a boolean value: either `true` or `false`. Booleans are essential for controlling the flow of your program (e.g., in `if` statements and loops).

```dart
void main() {
  // Declaring boolean variables
  bool isLoggedIn = true;
  bool isCompleted = false;
  bool hasErrors = false;

  print('User is logged in: $isLoggedIn');

  // Booleans are often the result of a comparison
  int score = 85;
  bool isPassing = score >= 60; // The expression `score >= 60` evaluates to true
  print('Is the score passing? $isPassing');

  // Using booleans in an if statement
  if (isLoggedIn && !hasErrors) {
    print('Welcome back! You can proceed.');
  } else {
    print('Please log in or fix the errors.');
  }
}
```

In Dart, only the boolean value `true` is treated as true. Unlike some other languages, values like `1` or a non-empty string are not automatically treated as true. This strictness helps prevent subtle bugs.

[⬅ Previous](topic-2-1-variables-data.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-2-3-operators.md)

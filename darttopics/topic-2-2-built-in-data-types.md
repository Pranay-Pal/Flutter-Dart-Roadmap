# Topic 2.2: Built-in Data Types

[⬅ Previous](topic-2-1-variables-data.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-2-3-operators.md)

    * [ ] Numbers (`int`, `double`, `num`)
    * [ ] Strings (`String`) and interpolation
    * [ ] Booleans (`bool`)

#### Numbers (`int`, `double`, `num`)

Dart has three main numeric types for handling different kinds of numbers.

**Integers (`int`):**
```dart
void main() {
  int wholeNumber = 42;
  int negative = -17;
  int large = 1000000;
  
  print('Whole number: $wholeNumber');
  print('Negative: $negative');
  print('Large: $large');
  
  // Integer operations
  int a = 10;
  int b = 3;
  print('Addition: ${a + b}');      // 13
  print('Subtraction: ${a - b}');   // 7
  print('Multiplication: ${a * b}'); // 30
  print('Integer division: ${a ~/ b}'); // 3
  print('Remainder: ${a % b}');     // 1
}
```

**Doubles (`double`):**
```dart
void main() {
  double decimal = 3.14;
  double scientific = 1.42e5; // 142000.0
  double small = 0.0001;
  
  print('Decimal: $decimal');
  print('Scientific: $scientific');
  print('Small: $small');
  
  // Double operations
  double x = 10.5;
  double y = 3.2;
  print('Addition: ${x + y}');      // 13.7
  print('Division: ${x / y}');      // 3.28125
  print('Rounded: ${(x / y).round()}'); // 3
}
```

**The `num` type (supertype of int and double):**
```dart
void main() {
  num number1 = 42;      // Can hold int
  num number2 = 3.14;    // Can hold double
  
  print('Number 1: $number1 (${number1.runtimeType})');
  print('Number 2: $number2 (${number2.runtimeType})');
  
  // Useful for functions that accept both int and double
  printNumber(42);
  printNumber(3.14);
}

void printNumber(num value) {
  print('The number is: $value');
  
  if (value is int) {
    print('It\'s an integer');
  } else if (value is double) {
    print('It\'s a double');
  }
}
```

**Number parsing and conversion:**
```dart
void main() {
  // String to number
  String intString = '42';
  String doubleString = '3.14';
  
  int parsedInt = int.parse(intString);
  double parsedDouble = double.parse(doubleString);
  
  print('Parsed int: $parsedInt');
  print('Parsed double: $parsedDouble');
  
  // Number to string
  int number = 42;
  String numberAsString = number.toString();
  print('Number as string: $numberAsString');
  
  // Handle parsing errors
  String invalidNumber = 'abc';
  int? result = int.tryParse(invalidNumber);
  print('Parsed result: $result'); // null
}
```

#### Strings (`String`) and Interpolation

Strings are sequences of characters used to represent text in Dart.

**Basic string creation:**
```dart
void main() {
  String singleQuotes = 'Hello, World!';
  String doubleQuotes = "Hello, Dart!";
  String multiLine = '''
    This is a 
    multi-line 
    string
  ''';
  
  print(singleQuotes);
  print(doubleQuotes);
  print(multiLine);
}
```

**String interpolation:**
```dart
void main() {
  String name = 'Alice';
  int age = 25;
  double height = 5.6;
  
  // Simple interpolation with $
  print('Name: $name');
  print('Age: $age');
  
  // Complex expressions with ${}
  print('Next year I will be ${age + 1}');
  print('Height in cm: ${height * 30.48}');
  print('Uppercase name: ${name.toUpperCase()}');
  
  // Escaping special characters
  print('Price: \$${10.99}');
  print('Quote: "Hello, $name!"');
}
```

**String operations:**
```dart
void main() {
  String text = 'Hello, Dart Programming';
  
  // Length
  print('Length: ${text.length}');
  
  // Case conversion
  print('Uppercase: ${text.toUpperCase()}');
  print('Lowercase: ${text.toLowerCase()}');
  
  // Substring
  print('Substring: ${text.substring(0, 5)}'); // "Hello"
  
  // Contains
  print('Contains "Dart": ${text.contains("Dart")}'); // true
  
  // Replace
  print('Replace: ${text.replaceAll("Dart", "Flutter")}');
  
  // Split
  List<String> words = text.split(' ');
  print('Words: $words');
  
  // Trim whitespace
  String padded = '  Hello  ';
  print('Trimmed: "${padded.trim()}"');
}
```

**Raw strings and escape sequences:**
```dart
void main() {
  // Raw strings (prefix with r)
  String rawString = r'This is a raw string with \n \t special characters';
  print(rawString);
  
  // Regular strings with escape sequences
  String escapedString = 'Line 1\nLine 2\tTabbed';
  print(escapedString);
  
  // Unicode
  String unicode = 'Emoji: \u{1F600}'; // 😀
  print(unicode);
}
```

#### Booleans (`bool`)

Booleans represent true or false values, essential for logical operations and control flow.

**Basic boolean usage:**
```dart
void main() {
  bool isActive = true;
  bool isComplete = false;
  
  print('Is active: $isActive');
  print('Is complete: $isComplete');
  
  // Boolean from expressions
  int score = 85;
  bool isPassing = score >= 60;
  print('Is passing: $isPassing');
  
  bool isHighScore = score > 90;
  print('Is high score: $isHighScore');
}
```

**Logical operations:**
```dart
void main() {
  bool a = true;
  bool b = false;
  
  // AND operator (&&)
  print('$a && $b = ${a && b}'); // false
  
  // OR operator (||)
  print('$a || $b = ${a || b}'); // true
  
  // NOT operator (!)
  print('!$a = ${!a}'); // false
  print('!$b = ${!b}'); // true
}
```

**Boolean in conditions:**
```dart
void main() {
  int temperature = 75;
  bool isSunny = true;
  bool isWeekend = false;
  
  bool goodWeatherForPicnic = temperature > 70 && isSunny;
  bool canGoPicnic = goodWeatherForPicnic && isWeekend;
  
  print('Good weather: $goodWeatherForPicnic');
  print('Can go picnic: $canGoPicnic');
  
  if (canGoPicnic) {
    print('Let\'s go for a picnic!');
  } else {
    print('Maybe next time...');
  }
}
```

[⬅ Previous](topic-2-1-variables-data.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-2-3-operators.md)

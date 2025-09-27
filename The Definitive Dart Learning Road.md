# The Definitive Dart Learning Roadmap

A complete, structured guide to learning the Dart programming language from the ground up, covering all major topics from the official documentation. This roadmap is designed for beginners and those looking for a comprehensive review of the Dart platform.

---

### **Module 1: First Steps with Dart**

* **Topic 1.1: Setup & Tooling**
    * [ ] Installing the Dart SDK
    * [ ] Configuring a code editor (e.g., VS Code with Dart extension)

#### Installing the Dart SDK

Dart is a programming language developed by Google that can be used for web, mobile, desktop, and server applications. To get started, you need to install the Dart SDK.

**Option 1: Using the Dart installer (Recommended for beginners)**
1. Visit [dart.dev/get-dart](https://dart.dev/get-dart)
2. Download the installer for your operating system
3. Run the installer and follow the instructions

**Option 2: Using package managers**

**Windows (using Chocolatey):**
```bash
choco install dart-sdk
```

**macOS (using Homebrew):**
```bash
brew tap dart-lang/dart
brew install dart
```

**Linux (using apt):**
```bash
sudo apt update
sudo apt install apt-transport-https
wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /usr/share/keyrings/dart.gpg
echo 'deb [signed-by=/usr/share/keyrings/dart.gpg arch=amd64] https://storage.googleapis.com/download.dartlang.org/linux/debian stable main' | sudo tee /etc/apt/sources.list.d/dart_stable.list
sudo apt update
sudo apt install dart
```

**Verify Installation:**
After installation, verify Dart is installed correctly:
```bash
dart --version
```
You should see output like: `Dart SDK version: 3.2.0 (stable)`

#### Configuring a Code Editor

**Visual Studio Code (Recommended)**
1. Download and install [VS Code](https://code.visualstudio.com/)
2. Install the "Dart" extension by Dart Code
3. The extension provides:
   - Syntax highlighting
   - Code completion
   - Debugging support
   - Linting and error detection

**Other Editor Options:**
- **IntelliJ IDEA/Android Studio**: Install the Dart plugin
- **Vim/Neovim**: Use the coc-dart plugin
- **Emacs**: Use dart-mode

* **Topic 1.2: Your First Program**
    * [ ] The `main()` function: Dart's entry point
    * [ ] Writing to the console with `print()`
    * [ ] Understanding code comments (`//`, `/*...*/`, `///`)

#### The `main()` Function: Dart's Entry Point

Every Dart program must have a `main()` function, which is the entry point where program execution begins.

**Basic structure:**
```dart
void main() {
  // Your code goes here
}
```

**Alternative forms:**
```dart
// With command-line arguments
void main(List<String> arguments) {
  print('Arguments: $arguments');
}

// Async main function (for asynchronous operations)
Future<void> main() async {
  // Async code here
}
```

#### Writing to the Console with `print()`

The `print()` function outputs text to the console. It's your primary tool for displaying information and debugging.

**Basic usage:**
```dart
void main() {
  print('Hello, World!');
  print('Welcome to Dart programming!');
}
```

**Printing different data types:**
```dart
void main() {
  print(42);           // Numbers
  print(3.14);         // Decimals
  print(true);         // Booleans
  print('Hello');      // Strings
  print([1, 2, 3]);    // Lists
}
```

**String interpolation in print:**
```dart
void main() {
  String name = 'Alice';
  int age = 25;
  print('My name is $name and I am $age years old.');
  print('Next year I will be ${age + 1} years old.');
}
```

#### Understanding Code Comments

Comments are essential for documenting your code and making it readable for yourself and others.

**Single-line comments (`//`):**
```dart
void main() {
  // This is a single-line comment
  print('Hello, World!'); // Comment at the end of a line
}
```

**Multi-line comments (`/* */`):**
```dart
void main() {
  /*
   * This is a multi-line comment
   * It can span multiple lines
   * Useful for longer explanations
   */
  print('Hello, World!');
}
```

**Documentation comments (`///`):**
```dart
/// This function greets a person by name.
/// 
/// The [name] parameter specifies who to greet.
/// Returns a greeting message as a String.
void greetPerson(String name) {
  print('Hello, $name!');
}

void main() {
  greetPerson('Alice');
}
```

**Your First Complete Program:**
```dart
/// A simple Dart program that demonstrates basic concepts
void main() {
  // Print a welcome message
  print('=== My First Dart Program ===');
  
  // Declare and use variables
  String programmer = 'Alice';
  int experience = 2;
  
  /*
   * Display information about the programmer
   * using string interpolation
   */
  print('Programmer: $programmer');
  print('Years of experience: $experience');
  print('Future experience: ${experience + 1} years');
  
  // End the program
  print('Program completed successfully!');
}
```

**Expected Output:**
```
=== My First Dart Program ===
Programmer: Alice
Years of experience: 2
Future experience: 3 years
Program completed successfully!
```

### **Module 2: Language Fundamentals**

* **Topic 2.1: Variables & Data**
    * [ ] Declaring variables: `var`, `final`, `const`
    * [ ] Understanding lazy initialization with `late`

#### Declaring Variables: `var`, `final`, `const`

Variables are containers that store data values. Dart provides several ways to declare variables, each with different properties.

**Using `var` (Mutable variables):**
```dart
void main() {
  var name = 'Alice';        // Type inferred as String
  var age = 25;              // Type inferred as int
  var height = 5.6;          // Type inferred as double
  
  print('Name: $name');
  print('Age: $age');
  print('Height: $height');
  
  // Values can be changed
  name = 'Bob';
  age = 30;
  print('Updated name: $name, age: $age');
}
```

**Using `final` (Immutable variables - runtime constant):**
```dart
void main() {
  final String city = 'New York';     // Explicit type
  final country = 'USA';              // Type inferred
  final currentTime = DateTime.now(); // Set at runtime
  
  print('City: $city');
  print('Country: $country');
  print('Current time: $currentTime');
  
  // This would cause an error:
  // city = 'Boston'; // Error: Can't assign to final variable
}
```

**Using `const` (Immutable variables - compile-time constant):**
```dart
void main() {
  const double pi = 3.14159;        // Mathematical constant
  const String appName = 'MyApp';   // Known at compile time
  const List<int> numbers = [1, 2, 3]; // Compile-time constant collection
  
  print('Pi: $pi');
  print('App name: $appName');
  print('Numbers: $numbers');
  
  // This would cause an error:
  // const currentTime = DateTime.now(); // Error: Not compile-time constant
}
```

**Comparison of var, final, and const:**
```dart
void main() {
  // var - can be reassigned
  var mutableValue = 10;
  mutableValue = 20; // ✓ This works
  
  // final - cannot be reassigned, set at runtime
  final immutableValue = DateTime.now().millisecondsSinceEpoch;
  // immutableValue = 999; // ✗ Error: Cannot reassign
  
  // const - cannot be reassigned, must be known at compile time
  const compileTimeConstant = 42;
  // compileTimeConstant = 99; // ✗ Error: Cannot reassign
  // const runtimeValue = DateTime.now(); // ✗ Error: Not compile-time constant
}
```

#### Understanding Lazy Initialization with `late`

The `late` keyword allows you to declare non-nullable variables that will be initialized later, but before they're used.

**Basic `late` usage:**
```dart
late String description;

void main() {
  // Variable declared but not initialized yet
  initializeDescription();
  print(description); // Safe to use after initialization
}

void initializeDescription() {
  description = 'This is a late-initialized variable';
}
```

**`late` with `final`:**
```dart
late final String computedValue;

void main() {
  // Expensive computation that we want to delay
  print('Before initialization');
  computedValue = performExpensiveComputation();
  print('Computed value: $computedValue');
  
  // This would cause an error:
  // computedValue = 'new value'; // Error: Can't reassign final variable
}

String performExpensiveComputation() {
  print('Performing expensive computation...');
  return 'Result of computation';
}
```

**Lazy initialization pattern:**
```dart
class DatabaseConnection {
  late final String connectionString;
  
  DatabaseConnection() {
    // connectionString will be initialized when first accessed
  }
  
  String get connection {
    connectionString ??= 'database://localhost:5432';
    return connectionString;
  }
}

void main() {
  var db = DatabaseConnection();
  print('Database created');
  print('Connection: ${db.connection}'); // Initialized here
}
```

* **Topic 2.2: Built-in Data Types**
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

* **Topic 2.3: Operators**
    * [ ] Arithmetic, equality, relational, and logical operators
    * [ ] Type test (`is`, `is!`), cascade (`..`), and conditional operators

#### Arithmetic Operators

Arithmetic operators perform mathematical operations on numbers.

```dart
void main() {
  int a = 10;
  int b = 3;
  double x = 10.5;
  double y = 3.2;
  
  // Basic arithmetic
  print('Addition: $a + $b = ${a + b}');           // 13
  print('Subtraction: $a - $b = ${a - b}');        // 7
  print('Multiplication: $a * $b = ${a * b}');     // 30
  print('Division: $x / $y = ${x / y}');           // 3.28125
  print('Integer division: $a ~/ $b = ${a ~/ b}'); // 3
  print('Modulo: $a % $b = ${a % b}');             // 1
  
  // Unary operators
  print('Unary minus: -$a = ${-a}');               // -10
  print('Unary plus: +$a = ${+a}');                // 10
  
  // Increment and decrement
  int counter = 5;
  print('Original counter: $counter');             // 5
  print('Pre-increment: ${++counter}');            // 6
  print('Post-increment: ${counter++}');           // 6
  print('After post-increment: $counter');         // 7
  print('Pre-decrement: ${--counter}');            // 6
  print('Post-decrement: ${counter--}');           // 6
  print('After post-decrement: $counter');         // 5
}
```

#### Equality and Relational Operators

These operators compare values and return boolean results.

```dart
void main() {
  int a = 10;
  int b = 5;
  int c = 10;
  
  // Equality operators
  print('$a == $b: ${a == b}');     // false
  print('$a == $c: ${a == c}');     // true
  print('$a != $b: ${a != b}');     // true
  print('$a != $c: ${a != c}');     // false
  
  // Relational operators
  print('$a > $b: ${a > b}');       // true
  print('$a < $b: ${a < b}');       // false
  print('$a >= $c: ${a >= c}');     // true
  print('$a <= $b: ${a <= b}');     // false
  
  // String comparison
  String name1 = 'Alice';
  String name2 = 'Bob';
  String name3 = 'Alice';
  
  print('$name1 == $name3: ${name1 == name3}'); // true
  print('$name1 == $name2: ${name1 == name2}'); // false
}
```

#### Logical Operators

Logical operators work with boolean values and are used for complex conditions.

```dart
void main() {
  bool isRaining = false;
  bool isCold = true;
  bool hasUmbrella = true;
  
  // AND operator (&&)
  bool stayInside = isRaining && isCold;
  print('Stay inside: $stayInside'); // false
  
  // OR operator (||)
  bool needJacket = isRaining || isCold;
  print('Need jacket: $needJacket'); // true
  
  // NOT operator (!)
  bool goodWeather = !isRaining && !isCold;
  print('Good weather: $goodWeather'); // false
  
  // Complex logical expressions
  bool canGoOut = !isRaining || (isRaining && hasUmbrella);
  print('Can go out: $canGoOut'); // true
}
```

#### Type Test Operators

Type test operators check the type of an object at runtime.

```dart
void main() {
  dynamic value1 = 42;
  dynamic value2 = 'Hello';
  dynamic value3 = [1, 2, 3];
  
  // 'is' operator - checks if object is of specific type
  print('$value1 is int: ${value1 is int}');         // true
  print('$value1 is String: ${value1 is String}');   // false
  print('$value2 is String: ${value2 is String}');   // true
  print('$value3 is List: ${value3 is List}');       // true
  
  // 'is!' operator - checks if object is NOT of specific type
  print('$value1 is! String: ${value1 is! String}'); // true
  print('$value2 is! int: ${value2 is! int}');       // true
  
  // Practical usage
  processValue(42);
  processValue('Hello');
  processValue([1, 2, 3]);
}

void processValue(dynamic value) {
  if (value is int) {
    print('Processing integer: ${value * 2}');
  } else if (value is String) {
    print('Processing string: ${value.toUpperCase()}');
  } else if (value is List) {
    print('Processing list with ${value.length} items');
  } else {
    print('Unknown type: ${value.runtimeType}');
  }
}
```

#### Cascade Operator (`..`)

The cascade operator allows you to perform multiple operations on the same object.

```dart
class Person {
  String? name;
  int? age;
  String? city;
  
  void introduce() {
    print('Hi, I\'m $name, $age years old from $city');
  }
  
  void celebrateBirthday() {
    age = age! + 1;
    print('$name is now $age years old!');
  }
}

void main() {
  // Without cascade operator
  Person person1 = Person();
  person1.name = 'Alice';
  person1.age = 25;
  person1.city = 'New York';
  person1.introduce();
  
  // With cascade operator
  Person person2 = Person()
    ..name = 'Bob'
    ..age = 30
    ..city = 'San Francisco'
    ..introduce()
    ..celebrateBirthday();
  
  // Cascade with lists
  List<int> numbers = []
    ..add(1)
    ..add(2)
    ..add(3)
    ..sort()
    ..forEach(print);
}
```

#### Conditional Operators

Conditional operators provide shorthand ways to handle conditional logic.

```dart
void main() {
  // Ternary operator (condition ? true_value : false_value)
  int score = 85;
  String grade = score >= 90 ? 'A' : score >= 80 ? 'B' : 'C';
  print('Score: $score, Grade: $grade');
  
  // Null-aware operator (??)
  String? name = null;
  String displayName = name ?? 'Anonymous';
  print('Display name: $displayName');
  
  // Null-aware assignment (??=)
  String? title;
  title ??= 'Default Title'; // Assigns only if title is null
  print('Title: $title');
  
  title ??= 'Another Title'; // Doesn't assign because title is not null
  print('Title: $title');
  
  // Null-aware access (?.)
  String? nullableString = null;
  print('Length: ${nullableString?.length}'); // null, no error
  
  nullableString = 'Hello';
  print('Length: ${nullableString?.length}'); // 5
  
  // Conditional access examples
  demonstrateConditionalAccess();
}

void demonstrateConditionalAccess() {
  List<String>? names = ['Alice', 'Bob', 'Charlie'];
  
  // Safe access to list methods
  print('First name: ${names?.first}');
  print('Length: ${names?.length}');
  
  names = null;
  print('First name when null: ${names?.first}');
  print('Length when null: ${names?.length}');
}
```

### **Module 3: Control Flow & Logic**

* **Topic 3.1: Conditional Statements**
    * [ ] `if`, `else`, and `else if`

#### If, Else, and Else If Statements

Conditional statements allow your program to make decisions and execute different code paths based on conditions.

**Basic if statement:**
```dart
void main() {
  int temperature = 75;
  
  if (temperature > 80) {
    print('It\'s hot outside!');
  }
  
  print('Temperature check complete.');
}
```

**If-else statement:**
```dart
void main() {
  int age = 17;
  
  if (age >= 18) {
    print('You are eligible to vote.');
  } else {
    print('You are not eligible to vote yet.');
  }
  
  // Another example
  bool isRaining = true;
  
  if (isRaining) {
    print('Take an umbrella!');
  } else {
    print('Enjoy the sunshine!');
  }
}
```

**If-else if-else chain:**
```dart
void main() {
  int score = 85;
  
  if (score >= 90) {
    print('Grade: A (Excellent!)');
  } else if (score >= 80) {
    print('Grade: B (Good job!)');
  } else if (score >= 70) {
    print('Grade: C (Average)');
  } else if (score >= 60) {
    print('Grade: D (Needs improvement)');
  } else {
    print('Grade: F (Failed)');
  }
  
  // Multiple conditions
  int temperature = 75;
  bool isSunny = true;
  
  if (temperature > 70 && isSunny) {
    print('Perfect weather for a picnic!');
  } else if (temperature > 70 && !isSunny) {
    print('Warm but cloudy day.');
  } else if (temperature <= 70 && isSunny) {
    print('Cool but sunny day.');
  } else {
    print('Cold and cloudy day.');
  }
}
```

**Nested if statements:**
```dart
void main() {
  bool hasLicense = true;
  int age = 20;
  bool hasInsurance = true;
  
  if (hasLicense) {
    if (age >= 18) {
      if (hasInsurance) {
        print('You can drive!');
      } else {
        print('You need insurance to drive.');
      }
    } else {
      print('You must be 18 or older to drive.');
    }
  } else {
    print('You need a valid license to drive.');
  }
}
```

* **Topic 3.2: Loops**
    * [ ] `for` (including `for-in`), `while`, and `do-while` loops
    * [ ] Using `break` and `continue`

#### For Loops

For loops are used when you know how many times you want to repeat a block of code.

**Traditional for loop:**
```dart
void main() {
  // Basic for loop
  print('Counting from 1 to 5:');
  for (int i = 1; i <= 5; i++) {
    print('Count: $i');
  }
  
  // Counting backwards
  print('\nCountdown:');
  for (int i = 5; i >= 1; i--) {
    print('$i...');
  }
  print('Blast off! 🚀');
  
  // Different increments
  print('\nEven numbers from 0 to 10:');
  for (int i = 0; i <= 10; i += 2) {
    print(i);
  }
}
```

**For-in loops (iterating over collections):**
```dart
void main() {
  // Iterating over a list
  List<String> fruits = ['apple', 'banana', 'orange', 'grape'];
  
  print('My favorite fruits:');
  for (String fruit in fruits) {
    print('- ${fruit.toUpperCase()}');
  }
  
  // Iterating over a set
  Set<int> numbers = {1, 2, 3, 4, 5};
  int sum = 0;
  
  for (int number in numbers) {
    sum += number;
  }
  print('Sum of numbers: $sum');
  
  // Iterating over map keys and values
  Map<String, int> ages = {
    'Alice': 25,
    'Bob': 30,
    'Charlie': 22
  };
  
  print('\nAges:');
  for (String name in ages.keys) {
    print('$name is ${ages[name]} years old');
  }
  
  // Alternative way to iterate over map entries
  for (MapEntry<String, int> entry in ages.entries) {
    print('${entry.key}: ${entry.value}');
  }
}
```

**Nested loops:**
```dart
void main() {
  // Multiplication table
  print('Multiplication Table (1-5):');
  for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
      print('$i x $j = ${i * j}');
    }
    print('---');
  }
  
  // Pattern printing
  print('\nPyramid pattern:');
  for (int i = 1; i <= 5; i++) {
    String spaces = ' ' * (5 - i);
    String stars = '*' * (2 * i - 1);
    print('$spaces$stars');
  }
}
```

#### While Loops

While loops continue executing as long as a condition is true.

**Basic while loop:**
```dart
void main() {
  int count = 1;
  
  print('While loop counting:');
  while (count <= 5) {
    print('Count: $count');
    count++; // Important: increment to avoid infinite loop
  }
  
  // User input simulation
  int attempts = 0;
  int maxAttempts = 3;
  bool success = false;
  
  while (attempts < maxAttempts && !success) {
    attempts++;
    print('Attempt $attempts');
    
    // Simulate some condition for success
    if (attempts == 2) {
      success = true;
      print('Success on attempt $attempts!');
    }
  }
  
  if (!success) {
    print('Failed after $maxAttempts attempts');
  }
}
```

#### Do-While Loops

Do-while loops execute the code block at least once before checking the condition.

```dart
void main() {
  // Menu system example
  int choice;
  
  do {
    print('\n=== Menu ===');
    print('1. View Profile');
    print('2. Settings');
    print('3. Exit');
    print('Enter your choice (1-3):');
    
    // Simulating user input
    choice = 2; // In real app, this would be user input
    
    switch (choice) {
      case 1:
        print('Viewing profile...');
        break;
      case 2:
        print('Opening settings...');
        break;
      case 3:
        print('Goodbye!');
        break;
      default:
        print('Invalid choice. Please try again.');
    }
    
    // For demo purposes, exit after one iteration
    choice = 3;
  } while (choice != 3);
  
  // Another example: input validation
  int number;
  do {
    print('Enter a number between 1 and 10:');
    number = 15; // Simulating invalid input first
    
    if (number < 1 || number > 10) {
      print('Invalid input. Please try again.');
      number = 5; // Simulating valid input on retry
    }
  } while (number < 1 || number > 10);
  
  print('Valid number entered: $number');
}
```

#### Using Break and Continue

Break and continue statements control loop execution flow.

**Break statement:**
```dart
void main() {
  print('Finding first even number greater than 10:');
  
  for (int i = 11; i <= 20; i++) {
    print('Checking: $i');
    
    if (i % 2 == 0) {
      print('Found first even number: $i');
      break; // Exit the loop
    }
  }
  
  // Break in nested loops
  print('\nSearching in nested loops:');
  bool found = false;
  
  for (int i = 1; i <= 3 && !found; i++) {
    for (int j = 1; j <= 3; j++) {
      print('Checking position ($i, $j)');
      
      if (i == 2 && j == 2) {
        print('Target found at ($i, $j)');
        found = true;
        break; // Only breaks inner loop
      }
    }
  }
}
```

**Continue statement:**
```dart
void main() {
  print('Printing odd numbers from 1 to 10:');
  
  for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
      continue; // Skip even numbers
    }
    print(i); // Only prints odd numbers
  }
  
  // Skip specific values
  print('\nProcessing list (skipping negative numbers):');
  List<int> numbers = [1, -2, 3, -4, 5, -6, 7];
  
  for (int number in numbers) {
    if (number < 0) {
      print('Skipping negative number: $number');
      continue;
    }
    
    print('Processing: $number');
    print('Square: ${number * number}');
  }
}
```

* **Topic 3.3: Switch and Case**
    * [ ] Traditional `switch` statements for flow control

#### Traditional Switch Statements

Switch statements provide a clean way to handle multiple conditions based on a single value.

**Basic switch statement:**
```dart
void main() {
  int dayOfWeek = 3;
  
  switch (dayOfWeek) {
    case 1:
      print('Monday - Start of the work week');
      break;
    case 2:
      print('Tuesday - Getting into the groove');
      break;
    case 3:
      print('Wednesday - Hump day!');
      break;
    case 4:
      print('Thursday - Almost there');
      break;
    case 5:
      print('Friday - TGIF!');
      break;
    case 6:
      print('Saturday - Weekend time');
      break;
    case 7:
      print('Sunday - Rest day');
      break;
    default:
      print('Invalid day number');
  }
}
```

**Switch with strings:**
```dart
void main() {
  String grade = 'B';
  
  switch (grade.toUpperCase()) {
    case 'A':
      print('Excellent! Keep up the great work!');
      break;
    case 'B':
      print('Good job! You\'re doing well.');
      break;
    case 'C':
      print('Average. There\'s room for improvement.');
      break;
    case 'D':
      print('Below average. Please study harder.');
      break;
    case 'F':
      print('Failed. You need to retake the exam.');
      break;
    default:
      print('Invalid grade entered.');
  }
}
```

**Multiple cases with same action:**
```dart
void main() {
  String month = 'December';
  
  switch (month.toLowerCase()) {
    case 'december':
    case 'january':
    case 'february':
      print('$month is a winter month');
      break;
    case 'march':
    case 'april':
    case 'may':
      print('$month is a spring month');
      break;
    case 'june':
    case 'july':
    case 'august':
      print('$month is a summer month');
      break;
    case 'september':
    case 'october':
    case 'november':
      print('$month is a fall/autumn month');
      break;
    default:
      print('Invalid month name');
  }
}
```

**Switch in functions:**
```dart
String getOperationResult(String operation, double a, double b) {
  switch (operation) {
    case '+':
      return '${a + b}';
    case '-':
      return '${a - b}';
    case '*':
      return '${a * b}';
    case '/':
      if (b != 0) {
        return '${a / b}';
      } else {
        return 'Error: Division by zero';
      }
    default:
      return 'Error: Unknown operation';
  }
}

void main() {
  print('Calculator:');
  print('5 + 3 = ${getOperationResult('+', 5, 3)}');
  print('10 - 4 = ${getOperationResult('-', 10, 4)}');
  print('6 * 7 = ${getOperationResult('*', 6, 7)}');
  print('15 / 3 = ${getOperationResult('/', 15, 3)}');
  print('10 / 0 = ${getOperationResult('/', 10, 0)}');
  print('5 % 2 = ${getOperationResult('%', 5, 2)}');
}
```

**Advanced switch usage:**
```dart
enum Priority { low, medium, high, urgent }

void main() {
  Priority taskPriority = Priority.high;
  
  switch (taskPriority) {
    case Priority.low:
      print('📝 Low priority - Handle when convenient');
      break;
    case Priority.medium:
      print('📋 Medium priority - Handle within a day');
      break;
    case Priority.high:
      print('⚡ High priority - Handle within hours');
      break;
    case Priority.urgent:
      print('🚨 Urgent - Handle immediately!');
      break;
  }
  
  // Switch with complex conditions
  processHttpStatusCode(200);
  processHttpStatusCode(404);
  processHttpStatusCode(500);
}

void processHttpStatusCode(int statusCode) {
  switch (statusCode) {
    case 200:
    case 201:
    case 202:
      print('✅ Success: Request completed successfully');
      break;
    case 400:
    case 401:
    case 403:
    case 404:
      print('❌ Client Error: Status code $statusCode');
      break;
    case 500:
    case 502:
    case 503:
      print('🔥 Server Error: Status code $statusCode');
      break;
    default:
      if (statusCode >= 100 && statusCode < 200) {
        print('ℹ️ Informational: Status code $statusCode');
      } else if (statusCode >= 300 && statusCode < 400) {
        print('↩️ Redirection: Status code $statusCode');
      } else {
        print('❓ Unknown status code: $statusCode');
      }
  }
}
```

### **Module 4: Collections (Grouping Data)**
* **Topic 4.1: Lists**
    * [ ] Creating and manipulating ordered lists of items
* **Topic 4.2: Sets**
    * [ ] Working with unordered collections of unique items
* **Topic 4.3: Maps**
    * [ ] Using key-value pairs to store data
* **Topic 4.4: Collection Tools**
    * [ ] The spread operator (`...`) and collection `if`/`for`

### **Module 5: Functions (Reusable Code)**
* **Topic 5.1: Function Basics**
    * [ ] Defining functions with parameters and return values
* **Topic 5.2: Parameter Types**
    * [ ] Positional, named, optional, and required parameters
* **Topic 5.3: Advanced Function Concepts**
    * [ ] Arrow syntax (`=>`), anonymous functions, and lexical scope
    * [ ] Using `typedef` for function aliases

### **Module 6: Object-Oriented Programming (OOP) - Part 1**
* **Topic 6.1: Classes and Objects**
    * [ ] Creating classes, properties (fields), and methods
* **Topic 6.2: Constructors**
    * [ ] Default, named, constant, and factory constructors
* **Topic 6.3: Inheritance**
    * [ ] Extending classes with `extends`, using `super`, and `@override`

### **Module 7: Object-Oriented Programming (OOP) - Part 2**
* **Topic 7.1: Advanced Blueprints**
    * [ ] Abstract classes and methods
    * [ ] Implicit interfaces with `implements`
    * [ ] Reusing code with `mixins`
* **Topic 7.2: Modern OOP Features**
    * [ ] Enhanced `enums`
    * [ ] Class modifiers (`interface`, `base`, `final`, `sealed`)
    * [ ] Adding functionality with `extension` methods

### **Module 8: Asynchronous Programming**
* **Topic 8.1: Core Concepts**
    * [ ] Understanding the event loop
    * [ ] Working with `Future` for one-time async results
    * [ ] Using `async` and `await` for clean code
* **Topic 8.2: Advanced Asynchrony**
    * [ ] Handling data sequences with `Stream`
    * [ ] True parallelism with `Isolates`
    * [ ] Lazy value generation with `sync*`/`async*` (Generators)

### **Module 9: Null Safety**
* **Topic 9.1: Understanding the System**
    * [ ] The principle of sound null safety
    * [ ] Nullable (`?`) vs. non-nullable types
* **Topic 9.2: Practical Application**
    * [ ] Type promotion and flow analysis
    * [ ] Using the assertion (`!`) and `late` keywords safely

### **Module 10: Modern Dart Features**
* **Topic 10.1: Records & Patterns**
    * [ ] Returning multiple values with Records
    * [ ] Destructuring data with patterns
    * [ ] Advanced logic with pattern matching (`switch`, `if-case`)
* **Topic 10.2: Error Handling**
    * [ ] Managing exceptions with `try`, `catch`, `finally`, and `throw`

### **Module 11: The Dart Ecosystem**
* **Topic 11.1: Libraries and Packages**
    * [ ] Organizing code with `library`, `import`, and `export`
    * [ ] Using the `pub` package manager and `pubspec.yaml`
* **Topic 11.2: Core Libraries**
    * [ ] Exploring key libraries: `dart:core`, `dart:math`, `dart:convert`, `dart:io`

### **Module 12: Tooling & Platform Integration**
* **Topic 12.1: The Professional Toolchain**
    * [ ] Using command-line tools (`run`, `compile`, `format`, `analyze`)
    * [ ] Writing and running tests with `dart test`
    * [ ] Debugging and profiling with Dart DevTools
* **Topic 12.2: Platform Interoperability**
    * [ ] Calling C code with Foreign Function Interface (FFI)
    * [ ] Interacting with Kotlin/Java and Swift/Objective-C code

### **Module 13: Best Practices & Next Steps**
* **Topic 13.1: Writing Quality Code**
    * [ ] Following the "Effective Dart" style and usage guides
* **Topic 13.2: Compilation and Deployment**
    * [ ] Understanding JIT (development) vs. AOT (production) compilation
* **Topic 13.3: Where to Go Next**
    * [ ] Applying Dart skills to build applications with **Flutter**
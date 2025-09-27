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

#### Creating and Manipulating Lists

Lists are ordered collections of items that can contain duplicates. They are one of the most commonly used data structures in Dart.

**Creating lists:**
```dart
void main() {
  // Creating lists with different approaches
  List<String> fruits = ['apple', 'banana', 'orange'];
  List<int> numbers = [1, 2, 3, 4, 5];
  List<double> prices = [10.99, 25.50, 5.75];
  
  // Creating an empty list
  List<String> emptyList = [];
  List<String> emptyList2 = <String>[];
  List<String> emptyList3 = List<String>.empty(growable: true);
  
  // Creating list with initial values
  List<int> filledList = List.filled(5, 0); // [0, 0, 0, 0, 0]
  List<int> generatedList = List.generate(5, (index) => index + 1); // [1, 2, 3, 4, 5]
  
  print('Fruits: $fruits');
  print('Numbers: $numbers');
  print('Prices: $prices');
  print('Filled list: $filledList');
  print('Generated list: $generatedList');
}
```

**Accessing list elements:**
```dart
void main() {
  List<String> colors = ['red', 'green', 'blue', 'yellow', 'purple'];
  
  // Accessing by index (0-based)
  print('First color: ${colors[0]}');
  print('Third color: ${colors[2]}');
  print('Last color: ${colors[colors.length - 1]}');
  
  // Safe access methods
  print('First color (safe): ${colors.first}');
  print('Last color (safe): ${colors.last}');
  
  // Get element at index with default value
  print('Element at index 10: ${colors.elementAtOrNull(10) ?? 'Not found'}');
  
  // List properties
  print('List length: ${colors.length}');
  print('Is empty: ${colors.isEmpty}');
  print('Is not empty: ${colors.isNotEmpty}');
}

// Extension method for safe element access (Dart 3.0+)
extension SafeList<T> on List<T> {
  T? elementAtOrNull(int index) {
    if (index >= 0 && index < length) {
      return this[index];
    }
    return null;
  }
}
```

**Adding and removing elements:**
```dart
void main() {
  List<String> shoppingCart = ['milk', 'bread'];
  
  print('Initial cart: $shoppingCart');
  
  // Adding elements
  shoppingCart.add('eggs');                    // Add single item
  shoppingCart.addAll(['butter', 'cheese']);   // Add multiple items
  shoppingCart.insert(1, 'yogurt');           // Insert at specific position
  shoppingCart.insertAll(0, ['fruits', 'vegetables']); // Insert multiple at position
  
  print('After adding: $shoppingCart');
  
  // Removing elements
  shoppingCart.remove('bread');               // Remove first occurrence
  shoppingCart.removeAt(0);                  // Remove at specific index
  String removed = shoppingCart.removeLast(); // Remove and return last element
  shoppingCart.removeWhere((item) => item.startsWith('v')); // Remove by condition
  
  print('After removing: $shoppingCart');
  print('Removed item: $removed');
  
  // Clear all elements
  List<String> tempList = ['a', 'b', 'c'];
  tempList.clear();
  print('Cleared list: $tempList');
}
```

**List operations and methods:**
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 3, 2, 1];
  
  // Searching
  print('Contains 3: ${numbers.contains(3)}');
  print('Index of 3: ${numbers.indexOf(3)}');
  print('Last index of 3: ${numbers.lastIndexOf(3)}');
  
  // Sublist operations
  List<int> subList = numbers.sublist(1, 4); // [2, 3, 4]
  print('Sublist (1 to 4): $subList');
  
  // Reversing
  List<int> reversed = numbers.reversed.toList();
  print('Reversed: $reversed');
  
  // Sorting
  List<String> names = ['Charlie', 'Alice', 'Bob', 'Diana'];
  names.sort(); // Sorts in place
  print('Sorted names: $names');
  
  List<int> scores = [85, 92, 78, 96, 88];
  scores.sort((a, b) => b.compareTo(a)); // Sort descending
  print('Sorted scores (desc): $scores');
  
  // Joining
  String joined = names.join(', ');
  print('Joined names: $joined');
}
```

**Advanced list operations:**
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  
  // Filtering
  List<int> evenNumbers = numbers.where((n) => n % 2 == 0).toList();
  print('Even numbers: $evenNumbers');
  
  // Mapping (transforming)
  List<int> squared = numbers.map((n) => n * n).toList();
  print('Squared numbers: $squared');
  
  // Reducing
  int sum = numbers.reduce((a, b) => a + b);
  print('Sum: $sum');
  
  // Folding
  int product = numbers.fold(1, (prev, current) => prev * current);
  print('Product: $product');
  
  // Any and every
  bool hasEven = numbers.any((n) => n % 2 == 0);
  bool allPositive = numbers.every((n) => n > 0);
  print('Has even number: $hasEven');
  print('All positive: $allPositive');
  
  // Take and skip
  List<int> firstThree = numbers.take(3).toList();
  List<int> skipFirstTwo = numbers.skip(2).toList();
  print('First three: $firstThree');
  print('Skip first two: $skipFirstTwo');
}
```

* **Topic 4.2: Sets**
    * [ ] Working with unordered collections of unique items

#### Working with Sets

Sets are unordered collections that contain only unique elements. They're perfect when you need to ensure no duplicates.

**Creating sets:**
```dart
void main() {
  // Creating sets with different approaches
  Set<String> colors = {'red', 'green', 'blue'};
  Set<int> numbers = {1, 2, 3, 4, 5};
  
  // Creating empty sets
  Set<String> emptySet = <String>{};
  Set<String> emptySet2 = Set<String>();
  
  // Creating set from list (removes duplicates)
  List<int> duplicateNumbers = [1, 2, 2, 3, 3, 3, 4, 5];
  Set<int> uniqueNumbers = duplicateNumbers.toSet();
  
  print('Colors: $colors');
  print('Numbers: $numbers');
  print('Unique numbers: $uniqueNumbers');
  print('Empty set: $emptySet');
}
```

**Set operations:**
```dart
void main() {
  Set<String> fruits = {'apple', 'banana', 'orange'};
  
  print('Initial fruits: $fruits');
  
  // Adding elements
  fruits.add('grape');
  fruits.addAll(['kiwi', 'mango']);
  print('After adding: $fruits');
  
  // Try adding duplicate (won't be added)
  bool added = fruits.add('apple'); // Returns false
  print('Added apple again: $added');
  print('Fruits after duplicate add: $fruits');
  
  // Removing elements
  fruits.remove('banana');
  print('After removing banana: $fruits');
  
  // Set properties
  print('Number of fruits: ${fruits.length}');
  print('Contains apple: ${fruits.contains('apple')}');
  print('Is empty: ${fruits.isEmpty}');
}
```

**Set mathematical operations:**
```dart
void main() {
  Set<int> setA = {1, 2, 3, 4, 5};
  Set<int> setB = {4, 5, 6, 7, 8};
  
  print('Set A: $setA');
  print('Set B: $setB');
  
  // Union (all elements from both sets)
  Set<int> union = setA.union(setB);
  print('Union: $union');
  
  // Intersection (common elements)
  Set<int> intersection = setA.intersection(setB);
  print('Intersection: $intersection');
  
  // Difference (elements in A but not in B)
  Set<int> difference = setA.difference(setB);
  print('Difference (A - B): $difference');
  
  // Symmetric difference (elements in either set, but not both)
  Set<int> symmetricDifference = setA.union(setB).difference(setA.intersection(setB));
  print('Symmetric difference: $symmetricDifference');
  
  // Check subset/superset relationships
  Set<int> subset = {2, 3};
  print('Is {2, 3} subset of A: ${subset.every((element) => setA.contains(element))}');
}
```

**Practical set examples:**
```dart
void main() {
  // User permissions system
  Set<String> adminPermissions = {'read', 'write', 'delete', 'admin'};
  Set<String> userPermissions = {'read', 'write'};
  Set<String> guestPermissions = {'read'};
  
  print('Admin permissions: $adminPermissions');
  print('User permissions: $userPermissions');
  print('Guest permissions: $guestPermissions');
  
  // Check if user can delete
  bool canDelete = userPermissions.contains('delete');
  print('User can delete: $canDelete');
  
  // Find what additional permissions user needs to become admin
  Set<String> additionalNeeded = adminPermissions.difference(userPermissions);
  print('Additional permissions needed: $additionalNeeded');
  
  // Unique visitors tracking
  Set<String> dailyVisitors = {'user1', 'user2', 'user3'};
  Set<String> weeklyVisitors = {'user1', 'user4', 'user5'};
  
  // All unique visitors this week
  Set<String> allVisitors = dailyVisitors.union(weeklyVisitors);
  print('All unique visitors: $allVisitors');
  
  // Returning visitors
  Set<String> returningVisitors = dailyVisitors.intersection(weeklyVisitors);
  print('Returning visitors: $returningVisitors');
}
```

* **Topic 4.3: Maps**
    * [ ] Using key-value pairs to store data

#### Using Key-Value Pairs with Maps

Maps store data as key-value pairs, where each key is unique and maps to a specific value.

**Creating maps:**
```dart
void main() {
  // Creating maps with different approaches
  Map<String, int> ages = {
    'Alice': 25,
    'Bob': 30,
    'Charlie': 22
  };
  
  Map<String, double> prices = {
    'coffee': 4.50,
    'tea': 3.25,
    'sandwich': 8.99
  };
  
  // Creating empty maps
  Map<String, String> emptyMap = {};
  Map<String, String> emptyMap2 = <String, String>{};
  Map<String, String> emptyMap3 = Map<String, String>();
  
  // Creating map from iterables
  List<String> keys = ['a', 'b', 'c'];
  List<int> values = [1, 2, 3];
  Map<String, int> mapFromLists = Map.fromIterables(keys, values);
  
  print('Ages: $ages');
  print('Prices: $prices');
  print('Map from lists: $mapFromLists');
}
```

**Accessing and modifying map values:**
```dart
void main() {
  Map<String, String> countries = {
    'US': 'United States',
    'UK': 'United Kingdom',
    'CA': 'Canada',
    'AU': 'Australia'
  };
  
  print('Initial countries: $countries');
  
  // Accessing values
  print('US: ${countries['US']}');
  print('Non-existent key: ${countries['XX']}'); // Returns null
  
  // Safe access with default value
  String country = countries['XX'] ?? 'Unknown Country';
  print('Country with default: $country');
  
  // Adding new key-value pairs
  countries['IN'] = 'India';
  countries['JP'] = 'Japan';
  print('After adding: $countries');
  
  // Updating existing values
  countries['US'] = 'United States of America';
  print('After updating: $countries');
  
  // Adding multiple entries
  countries.addAll({
    'FR': 'France',
    'DE': 'Germany'
  });
  print('After adding multiple: $countries');
  
  // Removing entries
  String? removed = countries.remove('AU');
  print('Removed value: $removed');
  print('After removal: $countries');
}
```

**Map operations and methods:**
```dart
void main() {
  Map<String, int> inventory = {
    'apples': 50,
    'bananas': 30,
    'oranges': 25,
    'grapes': 15
  };
  
  print('Inventory: $inventory');
  
  // Map properties
  print('Number of items: ${inventory.length}');
  print('Is empty: ${inventory.isEmpty}');
  print('Keys: ${inventory.keys}');
  print('Values: ${inventory.values}');
  
  // Checking for keys and values
  print('Contains apples: ${inventory.containsKey('apples')}');
  print('Contains quantity 30: ${inventory.containsValue(30)}');
  
  // Iterating over map
  print('\nInventory details:');
  inventory.forEach((fruit, quantity) {
    print('$fruit: $quantity');
  });
  
  // Iterating with entries
  print('\nUsing entries:');
  for (MapEntry<String, int> entry in inventory.entries) {
    print('${entry.key}: ${entry.value}');
  }
  
  // Convert to lists
  List<String> fruits = inventory.keys.toList();
  List<int> quantities = inventory.values.toList();
  print('Fruits list: $fruits');
  print('Quantities list: $quantities');
}
```

**Advanced map operations:**
```dart
void main() {
  Map<String, double> grades = {
    'Alice': 92.5,
    'Bob': 87.0,
    'Charlie': 95.5,
    'Diana': 89.5,
    'Eve': 78.5
  };
  
  // Filtering maps
  Map<String, double> highGrades = Map.fromEntries(
    grades.entries.where((entry) => entry.value >= 90)
  );
  print('High grades (>=90): $highGrades');
  
  // Transforming values
  Map<String, String> letterGrades = grades.map(
    (name, grade) => MapEntry(name, getLetterGrade(grade))
  );
  print('Letter grades: $letterGrades');
  
  // Finding maximum and minimum
  double highestGrade = grades.values.reduce((a, b) => a > b ? a : b);
  double lowestGrade = grades.values.reduce((a, b) => a < b ? a : b);
  
  String topStudent = grades.entries
      .where((entry) => entry.value == highestGrade)
      .first
      .key;
  
  print('Highest grade: $highestGrade (by $topStudent)');
  print('Lowest grade: $lowestGrade');
  
  // Group by condition
  Map<String, List<String>> groupedStudents = {
    'excellent': [],
    'good': [],
    'average': []
  };
  
  grades.forEach((name, grade) {
    if (grade >= 90) {
      groupedStudents['excellent']!.add(name);
    } else if (grade >= 80) {
      groupedStudents['good']!.add(name);
    } else {
      groupedStudents['average']!.add(name);
    }
  });
  
  print('Grouped students: $groupedStudents');
}

String getLetterGrade(double grade) {
  if (grade >= 90) return 'A';
  if (grade >= 80) return 'B';
  if (grade >= 70) return 'C';
  if (grade >= 60) return 'D';
  return 'F';
}
```

**Nested maps and complex data structures:**
```dart
void main() {
  // Student database with nested information
  Map<String, Map<String, dynamic>> students = {
    'student1': {
      'name': 'Alice Johnson',
      'age': 20,
      'grades': [92, 88, 95, 87],
      'address': {
        'street': '123 Main St',
        'city': 'Boston',
        'zipCode': '02101'
      }
    },
    'student2': {
      'name': 'Bob Smith',
      'age': 19,
      'grades': [78, 82, 85, 80],
      'address': {
        'street': '456 Oak Ave',
        'city': 'New York',
        'zipCode': '10001'
      }
    }
  };
  
  // Accessing nested data
  String studentName = students['student1']!['name'];
  List<int> grades = students['student1']!['grades'];
  String city = students['student1']!['address']['city'];
  
  print('Student: $studentName');
  print('Grades: $grades');
  print('City: $city');
  
  // Calculate average grade for each student
  students.forEach((id, studentData) {
    List<int> studentGrades = studentData['grades'];
    double average = studentGrades.reduce((a, b) => a + b) / studentGrades.length;
    print('${studentData['name']} average: ${average.toStringAsFixed(1)}');
  });
}
```

* **Topic 4.4: Collection Tools**
    * [ ] The spread operator (`...`) and collection `if`/`for`

#### The Spread Operator and Collection If/For

Dart provides powerful tools to work with collections more expressively and concisely.

**Spread operator (`...`):**
```dart
void main() {
  // Basic spread operator usage
  List<int> numbers1 = [1, 2, 3];
  List<int> numbers2 = [4, 5, 6];
  List<int> combined = [...numbers1, ...numbers2];
  
  print('Numbers 1: $numbers1');
  print('Numbers 2: $numbers2');
  print('Combined: $combined');
  
  // Adding individual elements with spread
  List<String> fruits = ['apple', 'banana'];
  List<String> moreFruits = ['orange', ...fruits, 'grape', 'kiwi'];
  print('More fruits: $moreFruits');
  
  // Null-aware spread operator (...?)
  List<int>? nullableList = null;
  List<int> safeList = [1, 2, ...?nullableList, 3, 4];
  print('Safe list: $safeList');
  
  // Spread with sets
  Set<String> colors1 = {'red', 'green'};
  Set<String> colors2 = {'blue', 'yellow'};
  Set<String> allColors = {...colors1, ...colors2, 'purple'};
  print('All colors: $allColors');
  
  // Spread with maps
  Map<String, int> map1 = {'a': 1, 'b': 2};
  Map<String, int> map2 = {'c': 3, 'd': 4};
  Map<String, int> combinedMap = {...map1, ...map2, 'e': 5};
  print('Combined map: $combinedMap');
}
```

**Collection if (conditional elements):**
```dart
void main() {
  bool includeBonusItems = true;
  bool isVip = false;
  int userLevel = 5;
  
  // List with conditional elements
  List<String> items = [
    'basic_item',
    'standard_item',
    if (includeBonusItems) 'bonus_item',
    if (isVip) 'vip_item',
    if (userLevel >= 5) 'advanced_item',
    if (userLevel >= 10) 'expert_item',
  ];
  
  print('Items: $items');
  
  // Set with conditional elements
  Set<String> permissions = {
    'read',
    if (userLevel >= 3) 'write',
    if (userLevel >= 5) 'modify',
    if (userLevel >= 8) 'delete',
    if (isVip) 'admin',
  };
  
  print('Permissions: $permissions');
  
  // Map with conditional entries
  Map<String, dynamic> userProfile = {
    'name': 'John Doe',
    'level': userLevel,
    if (isVip) 'membership': 'VIP',
    if (userLevel >= 5) 'badge': 'Advanced User',
    if (includeBonusItems) 'bonusPoints': 100,
  };
  
  print('User profile: $userProfile');
}
```

**Collection for (loops in collections):**
```dart
void main() {
  // Generate list using for loop
  List<int> squares = [
    for (int i = 1; i <= 5; i++) i * i
  ];
  print('Squares: $squares');
  
  // Generate list from another collection
  List<String> names = ['alice', 'bob', 'charlie'];
  List<String> upperNames = [
    for (String name in names) name.toUpperCase()
  ];
  print('Upper names: $upperNames');
  
  // Conditional generation
  List<int> evenNumbers = [
    for (int i = 1; i <= 10; i++)
      if (i % 2 == 0) i
  ];
  print('Even numbers: $evenNumbers');
  
  // Complex generation with multiple conditions
  List<String> products = ['laptop', 'phone', 'tablet', 'watch'];
  List<double> prices = [999.99, 599.99, 399.99, 299.99];
  
  List<String> expensiveProducts = [
    for (int i = 0; i < products.length; i++)
      if (prices[i] > 500) '${products[i]} (\$${prices[i]})'
  ];
  print('Expensive products: $expensiveProducts');
  
  // Generate set with for loop
  Set<String> uniqueFirstLetters = {
    for (String name in names) name[0].toUpperCase()
  };
  print('Unique first letters: $uniqueFirstLetters');
  
  // Generate map with for loop
  Map<String, int> nameLengths = {
    for (String name in names) name: name.length
  };
  print('Name lengths: $nameLengths');
}
```

**Combining spread, if, and for:**
```dart
void main() {
  List<String> basicFeatures = ['login', 'profile'];
  List<String> premiumFeatures = ['analytics', 'export'];
  List<String> adminFeatures = ['user_management', 'system_config'];
  
  bool isPremium = true;
  bool isAdmin = false;
  List<String> extraFeatures = ['notifications', 'themes'];
  
  // Complex collection construction
  List<String> availableFeatures = [
    ...basicFeatures,
    if (isPremium) ...premiumFeatures,
    if (isAdmin) ...adminFeatures,
    ...extraFeatures,
    for (int i = 1; i <= 3; i++) 'feature_$i',
    if (isPremium)
      for (String feature in ['advanced_search', 'priority_support'])
        feature,
  ];
  
  print('Available features: $availableFeatures');
  
  // Building user interface elements
  bool showHeader = true;
  bool showFooter = true;
  List<String> menuItems = ['home', 'about', 'contact'];
  
  List<String> pageElements = [
    if (showHeader) 'header',
    'main_content',
    for (String item in menuItems) 'menu_$item',
    if (showFooter) 'footer',
    for (int i = 1; i <= 2; i++)
      if (isPremium) 'premium_widget_$i',
  ];
  
  print('Page elements: $pageElements');
}
```

**Practical examples combining all collection tools:**
```dart
void main() {
  // E-commerce cart system
  Map<String, double> inventory = {
    'laptop': 999.99,
    'mouse': 29.99,
    'keyboard': 79.99,
    'monitor': 299.99,
    'webcam': 89.99,
  };
  
  List<String> cartItems = ['laptop', 'mouse'];
  bool hasDiscount = true;
  bool isMember = true;
  double discountRate = 0.1;
  double membershipDiscount = 0.05;
  
  // Calculate order summary
  List<Map<String, dynamic>> orderItems = [
    for (String item in cartItems)
      if (inventory.containsKey(item))
        {
          'name': item,
          'price': inventory[item]!,
          'discountedPrice': inventory[item]! * 
            (1 - (hasDiscount ? discountRate : 0) - 
             (isMember ? membershipDiscount : 0))
        }
  ];
  
  print('Order items:');
  for (var item in orderItems) {
    print('${item['name']}: \$${item['price']} -> \$${item['discountedPrice']?.toStringAsFixed(2)}');
  }
  
  // Generate report
  double totalOriginal = orderItems.fold(0, (sum, item) => sum + item['price']);
  double totalDiscounted = orderItems.fold(0, (sum, item) => sum + item['discountedPrice']);
  double savings = totalOriginal - totalDiscounted;
  
  Map<String, dynamic> orderSummary = {
    'items': orderItems.length,
    'originalTotal': totalOriginal,
    'finalTotal': totalDiscounted,
    'savings': savings,
    if (hasDiscount) 'discount': '${(discountRate * 100).toInt()}%',
    if (isMember) 'membershipDiscount': '${(membershipDiscount * 100).toInt()}%',
    ...if (savings > 50) {'specialOffer': 'Free shipping!'},
  };
  
  print('\nOrder Summary: $orderSummary');
}
```

### **Module 5: Functions (Reusable Code)**

* **Topic 5.1: Function Basics**
    * [ ] Defining functions with parameters and return values

#### Defining Functions with Parameters and Return Values

Functions are reusable blocks of code that perform specific tasks. They help organize code and avoid repetition.

**Basic function structure:**
```dart
// Function declaration syntax:
// returnType functionName(parameters) {
//   // function body
//   return value; // if return type is not void
// }

void main() {
  // Calling functions
  greetUser();
  sayHello('Alice');
  
  int result = addNumbers(5, 3);
  print('5 + 3 = $result');
  
  double area = calculateCircleArea(5.0);
  print('Area of circle with radius 5: ${area.toStringAsFixed(2)}');
}

// Function with no parameters and no return value
void greetUser() {
  print('Hello, welcome to Dart programming!');
}

// Function with parameter but no return value
void sayHello(String name) {
  print('Hello, $name!');
}

// Function with parameters and return value
int addNumbers(int a, int b) {
  return a + b;
}

// Function with calculation
double calculateCircleArea(double radius) {
  const double pi = 3.14159;
  return pi * radius * radius;
}
```

**Functions with different return types:**
```dart
void main() {
  // String function
  String message = createWelcomeMessage('Bob', 25);
  print(message);
  
  // Boolean function
  bool isEligible = checkVotingEligibility(20);
  print('Can vote: $isEligible');
  
  // List function
  List<int> numbers = generateNumbers(5);
  print('Generated numbers: $numbers');
  
  // Map function
  Map<String, dynamic> profile = createUserProfile('Charlie', 30, 'Engineer');
  print('User profile: $profile');
}

String createWelcomeMessage(String name, int age) {
  return 'Welcome $name! You are $age years old.';
}

bool checkVotingEligibility(int age) {
  return age >= 18;
}

List<int> generateNumbers(int count) {
  List<int> numbers = [];
  for (int i = 1; i <= count; i++) {
    numbers.add(i);
  }
  return numbers;
}

Map<String, dynamic> createUserProfile(String name, int age, String job) {
  return {
    'name': name,
    'age': age,
    'job': job,
    'createdAt': DateTime.now().toString(),
  };
}
```

**Function documentation and best practices:**
```dart
/// Calculates the Body Mass Index (BMI) for a person.
/// 
/// The [weight] should be in kilograms and [height] in meters.
/// Returns the BMI value as a double.
/// 
/// Example:
/// ```dart
/// double bmi = calculateBMI(70.0, 1.75);
/// print('BMI: ${bmi.toStringAsFixed(1)}'); // BMI: 22.9
/// ```
double calculateBMI(double weight, double height) {
  if (height <= 0) {
    throw ArgumentError('Height must be greater than 0');
  }
  return weight / (height * height);
}

/// Determines BMI category based on BMI value.
/// 
/// Returns a string describing the BMI category:
/// - 'Underweight' for BMI < 18.5
/// - 'Normal' for BMI 18.5-24.9
/// - 'Overweight' for BMI 25-29.9
/// - 'Obese' for BMI >= 30
String getBMICategory(double bmi) {
  if (bmi < 18.5) {
    return 'Underweight';
  } else if (bmi < 25.0) {
    return 'Normal';
  } else if (bmi < 30.0) {
    return 'Overweight';
  } else {
    return 'Obese';
  }
}

void main() {
  try {
    double bmi = calculateBMI(70.0, 1.75);
    String category = getBMICategory(bmi);
    
    print('BMI: ${bmi.toStringAsFixed(1)}');
    print('Category: $category');
  } catch (e) {
    print('Error: $e');
  }
}
```

* **Topic 5.2: Parameter Types**
    * [ ] Positional, named, optional, and required parameters

#### Parameter Types in Dart

Dart provides flexible ways to define function parameters to make your functions more versatile and easier to use.

**Positional parameters:**
```dart
void main() {
  // Required positional parameters - must be provided in order
  String greeting = createGreeting('Hello', 'Alice');
  print(greeting);
  
  // Order matters with positional parameters
  int result1 = subtract(10, 3); // 10 - 3 = 7
  int result2 = subtract(3, 10); // 3 - 10 = -7
  print('10 - 3 = $result1');
  print('3 - 10 = $result2');
  
  // Function with multiple positional parameters
  displayPersonInfo('John', 25, 'Engineer');
}

String createGreeting(String greeting, String name) {
  return '$greeting, $name!';
}

int subtract(int a, int b) {
  return a - b;
}

void displayPersonInfo(String name, int age, String profession) {
  print('Name: $name, Age: $age, Profession: $profession');
}
```

**Optional positional parameters:**
```dart
void main() {
  // Optional positional parameters are enclosed in square brackets
  printUserInfo('Alice'); // Only required parameter
  printUserInfo('Bob', 30); // Required + one optional
  printUserInfo('Charlie', 25, 'Engineer'); // All parameters
  
  // Default values for optional parameters
  String message1 = formatMessage('Hello');
  String message2 = formatMessage('Hello', 'World');
  String message3 = formatMessage('Hello', 'World', '!');
  
  print(message1); // "Hello there"
  print(message2); // "Hello World"
  print(message3); // "Hello World!"
}

void printUserInfo(String name, [int? age, String? profession]) {
  print('Name: $name');
  if (age != null) {
    print('Age: $age');
  }
  if (profession != null) {
    print('Profession: $profession');
  }
  print('---');
}

String formatMessage(String greeting, [String target = 'there', String punctuation = '']) {
  return '$greeting $target$punctuation';
}
```

**Named parameters:**
```dart
void main() {
  // Named parameters make function calls more readable
  createUser(name: 'Alice', email: 'alice@example.com');
  
  // Order doesn't matter with named parameters
  createUser(email: 'bob@example.com', name: 'Bob');
  
  // Optional named parameters
  createUser(name: 'Charlie', email: 'charlie@example.com', age: 25);
  
  // With default values
  displayMessage(text: 'Hello World');
  displayMessage(text: 'Important!', isUrgent: true);
  displayMessage(text: 'Debug info', isUrgent: false, includeTimestamp: true);
}

void createUser({required String name, required String email, int? age, String role = 'user'}) {
  print('Creating user:');
  print('  Name: $name');
  print('  Email: $email');
  if (age != null) {
    print('  Age: $age');
  }
  print('  Role: $role');
  print('---');
}

void displayMessage({
  required String text,
  bool isUrgent = false,
  bool includeTimestamp = false,
}) {
  String message = text;
  
  if (isUrgent) {
    message = '🚨 URGENT: $message';
  }
  
  if (includeTimestamp) {
    message = '${DateTime.now()}: $message';
  }
  
  print(message);
}
```

**Mixing positional and named parameters:**
```dart
void main() {
  // Positional parameters come first, then named parameters
  processOrder('12345', 'laptop', quantity: 2);
  processOrder('12346', 'phone', quantity: 1, priority: 'high');
  
  // With optional positional and named parameters
  analyzeData([1, 2, 3, 4, 5]);
  analyzeData([1, 2, 3, 4, 5], 'mean');
  analyzeData([1, 2, 3, 4, 5], 'median', includeDetails: true);
}

void processOrder(String orderId, String item, {required int quantity, String priority = 'normal'}) {
  print('Processing Order:');
  print('  ID: $orderId');
  print('  Item: $item');
  print('  Quantity: $quantity');
  print('  Priority: $priority');
  print('---');
}

void analyzeData(List<int> data, [String method = 'sum', {bool includeDetails = false}]) {
  print('Analyzing data: $data');
  print('Method: $method');
  
  switch (method) {
    case 'sum':
      int sum = data.reduce((a, b) => a + b);
      print('Sum: $sum');
      break;
    case 'mean':
      double mean = data.reduce((a, b) => a + b) / data.length;
      print('Mean: ${mean.toStringAsFixed(2)}');
      break;
    case 'median':
      List<int> sorted = [...data]..sort();
      double median = sorted.length % 2 == 0
          ? (sorted[sorted.length ~/ 2 - 1] + sorted[sorted.length ~/ 2]) / 2
          : sorted[sorted.length ~/ 2].toDouble();
      print('Median: $median');
      break;
  }
  
  if (includeDetails) {
    print('Data length: ${data.length}');
    print('Min: ${data.reduce((a, b) => a < b ? a : b)}');
    print('Max: ${data.reduce((a, b) => a > b ? a : b)}');
  }
  print('---');
}
```

**Real-world parameter examples:**
```dart
void main() {
  // API function with various parameter types
  String response1 = makeApiCall('/users');
  String response2 = makeApiCall('/users', method: 'POST');
  String response3 = makeApiCall(
    '/users/123',
    method: 'PUT',
    headers: {'Authorization': 'Bearer token123'},
    timeout: 5000,
  );
  
  print('Response 1: $response1');
  print('Response 2: $response2');
  print('Response 3: $response3');
  
  // Configuration function
  startServer();
  startServer(8080);
  startServer(3000, host: '0.0.0.0', enableLogging: true);
}

String makeApiCall(
  String endpoint, {
  String method = 'GET',
  Map<String, String>? headers,
  int timeout = 3000,
  Map<String, dynamic>? body,
}) {
  // Simulate API call
  String result = 'API Call:\n';
  result += '  Endpoint: $endpoint\n';
  result += '  Method: $method\n';
  result += '  Timeout: ${timeout}ms\n';
  
  if (headers != null) {
    result += '  Headers: $headers\n';
  }
  
  if (body != null) {
    result += '  Body: $body\n';
  }
  
  return result;
}

void startServer([int port = 8080, {String host = 'localhost', bool enableLogging = false}]) {
  print('Starting server...');
  print('  Host: $host');
  print('  Port: $port');
  print('  Logging: ${enableLogging ? 'enabled' : 'disabled'}');
  print('---');
}
```

* **Topic 5.3: Advanced Function Concepts**
    * [ ] Arrow syntax (`=>`), anonymous functions, and lexical scope
    * [ ] Using `typedef` for function aliases

#### Arrow Syntax (`=>`)

Arrow functions provide a concise way to write simple functions that contain only one expression.

```dart
void main() {
  // Traditional function vs arrow function
  print('Traditional: ${addTraditional(5, 3)}');
  print('Arrow: ${addArrow(5, 3)}');
  
  // Arrow functions with different operations
  print('Square of 7: ${square(7)}');
  print('Is even 8: ${isEven(8)}');
  print('Is even 7: ${isEven(7)}');
  
  // Arrow functions with string operations
  print('Greeting: ${greet('Alice')}');
  print('Uppercase: ${toUpper('hello world')}');
  
  // Arrow functions with collections
  List<int> numbers = [1, 2, 3, 4, 5];
  List<int> doubled = numbers.map(doubleValue).toList();
  List<int> evens = numbers.where(isEven).toList();
  
  print('Original: $numbers');
  print('Doubled: $doubled');
  print('Evens: $evens');
}

// Traditional function
int addTraditional(int a, int b) {
  return a + b;
}

// Arrow function - equivalent to above
int addArrow(int a, int b) => a + b;

// More arrow function examples
int square(int n) => n * n;
bool isEven(int n) => n % 2 == 0;
String greet(String name) => 'Hello, $name!';
String toUpper(String text) => text.toUpperCase();
int doubleValue(int n) => n * 2;

// Arrow functions with more complex expressions
String formatPrice(double price) => '\$${price.toStringAsFixed(2)}';
bool isValidEmail(String email) => email.contains('@') && email.contains('.');
int getMax(int a, int b) => a > b ? a : b;
```

#### Anonymous Functions (Lambda Functions)

Anonymous functions are functions without a name, often used as arguments to other functions.

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  
  // Anonymous function with traditional syntax
  List<int> squares = numbers.map((int n) {
    return n * n;
  }).toList();
  print('Squares: $squares');
  
  // Anonymous function with arrow syntax
  List<int> cubes = numbers.map((n) => n * n * n).toList();
  print('Cubes: $cubes');
  
  // Filtering with anonymous functions
  List<int> evens = numbers.where((n) => n % 2 == 0).toList();
  List<int> largeNumbers = numbers.where((n) => n > 5).toList();
  
  print('Even numbers: $evens');
  print('Numbers > 5: $largeNumbers');
  
  // forEach with anonymous function
  print('Printing with forEach:');
  numbers.where((n) => n <= 5).forEach((n) {
    print('  Number: $n, Square: ${n * n}');
  });
  
  // Sorting with anonymous functions
  List<String> names = ['Charlie', 'Alice', 'Bob', 'Diana'];
  
  // Sort by length
  List<String> sortedByLength = [...names];
  sortedByLength.sort((a, b) => a.length.compareTo(b.length));
  print('Sorted by length: $sortedByLength');
  
  // Sort by last letter
  List<String> sortedByLastLetter = [...names];
  sortedByLastLetter.sort((a, b) => a[a.length - 1].compareTo(b[b.length - 1]));
  print('Sorted by last letter: $sortedByLastLetter');
}
```

**Anonymous functions in practice:**
```dart
void main() {
  // Using anonymous functions for event handling simulation
  List<String> events = ['click', 'hover', 'focus', 'blur'];
  
  // Map events to handlers
  Map<String, Function> eventHandlers = {
    for (String event in events)
      event: () => print('Handling $event event')
  };
  
  // Execute handlers
  eventHandlers.forEach((event, handler) {
    print('Event: $event');
    handler();
  });
  
  // Complex data processing with anonymous functions
  List<Map<String, dynamic>> products = [
    {'name': 'Laptop', 'price': 999.99, 'category': 'Electronics'},
    {'name': 'Book', 'price': 15.99, 'category': 'Education'},
    {'name': 'Phone', 'price': 599.99, 'category': 'Electronics'},
    {'name': 'Desk', 'price': 299.99, 'category': 'Furniture'},
  ];
  
  // Filter expensive electronics
  List<Map<String, dynamic>> expensiveElectronics = products
      .where((product) => 
          product['category'] == 'Electronics' && product['price'] > 500)
      .toList();
  
  print('Expensive electronics:');
  expensiveElectronics.forEach((product) => 
      print('  ${product['name']}: \$${product['price']}'));
  
  // Calculate total by category
  Map<String, double> categoryTotals = {};
  products.forEach((product) {
    String category = product['category'];
    double price = product['price'];
    categoryTotals[category] = (categoryTotals[category] ?? 0) + price;
  });
  
  print('Category totals: $categoryTotals');
}
```

#### Lexical Scope

Lexical scope means that functions have access to variables in their defining scope.

```dart
// Global variable
String globalMessage = 'I am global';

void main() {
  String mainMessage = 'I am in main';
  
  // Function defined in main scope
  void innerFunction() {
    String innerMessage = 'I am inner';
    print('From inner function:');
    print('  $innerMessage');
    print('  $mainMessage'); // Can access main's variable
    print('  $globalMessage'); // Can access global variable
  }
  
  innerFunction();
  
  // Closures - functions that capture variables from outer scope
  Function makeMultiplier(int multiplier) {
    // This function "closes over" the multiplier parameter
    return (int value) => value * multiplier;
  }
  
  Function multiplyByTwo = makeMultiplier(2);
  Function multiplyByTen = makeMultiplier(10);
  
  print('5 * 2 = ${multiplyByTwo(5)}');
  print('5 * 10 = ${multiplyByTen(5)}');
  
  // Practical closure example - counter
  Function createCounter() {
    int count = 0; // This variable is captured by the closure
    
    return () {
      count++;
      return count;
    };
  }
  
  Function counter1 = createCounter();
  Function counter2 = createCounter();
  
  print('Counter 1: ${counter1()}'); // 1
  print('Counter 1: ${counter1()}'); // 2
  print('Counter 2: ${counter2()}'); // 1 (separate instance)
  print('Counter 1: ${counter1()}'); // 3
}
```

**Advanced closure examples:**
```dart
void main() {
  // Configuration closure
  Function createApiClient(String baseUrl, String apiKey) {
    return (String endpoint) {
      return 'Calling $baseUrl$endpoint with API key: ${apiKey.substring(0, 3)}...';
    };
  }
  
  Function userApi = createApiClient('https://api.example.com', 'key123456');
  print(userApi('/users'));
  print(userApi('/posts'));
  
  // Accumulator closure
  Function createAccumulator(int initialValue) {
    int sum = initialValue;
    
    return (int value) {
      sum += value;
      return sum;
    };
  }
  
  Function accumulator = createAccumulator(100);
  print('Accumulator: ${accumulator(10)}'); // 110
  print('Accumulator: ${accumulator(5)}');  // 115
  print('Accumulator: ${accumulator(-3)}'); // 112
  
  // Memoization closure
  Function memoize(Function fn) {
    Map<String, dynamic> cache = {};
    
    return (dynamic arg) {
      String key = arg.toString();
      if (cache.containsKey(key)) {
        print('Cache hit for $key');
        return cache[key];
      }
      
      dynamic result = fn(arg);
      cache[key] = result;
      print('Computed and cached result for $key');
      return result;
    };
  }
  
  Function expensiveCalculation = (int n) {
    // Simulate expensive operation
    int result = 1;
    for (int i = 1; i <= n; i++) {
      result *= i;
    }
    return result;
  };
  
  Function memoizedFactorial = memoize(expensiveCalculation);
  
  print('Factorial 5: ${memoizedFactorial(5)}');
  print('Factorial 5: ${memoizedFactorial(5)}'); // Should use cache
  print('Factorial 6: ${memoizedFactorial(6)}');
}
```

#### Using `typedef` for Function Aliases

`typedef` allows you to create type aliases for functions, making code more readable and maintainable.

```dart
// Define function type aliases
typedef Calculator = int Function(int a, int b);
typedef StringProcessor = String Function(String input);
typedef BoolValidator = bool Function(String value);
typedef EventHandler = void Function(String eventType, Map<String, dynamic> data);

void main() {
  // Using typedef with different function implementations
  Calculator add = (a, b) => a + b;
  Calculator multiply = (a, b) => a * b;
  Calculator subtract = (a, b) => a - b;
  
  print('Add: ${performCalculation(add, 5, 3)}');
  print('Multiply: ${performCalculation(multiply, 5, 3)}');
  print('Subtract: ${performCalculation(subtract, 5, 3)}');
  
  // String processors
  StringProcessor upperCase = (s) => s.toUpperCase();
  StringProcessor addPrefix = (s) => 'PREFIX: $s';
  StringProcessor reverse = (s) => s.split('').reversed.join('');
  
  String text = 'hello world';
  print('Original: $text');
  print('Upper: ${processString(upperCase, text)}');
  print('Prefixed: ${processString(addPrefix, text)}');
  print('Reversed: ${processString(reverse, text)}');
  
  // Validators
  BoolValidator emailValidator = (email) => email.contains('@') && email.contains('.');
  BoolValidator phoneValidator = (phone) => RegExp(r'^\d{10}$').hasMatch(phone);
  BoolValidator notEmptyValidator = (value) => value.trim().isNotEmpty;
  
  print('Email validation: ${validateInput(emailValidator, 'user@example.com')}');
  print('Phone validation: ${validateInput(phoneValidator, '1234567890')}');
  print('Not empty validation: ${validateInput(notEmptyValidator, 'hello')}');
  
  // Event handling system
  EventHandler logHandler = (type, data) => print('LOG: $type - $data');
  EventHandler analyticsHandler = (type, data) => print('ANALYTICS: Tracking $type');
  
  handleEvent(logHandler, 'user_click', {'button': 'submit', 'page': 'login'});
  handleEvent(analyticsHandler, 'page_view', {'page': 'dashboard'});
}

int performCalculation(Calculator calc, int a, int b) {
  return calc(a, b);
}

String processString(StringProcessor processor, String input) {
  return processor(input);
}

bool validateInput(BoolValidator validator, String input) {
  return validator(input);
}

void handleEvent(EventHandler handler, String eventType, Map<String, dynamic> data) {
  handler(eventType, data);
}
```

**Advanced typedef usage:**
```dart
// Complex typedef examples
typedef AsyncDataProcessor<T> = Future<T> Function(T data);
typedef ListTransformer<T, R> = List<R> Function(List<T> items);
typedef ConditionalAction = void Function() Function(bool condition);

// Generic typedef
typedef Mapper<T, R> = R Function(T input);
typedef Predicate<T> = bool Function(T input);
typedef Consumer<T> = void Function(T input);

void main() {
  // Generic mappers
  Mapper<int, String> intToString = (n) => 'Number: $n';
  Mapper<String, int> stringLength = (s) => s.length;
  Mapper<double, int> doubleToInt = (d) => d.round();
  
  print('Int to string: ${intToString(42)}');
  print('String length: ${stringLength('Hello')}');
  print('Double to int: ${doubleToInt(3.7)}');
  
  // List transformers
  ListTransformer<int, String> numbersToStrings = (numbers) =>
      numbers.map((n) => 'Item $n').toList();
  
  ListTransformer<String, int> stringsToLengths = (strings) =>
      strings.map((s) => s.length).toList();
  
  List<int> numbers = [1, 2, 3, 4, 5];
  List<String> words = ['hello', 'world', 'dart'];
  
  print('Numbers to strings: ${numbersToStrings(numbers)}');
  print('Strings to lengths: ${stringsToLengths(words)}');
  
  // Conditional actions
  ConditionalAction createConditionalGreeting(String name) {
    return (bool shouldGreet) {
      return shouldGreet ? () => print('Hello, $name!') : () => print('...');
    };
  }
  
  ConditionalAction greetAlice = createConditionalGreeting('Alice');
  greetAlice(true)();  // Hello, Alice!
  greetAlice(false)(); // ...
  
  // Practical application: Plugin system
  demonstratePluginSystem();
}

// Plugin system demonstration using typedef
typedef PluginAction = void Function(Map<String, dynamic> context);
typedef PluginFilter = bool Function(String pluginName, Map<String, dynamic> context);

void demonstratePluginSystem() {
  // Define plugins
  Map<String, PluginAction> plugins = {
    'logger': (context) => print('LOG: ${context['message']}'),
    'validator': (context) => print('VALIDATE: ${context['data']}'),
    'formatter': (context) => print('FORMAT: ${context['text']?.toUpperCase()}'),
  };
  
  // Plugin filters
  PluginFilter devOnlyFilter = (name, context) => 
      name.startsWith('dev_') || context['environment'] == 'development';
  
  PluginFilter userRoleFilter = (name, context) =>
      context['userRole'] == 'admin' || !name.contains('admin');
  
  // Execute plugins with filtering
  Map<String, dynamic> context = {
    'message': 'User logged in',
    'data': {'user': 'john@example.com'},
    'text': 'hello world',
    'environment': 'production',
    'userRole': 'user',
  };
  
  executePlugins(plugins, context, [userRoleFilter]);
}

void executePlugins(
  Map<String, PluginAction> plugins,
  Map<String, dynamic> context,
  List<PluginFilter> filters,
) {
  plugins.forEach((name, action) {
    bool shouldExecute = filters.every((filter) => filter(name, context));
    
    if (shouldExecute) {
      print('Executing plugin: $name');
      action(context);
    } else {
      print('Skipping plugin: $name (filtered out)');
    }
  });
}
```

### **Module 6: Object-Oriented Programming (OOP) - Part 1**

* **Topic 6.1: Classes and Objects**
    * [ ] Creating classes, properties (fields), and methods

#### Creating Classes, Properties (Fields), and Methods

Classes are blueprints for creating objects. They encapsulate data (properties) and behavior (methods) into a single unit.

**Basic class structure:**
```dart
class Person {
  // Properties (fields)
  String name;
  int age;
  String email;
  
  // Constructor
  Person(this.name, this.age, this.email);
  
  // Methods
  void introduce() {
    print('Hi, I\'m $name, $age years old. Email: $email');
  }
  
  void celebrateBirthday() {
    age++;
    print('Happy birthday! $name is now $age years old.');
  }
  
  String getInfo() {
    return 'Name: $name, Age: $age, Email: $email';
  }
}

void main() {
  // Creating objects (instances of the class)
  Person person1 = Person('Alice', 25, 'alice@example.com');
  Person person2 = Person('Bob', 30, 'bob@example.com');
  
  // Using object methods
  person1.introduce();
  person2.introduce();
  
  // Accessing and modifying properties
  print('${person1.name} is ${person1.age} years old');
  person1.celebrateBirthday();
  
  // Using getter methods
  print(person1.getInfo());
}
```

**Class with private properties and getters/setters:**
```dart
class BankAccount {
  String _accountNumber; // Private property (starts with _)
  String _holderName;
  double _balance;
  
  BankAccount(this._accountNumber, this._holderName, this._balance);
  
  // Getters
  String get accountNumber => _accountNumber;
  String get holderName => _holderName;
  double get balance => _balance;
  
  // Setter with validation
  set holderName(String name) {
    if (name.trim().isEmpty) {
      throw ArgumentError('Name cannot be empty');
    }
    _holderName = name;
  }
  
  // Methods
  void deposit(double amount) {
    if (amount <= 0) {
      print('Deposit amount must be positive');
      return;
    }
    _balance += amount;
    print('Deposited \$${amount.toStringAsFixed(2)}. New balance: \$${_balance.toStringAsFixed(2)}');
  }
  
  bool withdraw(double amount) {
    if (amount <= 0) {
      print('Withdrawal amount must be positive');
      return false;
    }
    if (amount > _balance) {
      print('Insufficient funds. Available balance: \$${_balance.toStringAsFixed(2)}');
      return false;
    }
    _balance -= amount;
    print('Withdrew \$${amount.toStringAsFixed(2)}. New balance: \$${_balance.toStringAsFixed(2)}');
    return true;
  }
  
  void displayAccountInfo() {
    print('Account: $_accountNumber');
    print('Holder: $_holderName');
    print('Balance: \$${_balance.toStringAsFixed(2)}');
  }
}

void main() {
  BankAccount account = BankAccount('ACC123456', 'John Doe', 1000.0);
  
  account.displayAccountInfo();
  
  // Using getters
  print('Account holder: ${account.holderName}');
  print('Current balance: \$${account.balance.toStringAsFixed(2)}');
  
  // Using methods
  account.deposit(500.0);
  account.withdraw(200.0);
  account.withdraw(2000.0); // Should fail
  
  // Using setter
  account.holderName = 'John Smith';
  account.displayAccountInfo();
}
```

**Static properties and methods:**
```dart
class MathUtils {
  // Static property
  static const double pi = 3.14159;
  static int _calculationCount = 0;
  
  // Static methods
  static double calculateCircleArea(double radius) {
    _calculationCount++;
    return pi * radius * radius;
  }
  
  static double calculateRectangleArea(double width, double height) {
    _calculationCount++;
    return width * height;
  }
  
  static int getCalculationCount() {
    return _calculationCount;
  }
  
  static void resetCalculationCount() {
    _calculationCount = 0;
  }
}

class Student {
  String name;
  int grade;
  static int totalStudents = 0; // Tracks total number of students
  
  Student(this.name, this.grade) {
    totalStudents++; // Increment when new student is created
  }
  
  void displayInfo() {
    print('Student: $name, Grade: $grade');
  }
  
  static void displayTotalStudents() {
    print('Total students: $totalStudents');
  }
}

void main() {
  // Using static methods without creating instances
  double circleArea = MathUtils.calculateCircleArea(5.0);
  double rectArea = MathUtils.calculateRectangleArea(4.0, 6.0);
  
  print('Circle area: ${circleArea.toStringAsFixed(2)}');
  print('Rectangle area: ${rectArea.toStringAsFixed(2)}');
  print('Total calculations: ${MathUtils.getCalculationCount()}');
  
  // Using static properties
  print('Value of Pi: ${MathUtils.pi}');
  
  // Tracking students
  Student.displayTotalStudents(); // 0
  
  Student student1 = Student('Alice', 90);
  Student student2 = Student('Bob', 85);
  Student student3 = Student('Charlie', 92);
  
  Student.displayTotalStudents(); // 3
  
  student1.displayInfo();
  student2.displayInfo();
  student3.displayInfo();
}
```

* **Topic 6.2: Constructors**
    * [ ] Default, named, constant, and factory constructors

#### Constructors in Dart

Constructors are special methods used to create and initialize objects. Dart provides several types of constructors.

**Default and basic constructors:**
```dart
class Rectangle {
  double width;
  double height;
  
  // Default constructor
  Rectangle(this.width, this.height);
  
  // Constructor with parameter validation
  Rectangle.withValidation(double width, double height) {
    if (width <= 0 || height <= 0) {
      throw ArgumentError('Width and height must be positive');
    }
    this.width = width;
    this.height = height;
  }
  
  double get area => width * height;
  double get perimeter => 2 * (width + height);
  
  void display() {
    print('Rectangle: ${width}x$height, Area: $area, Perimeter: $perimeter');
  }
}

void main() {
  // Using default constructor
  Rectangle rect1 = Rectangle(5.0, 3.0);
  rect1.display();
  
  // Using constructor with validation
  try {
    Rectangle rect2 = Rectangle.withValidation(4.0, 6.0);
    rect2.display();
    
    Rectangle rect3 = Rectangle.withValidation(-2.0, 3.0); // This will throw an error
  } catch (e) {
    print('Error: $e');
  }
}
```

**Named constructors:**
```dart
class Point {
  double x;
  double y;
  
  // Default constructor
  Point(this.x, this.y);
  
  // Named constructor for origin point
  Point.origin() {
    x = 0;
    y = 0;
  }
  
  // Named constructor from another point
  Point.fromPoint(Point other) {
    x = other.x;
    y = other.y;
  }
  
  // Named constructor from coordinates
  Point.fromCoordinates(double x, double y) : this(x, y);
  
  // Named constructor for point on x-axis
  Point.onXAxis(double x) : this(x, 0);
  
  // Named constructor for point on y-axis  
  Point.onYAxis(double y) : this(0, y);
  
  double distanceFromOrigin() {
    return sqrt(x * x + y * y);
  }
  
  void display() {
    print('Point($x, $y)');
  }
}

// Import for sqrt function
import 'dart:math';

void main() {
  // Using different constructors
  Point p1 = Point(3.0, 4.0);           // Default constructor
  Point p2 = Point.origin();            // Named constructor
  Point p3 = Point.fromPoint(p1);       // Copy constructor
  Point p4 = Point.onXAxis(5.0);        // Point on x-axis
  Point p5 = Point.onYAxis(-3.0);       // Point on y-axis
  
  print('Points created:');
  p1.display();
  p2.display(); 
  p3.display();
  p4.display();
  p5.display();
  
  print('Distance from origin: ${p1.distanceFromOrigin().toStringAsFixed(2)}');
}
```

**Constant constructors:**
```dart
class ImmutablePoint {
  final double x;
  final double y;
  
  // Constant constructor - all fields must be final
  const ImmutablePoint(this.x, this.y);
  
  // Named constant constructor
  const ImmutablePoint.origin() : x = 0, y = 0;
  
  double get distanceFromOrigin => x * x + y * y;
  
  @override
  String toString() => 'ImmutablePoint($x, $y)';
}

class Color {
  final int red;
  final int green;
  final int blue;
  
  const Color(this.red, this.green, this.blue);
  
  // Named constant constructors for common colors
  const Color.black() : red = 0, green = 0, blue = 0;
  const Color.white() : red = 255, green = 255, blue = 255;
  const Color.red() : red = 255, green = 0, blue = 0;
  const Color.green() : red = 0, green = 255, blue = 0;
  const Color.blue() : red = 0, green = 0, blue = 255;
  
  @override
  String toString() => 'Color(r:$red, g:$green, b:$blue)';
}

void main() {
  // Constant objects - created at compile time
  const ImmutablePoint origin = ImmutablePoint.origin();
  const ImmutablePoint point = ImmutablePoint(3, 4);
  
  // Constant colors
  const Color redColor = Color.red();
  const Color customColor = Color(128, 64, 192);
  
  print('Origin: $origin');
  print('Point: $point');
  print('Red color: $redColor');
  print('Custom color: $customColor');
  
  // Comparing constant objects
  const ImmutablePoint p1 = ImmutablePoint(1, 2);
  const ImmutablePoint p2 = ImmutablePoint(1, 2);
  
  print('p1 == p2: ${p1 == p2}'); // true for const objects with same values
  print('identical(p1, p2): ${identical(p1, p2)}'); // true - same object in memory
}
```

**Factory constructors:**
```dart
class Logger {
  String name;
  static final Map<String, Logger> _loggers = {};
  
  // Private constructor
  Logger._internal(this.name);
  
  // Factory constructor - can return existing instance
  factory Logger(String name) {
    if (_loggers.containsKey(name)) {
      return _loggers[name]!;
    } else {
      final logger = Logger._internal(name);
      _loggers[name] = logger;
      return logger;
    }
  }
  
  void log(String message) {
    print('[$name] $message');
  }
}

class DatabaseConnection {
  String host;
  int port;
  bool isConnected = false;
  
  DatabaseConnection._internal(this.host, this.port);
  
  // Factory constructor with connection logic
  factory DatabaseConnection.connect(String host, int port) {
    print('Connecting to $host:$port...');
    
    // Simulate connection validation
    if (host.isEmpty || port <= 0) {
      throw ArgumentError('Invalid host or port');
    }
    
    DatabaseConnection connection = DatabaseConnection._internal(host, port);
    connection.isConnected = true;
    print('Connected successfully!');
    return connection;
  }
  
  void query(String sql) {
    if (!isConnected) {
      print('Error: Not connected to database');
      return;
    }
    print('Executing query: $sql');
  }
  
  void disconnect() {
    isConnected = false;
    print('Disconnected from $host:$port');
  }
}

void main() {
  // Factory constructor - singleton pattern
  Logger logger1 = Logger('App');
  Logger logger2 = Logger('App');
  Logger logger3 = Logger('Database');
  
  print('logger1 == logger2: ${identical(logger1, logger2)}'); // true - same instance
  print('logger1 == logger3: ${identical(logger1, logger3)}'); // false - different instances
  
  logger1.log('Application started');
  logger2.log('User logged in'); // Same logger instance
  logger3.log('Database query executed');
  
  // Factory constructor with validation
  try {
    DatabaseConnection db = DatabaseConnection.connect('localhost', 5432);
    db.query('SELECT * FROM users');
    db.disconnect();
  } catch (e) {
    print('Database error: $e');
  }
}
```

* **Topic 6.3: Inheritance**
    * [ ] Extending classes with `extends`, using `super`, and `@override`

#### Inheritance with extends, super, and @override

Inheritance allows a class to inherit properties and methods from another class, promoting code reuse.

**Basic inheritance:**
```dart
// Base class (parent/superclass)
class Animal {
  String name;
  int age;
  
  Animal(this.name, this.age);
  
  void eat() {
    print('$name is eating');
  }
  
  void sleep() {
    print('$name is sleeping');
  }
  
  void makeSound() {
    print('$name makes a sound');
  }
  
  void displayInfo() {
    print('Animal: $name, Age: $age');
  }
}

// Derived class (child/subclass)
class Dog extends Animal {
  String breed;
  
  Dog(String name, int age, this.breed) : super(name, age);
  
  // Override parent method
  @override
  void makeSound() {
    print('$name barks: Woof! Woof!');
  }
  
  // Additional method specific to Dog
  void wagTail() {
    print('$name is wagging tail happily');
  }
  
  // Override displayInfo to include breed
  @override
  void displayInfo() {
    super.displayInfo(); // Call parent method
    print('Breed: $breed');
  }
}

class Cat extends Animal {
  bool isIndoor;
  
  Cat(String name, int age, this.isIndoor) : super(name, age);
  
  @override
  void makeSound() {
    print('$name meows: Meow! Meow!');
  }
  
  void purr() {
    print('$name is purring contentedly');
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Indoor cat: $isIndoor');
  }
}

void main() {
  // Creating instances of derived classes
  Dog dog = Dog('Buddy', 3, 'Golden Retriever');
  Cat cat = Cat('Whiskers', 2, true);
  
  // Using inherited methods
  dog.eat();
  dog.sleep();
  cat.eat();
  cat.sleep();
  
  // Using overridden methods
  dog.makeSound();
  cat.makeSound();
  
  // Using class-specific methods
  dog.wagTail();
  cat.purr();
  
  // Using overridden displayInfo
  print('\nDog Information:');
  dog.displayInfo();
  
  print('\nCat Information:');
  cat.displayInfo();
}
```

**Advanced inheritance with method overriding:**
```dart
class Vehicle {
  String brand;
  String model;
  int year;
  double speed = 0;
  
  Vehicle(this.brand, this.model, this.year);
  
  void start() {
    print('$brand $model is starting...');
  }
  
  void stop() {
    speed = 0;
    print('$brand $model has stopped');
  }
  
  void accelerate(double increment) {
    speed += increment;
    print('$brand $model is now going ${speed.toStringAsFixed(1)} km/h');
  }
  
  virtual void displaySpeedLimit() {
    print('General speed limit applies');
  }
  
  void displayInfo() {
    print('Vehicle: $brand $model ($year)');
    print('Current speed: ${speed.toStringAsFixed(1)} km/h');
  }
}

class Car extends Vehicle {
  int numberOfDoors;
  String fuelType;
  
  Car(String brand, String model, int year, this.numberOfDoors, this.fuelType) 
      : super(brand, model, year);
  
  @override
  void start() {
    print('Starting car engine...');
    super.start(); // Call parent start method
    print('Car is ready to drive');
  }
  
  @override
  void accelerate(double increment) {
    if (speed + increment > 180) {
      print('Warning: Speed limit for cars is 180 km/h');
      speed = 180;
    } else {
      super.accelerate(increment);
    }
  }
  
  @override
  void displaySpeedLimit() {
    print('Speed limit for cars: 180 km/h');
  }
  
  void honk() {
    print('$brand $model: Beep! Beep!');
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Doors: $numberOfDoors, Fuel: $fuelType');
  }
}

class Motorcycle extends Vehicle {
  bool hasSidecar;
  
  Motorcycle(String brand, String model, int year, this.hasSidecar) 
      : super(brand, model, year);
  
  @override
  void start() {
    print('Kick-starting motorcycle...');
    super.start();
    print('Motorcycle is ready to ride');
  }
  
  @override
  void accelerate(double increment) {
    if (speed + increment > 120) {
      print('Warning: Speed limit for motorcycles is 120 km/h');
      speed = 120;
    } else {
      super.accelerate(increment);
    }
  }
  
  @override
  void displaySpeedLimit() {
    print('Speed limit for motorcycles: 120 km/h');
  }
  
  void wheelie() {
    if (hasSidecar) {
      print('Cannot perform wheelie with sidecar');
    } else {
      print('$brand $model is doing a wheelie!');
    }
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Has sidecar: $hasSidecar');
  }
}

void main() {
  Car car = Car('Toyota', 'Camry', 2023, 4, 'Gasoline');
  Motorcycle bike = Motorcycle('Harley-Davidson', 'Sportster', 2022, false);
  
  print('=== Car Operations ===');
  car.start();
  car.accelerate(50);
  car.accelerate(100);
  car.accelerate(50); // Should trigger speed limit warning
  car.honk();
  car.displaySpeedLimit();
  car.displayInfo();
  car.stop();
  
  print('\n=== Motorcycle Operations ===');
  bike.start();
  bike.accelerate(60);
  bike.accelerate(80); // Should trigger speed limit warning
  bike.wheelie();
  bike.displaySpeedLimit();
  bike.displayInfo();
  bike.stop();
  
  // Polymorphism - treating derived classes as base class
  print('\n=== Polymorphism Demo ===');
  List<Vehicle> vehicles = [car, bike];
  
  for (Vehicle vehicle in vehicles) {
    vehicle.displaySpeedLimit(); // Calls overridden method
    vehicle.accelerate(30);
  }
}
```

**Using super in constructors and methods:**
```dart
class Employee {
  String name;
  String id;
  double baseSalary;
  
  Employee(this.name, this.id, this.baseSalary) {
    print('Employee constructor called for $name');
  }
  
  double calculatePay() {
    return baseSalary;
  }
  
  void displayInfo() {
    print('Employee: $name (ID: $id)');
    print('Base Salary: \$${baseSalary.toStringAsFixed(2)}');
  }
}

class Manager extends Employee {
  double bonus;
  int teamSize;
  
  Manager(String name, String id, double baseSalary, this.bonus, this.teamSize) 
      : super(name, id, baseSalary) {
    print('Manager constructor called for $name');
  }
  
  @override
  double calculatePay() {
    double basePay = super.calculatePay(); // Get base salary from parent
    return basePay + bonus;
  }
  
  @override
  void displayInfo() {
    super.displayInfo(); // Call parent displayInfo first
    print('Bonus: \$${bonus.toStringAsFixed(2)}');
    print('Team Size: $teamSize');
    print('Total Pay: \$${calculatePay().toStringAsFixed(2)}');
  }
  
  void conductMeeting() {
    print('$name is conducting a team meeting with $teamSize members');
  }
}

class Developer extends Employee {
  String programmingLanguage;
  int experienceYears;
  
  Developer(String name, String id, double baseSalary, this.programmingLanguage, this.experienceYears) 
      : super(name, id, baseSalary) {
    print('Developer constructor called for $name');
  }
  
  @override
  double calculatePay() {
    double basePay = super.calculatePay();
    // Add experience bonus
    double experienceBonus = experienceYears * 1000;
    return basePay + experienceBonus;
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Programming Language: $programmingLanguage');
    print('Experience: $experienceYears years');
    print('Total Pay: \$${calculatePay().toStringAsFixed(2)}');
  }
  
  void writeCode() {
    print('$name is writing code in $programmingLanguage');
  }
}

void main() {
  print('Creating employees...\n');
  
  Manager manager = Manager('Alice Johnson', 'MGR001', 80000, 15000, 5);
  Developer developer = Developer('Bob Smith', 'DEV001', 70000, 'Dart', 3);
  
  print('\n=== Manager Information ===');
  manager.displayInfo();
  manager.conductMeeting();
  
  print('\n=== Developer Information ===');
  developer.displayInfo();
  developer.writeCode();
  
  // Demonstrating polymorphism
  print('\n=== Payroll Processing ===');
  List<Employee> employees = [manager, developer];
  
  double totalPayroll = 0;
  for (Employee emp in employees) {
    double pay = emp.calculatePay(); // Calls overridden method
    totalPayroll += pay;
    print('${emp.name}: \$${pay.toStringAsFixed(2)}');
  }
  
  print('Total Payroll: \$${totalPayroll.toStringAsFixed(2)}');
}
```

### **Module 7: Object-Oriented Programming (OOP) - Part 2**

* **Topic 7.1: Advanced Blueprints**
    * [ ] Abstract classes and methods
    * [ ] Implicit interfaces with `implements`
    * [ ] Reusing code with `mixins`

#### Abstract Classes and Methods

Abstract classes define a contract that subclasses must follow. They cannot be instantiated directly.

```dart
// Abstract class
abstract class Shape {
  String name;
  
  Shape(this.name);
  
  // Abstract methods - must be implemented by subclasses
  double calculateArea();
  double calculatePerimeter();
  
  // Concrete method - can be used by subclasses
  void displayInfo() {
    print('Shape: $name');
    print('Area: ${calculateArea().toStringAsFixed(2)}');
    print('Perimeter: ${calculatePerimeter().toStringAsFixed(2)}');
  }
}

class Circle extends Shape {
  double radius;
  
  Circle(this.radius) : super('Circle');
  
  @override
  double calculateArea() => 3.14159 * radius * radius;
  
  @override
  double calculatePerimeter() => 2 * 3.14159 * radius;
}

class Rectangle extends Shape {
  double width, height;
  
  Rectangle(this.width, this.height) : super('Rectangle');
  
  @override
  double calculateArea() => width * height;
  
  @override
  double calculatePerimeter() => 2 * (width + height);
}

void main() {
  // Shape shape = Shape('Generic'); // Error: Cannot instantiate abstract class
  
  List<Shape> shapes = [
    Circle(5.0),
    Rectangle(4.0, 6.0),
  ];
  
  for (Shape shape in shapes) {
    shape.displayInfo();
    print('---');
  }
}
```

#### Implicit Interfaces with implements

Every class defines an implicit interface. Use `implements` to ensure a class provides all methods of another class.

```dart
class Flyable {
  void fly() => print('Flying through the air');
}

class Swimmable {
  void swim() => print('Swimming in water');
}

// Bird implements both interfaces
class Bird implements Flyable, Swimmable {
  String species;
  
  Bird(this.species);
  
  @override
  void fly() => print('$species is flying with wings');
  
  @override
  void swim() => print('$species is swimming on water surface');
}

class Fish implements Swimmable {
  String species;
  
  Fish(this.species);
  
  @override
  void swim() => print('$species is swimming underwater');
  
  // Fish doesn't need to implement fly()
}

void main() {
  Bird duck = Bird('Duck');
  Fish salmon = Fish('Salmon');
  
  duck.fly();
  duck.swim();
  salmon.swim();
}
```

#### Reusing Code with Mixins

Mixins allow you to reuse code across multiple class hierarchies.

```dart
mixin Walkable {
  void walk() => print('Walking on legs');
}

mixin Flyable {
  void fly() => print('Flying with wings');
}

mixin Swimmable {
  void swim() => print('Swimming in water');
}

class Animal {
  String name;
  Animal(this.name);
}

class Duck extends Animal with Walkable, Flyable, Swimmable {
  Duck(String name) : super(name);
  
  @override
  void fly() => print('$name is flying low over water');
}

class Penguin extends Animal with Walkable, Swimmable {
  Penguin(String name) : super(name);
  
  @override
  void swim() => print('$name is swimming underwater');
}

void main() {
  Duck duck = Duck('Donald');
  Penguin penguin = Penguin('Pingu');
  
  duck.walk();
  duck.fly();
  duck.swim();
  
  penguin.walk();
  penguin.swim();
  // penguin.fly(); // Error: Penguin doesn't have fly()
}
```

* **Topic 7.2: Modern OOP Features**
    * [ ] Enhanced `enums`
    * [ ] Class modifiers (`interface`, `base`, `final`, `sealed`)
    * [ ] Adding functionality with `extension` methods

#### Enhanced Enums

Modern Dart enums can have fields, methods, and constructors.

```dart
enum Planet {
  mercury(3.303e+23, 2.4397e6),
  venus(4.869e+24, 6.0518e6),
  earth(5.976e+24, 6.37814e6),
  mars(6.421e+23, 3.3972e6);
  
  const Planet(this.mass, this.radius);
  
  final double mass;       // in kilograms
  final double radius;     // in meters
  
  double get surfaceGravity => 6.67300E-11 * mass / (radius * radius);
  
  double surfaceWeight(double mass) => mass * surfaceGravity;
}

enum Status {
  pending('Pending', '⏳'),
  approved('Approved', '✅'),
  rejected('Rejected', '❌');
  
  const Status(this.label, this.icon);
  
  final String label;
  final String icon;
  
  @override
  String toString() => '$icon $label';
}

void main() {
  print('Planet data:');
  for (Planet planet in Planet.values) {
    print('${planet.name}: Surface gravity = ${planet.surfaceGravity.toStringAsFixed(2)} m/s²');
  }
  
  print('\nStatus examples:');
  for (Status status in Status.values) {
    print(status);
  }
}
```

#### Class Modifiers

```dart
// Interface class - can only be implemented, not extended
interface class Drawable {
  void draw();
}

// Base class - can be extended but not implemented
base class Vehicle {
  void start() => print('Vehicle starting');
}

// Final class - cannot be extended or implemented
final class DatabaseConnection {
  void connect() => print('Connected to database');
}

// Sealed class - can only be extended in the same library
sealed class Result<T> {
  const Result();
}

class Success<T> extends Result<T> {
  final T value;
  const Success(this.value);
}

class Error<T> extends Result<T> {
  final String message;
  const Error(this.message);
}

void main() {
  Result<String> result = Success('Data loaded');
  
  switch (result) {
    case Success(value: var data):
      print('Success: $data');
    case Error(message: var msg):
      print('Error: $msg');
  };
}
```

#### Extension Methods

Add functionality to existing classes without modifying them.

```dart
extension StringExtensions on String {
  bool get isEmail => contains('@') && contains('.');
  
  String get capitalizeFirst {
    if (isEmpty) return this;
    return this[0].toUpperCase() + substring(1);
  }
  
  String reverse() => split('').reversed.join('');
  
  int get wordCount => trim().isEmpty ? 0 : split(' ').length;
}

extension ListExtensions<T> on List<T> {
  T? get firstOrNull => isEmpty ? null : first;
  T? get lastOrNull => isEmpty ? null : last;
  
  List<T> get unique => {...this}.toList();
  
  void addIfNotNull(T? item) {
    if (item != null) add(item);
  }
}

void main() {
  String email = 'user@example.com';
  String text = 'hello world';
  
  print('Is email: ${email.isEmail}');
  print('Capitalized: ${text.capitalizeFirst}');
  print('Reversed: ${text.reverse()}');
  print('Word count: ${text.wordCount}');
  
  List<int> numbers = [1, 2, 2, 3, 3, 3];
  print('Unique numbers: ${numbers.unique}');
  print('First or null: ${numbers.firstOrNull}');
}
```

### **Module 8: Asynchronous Programming**

* **Topic 8.1: Core Concepts**
    * [ ] Understanding the event loop
    * [ ] Working with `Future` for one-time async results
    * [ ] Using `async` and `await` for clean code

#### Understanding Futures and Async/Await

```dart
import 'dart:async';
import 'dart:math';

// Simulate network delay
Future<String> fetchUserData(String userId) async {
  print('Fetching user data for $userId...');
  await Future.delayed(Duration(seconds: 2));
  
  if (Random().nextBool()) {
    return 'User data for $userId';
  } else {
    throw Exception('Failed to fetch user data');
  }
}

Future<List<String>> fetchMultipleUsers(List<String> userIds) async {
  List<String> results = [];
  
  for (String id in userIds) {
    try {
      String userData = await fetchUserData(id);
      results.add(userData);
    } catch (e) {
      results.add('Error: $e');
    }
  }
  
  return results;
}

void main() async {
  print('Starting async operations...');
  
  try {
    // Single async operation
    String user = await fetchUserData('123');
    print('Received: $user');
    
    // Multiple async operations
    List<String> users = await fetchMultipleUsers(['1', '2', '3']);
    users.forEach(print);
    
  } catch (e) {
    print('Error: $e');
  }
  
  print('Program completed');
}
```

* **Topic 8.2: Advanced Asynchrony**
    * [ ] Handling data sequences with `Stream`
    * [ ] True parallelism with `Isolates`
    * [ ] Lazy value generation with `sync*`/`async*` (Generators)

#### Streams and Generators

```dart
import 'dart:async';

// Stream generator
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(milliseconds: 500));
    yield i;
  }
}

// Sync generator
Iterable<int> fibonacci(int n) sync* {
  int a = 0, b = 1;
  for (int i = 0; i < n; i++) {
    yield a;
    int temp = a + b;
    a = b;
    b = temp;
  }
}

void main() async {
  print('Stream example:');
  await for (int value in countStream(5)) {
    print('Received: $value');
  }
  
  print('\nFibonacci sequence:');
  for (int value in fibonacci(10)) {
    print(value);
  }
}
```

### **Module 9: Null Safety**

* **Topic 9.1: Understanding the System**
    * [ ] The principle of sound null safety
    * [ ] Nullable (`?`) vs. non-nullable types

* **Topic 9.2: Practical Application**
    * [ ] Type promotion and flow analysis
    * [ ] Using the assertion (`!`) and `late` keywords safely

#### Null Safety in Practice

```dart
class User {
  String name;
  String? email; // Nullable
  late String id; // Late initialization
  
  User(this.name, {this.email});
  
  void initializeId() {
    id = 'user_${name.toLowerCase().replaceAll(' ', '_')}';
  }
  
  String getDisplayName() {
    // Type promotion - Dart knows email is not null inside this block
    if (email != null) {
      return '$name (${email!.split('@')[0]})';
    }
    return name;
  }
  
  void sendEmail(String message) {
    // Null-aware operator
    email?.isNotEmpty == true 
        ? print('Sending email to $email: $message')
        : print('No email available for $name');
  }
}

void main() {
  User user1 = User('John Doe', email: 'john@example.com');
  User user2 = User('Jane Smith'); // No email
  
  user1.initializeId();
  user2.initializeId();
  
  print(user1.getDisplayName());
  print(user2.getDisplayName());
  
  user1.sendEmail('Welcome!');
  user2.sendEmail('Welcome!');
}
```

### **Module 10: Modern Dart Features**

* **Topic 10.1: Records & Patterns**
    * [ ] Returning multiple values with Records
    * [ ] Destructuring data with patterns
    * [ ] Advanced logic with pattern matching (`switch`, `if-case`)

#### Records and Pattern Matching

```dart
// Function returning multiple values using records
(String, int, bool) getUserInfo(String userId) {
  // Simulate database lookup
  return ('John Doe', 25, true); // (name, age, isActive)
}

// Pattern matching with switch expressions
String categorizeAge(int age) => switch (age) {
  < 13 => 'Child',
  >= 13 && < 20 => 'Teenager',
  >= 20 && < 60 => 'Adult',
  _ => 'Senior'
};

void main() {
  // Record destructuring
  var (name, age, isActive) = getUserInfo('123');
  print('User: $name, Age: $age, Active: $isActive');
  
  // Pattern matching with switch
  List<dynamic> data = ['Alice', 30, true];
  
  switch (data) {
    case [String name, int age, bool active] when age >= 18:
      print('Adult user: $name');
    case [String name, int age, bool active]:
      print('Minor user: $name');
    default:
      print('Invalid data format');
  }
  
  print('Age category: ${categorizeAge(age)}');
}
```

* **Topic 10.2: Error Handling**
    * [ ] Managing exceptions with `try`, `catch`, `finally`, and `throw`

#### Exception Handling

```dart
class CustomException implements Exception {
  final String message;
  CustomException(this.message);
  
  @override
  String toString() => 'CustomException: $message';
}

int divide(int a, int b) {
  if (b == 0) {
    throw CustomException('Division by zero is not allowed');
  }
  return a ~/ b;
}

void main() {
  List<List<int>> testCases = [
    [10, 2],
    [15, 3],
    [8, 0], // This will throw an exception
    [20, 4]
  ];
  
  for (var testCase in testCases) {
    try {
      int result = divide(testCase[0], testCase[1]);
      print('${testCase[0]} ÷ ${testCase[1]} = $result');
    } on CustomException catch (e) {
      print('Custom error: $e');
    } catch (e) {
      print('Unexpected error: $e');
    } finally {
      print('Division operation completed');
    }
  }
}
```

### **Module 11: The Dart Ecosystem**

* **Topic 11.1: Libraries and Packages**
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

* **Topic 11.2: Core Libraries**
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

* **Topic 12.1: The Professional Toolchain**
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

* **Topic 12.2: Platform Interoperability**
    * [ ] Calling C code with Foreign Function Interface (FFI)
    * [ ] Interacting with Kotlin/Java and Swift/Objective-C code

#### FFI Example

```dart
import 'dart:ffi';
import 'dart:io';

// C function signature: int add(int a, int b)
typedef AddC = Int32 Function(Int32 a, Int32 b);
typedef AddDart = int Function(int a, int b);

void main() {
  // Load the dynamic library
  final dylib = Platform.isWindows 
      ? DynamicLibrary.open('math_lib.dll')
      : DynamicLibrary.open('libmath_lib.so');
  
  // Look up the function
  final add = dylib.lookupFunction<AddC, AddDart>('add');
  
  // Call the C function
  final result = add(5, 3);
  print('5 + 3 = $result');
}
```

### **Module 13: Best Practices & Next Steps**

* **Topic 13.1: Writing Quality Code**
    * [ ] Following the "Effective Dart" style and usage guides

#### Effective Dart Guidelines

```dart
// Good: Use clear, descriptive names
class UserRepository {
  final HttpClient _httpClient;
  
  UserRepository(this._httpClient);
  
  // Good: Use async/await for readability
  Future<User> fetchUser(String id) async {
    try {
      final response = await _httpClient.get('/users/$id');
      return User.fromJson(response.data);
    } on HttpException catch (e) {
      throw UserNotFoundException('User $id not found: ${e.message}');
    }
  }
  
  // Good: Use meaningful parameter names
  Future<List<User>> searchUsers({
    required String query,
    int limit = 10,
    int offset = 0,
  }) async {
    // Implementation
    return [];
  }
}

// Good: Document public APIs
/// Represents a user in the system.
/// 
/// Contains basic information like name, email, and creation date.
class User {
  final String id;
  final String name;
  final String email;
  final DateTime createdAt;
  
  const User({
    required this.id,
    required this.name,
    required this.email,
    required this.createdAt,
  });
  
  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] as String,
      name: json['name'] as String,
      email: json['email'] as String,
      createdAt: DateTime.parse(json['created_at'] as String),
    );
  }
}
```

* **Topic 13.2: Compilation and Deployment**
    * [ ] Understanding JIT (development) vs. AOT (production) compilation

#### Compilation Types

```dart
// Development (JIT - Just In Time)
// - Fast startup
// - Hot reload support
// - Larger memory usage
// Command: dart run main.dart

// Production (AOT - Ahead Of Time)  
// - Optimized performance
// - Smaller runtime footprint
// - No hot reload
// Commands:
// dart compile exe main.dart        # Native executable
// dart compile js main.dart         # JavaScript (web)
// dart compile aot-snapshot main.dart # AOT snapshot

void main() {
  print('Dart compilation demonstration');
  print('JIT: Great for development');
  print('AOT: Perfect for production');
}
```

* **Topic 13.3: Where to Go Next**
    * [ ] Applying Dart skills to build applications with **Flutter**

#### Next Steps with Flutter

```dart
// Example Flutter app structure using Dart concepts learned

import 'package:flutter/material.dart';

// Using classes and inheritance
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Dart to Flutter',
      home: UserListScreen(),
    );
  }
}

// Using async programming
class UserListScreen extends StatefulWidget {
  @override
  _UserListScreenState createState() => _UserListScreenState();
}

class _UserListScreenState extends State<UserListScreen> {
  List<User> users = [];
  bool isLoading = true;
  
  @override
  void initState() {
    super.initState();
    loadUsers();
  }
  
  Future<void> loadUsers() async {
    try {
      // Using the concepts learned in previous modules
      final repository = UserRepository(HttpClient());
      final fetchedUsers = await repository.fetchAllUsers();
      
      setState(() {
        users = fetchedUsers;
        isLoading = false;
      });
    } catch (e) {
      setState(() {
        isLoading = false;
      });
      // Handle error
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Users')),
      body: isLoading
          ? Center(child: CircularProgressIndicator())
          : ListView.builder(
              itemCount: users.length,
              itemBuilder: (context, index) {
                final user = users[index];
                return ListTile(
                  title: Text(user.name),
                  subtitle: Text(user.email),
                );
              },
            ),
    );
  }
}

void main() {
  runApp(MyApp());
}
```

---

## **Congratulations! 🎉**

You've completed the comprehensive Dart learning roadmap! You now have:

- **Solid foundations** in Dart syntax and concepts
- **Object-oriented programming** skills
- **Asynchronous programming** knowledge
- **Modern Dart features** understanding
- **Best practices** for writing quality code

### **What's Next?**

1. **Build Projects**: Create CLI tools, web servers, or scripts using Dart
2. **Learn Flutter**: Apply your Dart skills to mobile app development
3. **Explore Advanced Topics**: Study design patterns, state management, and architecture
4. **Join the Community**: Participate in Dart and Flutter communities
5. **Keep Learning**: Stay updated with new Dart features and best practices

### **Resources for Continued Learning:**

- [Dart Language Tour](https://dart.dev/language)
- [Effective Dart](https://dart.dev/effective-dart)
- [Flutter Documentation](https://flutter.dev/docs)
- [Dart Packages](https://pub.dev)
- [DartPad Online IDE](https://dartpad.dev)

Happy coding with Dart! 🚀
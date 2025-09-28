# Topic 2.3: Operators

[⬅ Previous](topic-2-2-built-in-data-types.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-3-1-conditional-statements.md)

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

[⬅ Previous](topic-2-2-built-in-data-types.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-3-1-conditional-statements.md)

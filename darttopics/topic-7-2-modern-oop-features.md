# Topic 7.2: Modern OOP Features

[⬅ Previous](topic-7-1-advanced-blueprints.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-8-1-core-concepts.md)

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

[⬅ Previous](topic-7-1-advanced-blueprints.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-8-1-core-concepts.md)

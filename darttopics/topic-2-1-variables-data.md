# Topic 2.1: Variables & Data

[⬅ Previous](topic-1-2-your-first-program.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-2-2-built-in-data-types.md)

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
class Configuration {
  // The initializer runs only the first time the field is accessed.
  late final String apiEndpoint = _loadApiEndpoint();

  String _loadApiEndpoint() {
    print('Loading API endpoint from disk...');
    return 'https://api.example.com/v1';
  }
}

void main() {
  final config = Configuration();
  print('Configuration created');

  // First access triggers the lazy initializer.
  print('API endpoint: ${config.apiEndpoint}');

  // Subsequent accesses reuse the cached value without recomputation.
  print('Cached endpoint: ${config.apiEndpoint}');
}
```

[⬅ Previous](topic-1-2-your-first-program.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-2-2-built-in-data-types.md)

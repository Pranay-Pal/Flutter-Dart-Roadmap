# Topic 7.2: Modern OOP Features in Dart

[⬅ Previous](topic-7-1-advanced-blueprints.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-8-1-core-concepts.md)

Dart is a modern, evolving language. Recent updates have introduced powerful features that give you more control over your class hierarchies and allow you to extend existing classes in new ways.

This topic covers three of the most impactful modern features:
- **Enhanced Enums:** Enums that can have fields, methods, and constructors.
- **Class Modifiers:** Keywords like `interface`, `base`, `final`, and `sealed` that control how classes can be used.
- **Extension Methods:** A way to add new functionality to existing classes without modifying their source code.

---

### 1. Enhanced Enums: More Than Just a List

In modern Dart, enums are no longer just simple lists of values. They can now be structured like classes, with fields, methods, and constructors. This is incredibly useful for modeling data where each enumerated value has associated properties or behaviors.

**Key Features:**
- Enums can have `final` fields to hold data.
- They must have a `const` constructor to initialize their fields.
- They can have methods and getters to provide behavior.

```dart
// An enhanced enum to model different levels of a subscription plan.
enum SubscriptionPlan {
  free('Free', 0.00),
  premium('Premium', 9.99),
  vip('VIP', 29.99);

  // 1. Fields for each enum value.
  final String displayName;
  final double price;

  // 2. A const constructor is required.
  const SubscriptionPlan(this.displayName, this.price);

  // 3. Methods and getters can be added.
  bool get hasUnlimitedAccess => price > 20.0;

  void displayInfo() {
    print('Plan: $displayName, Price: \$$price/month');
  }
}

void main() {
  SubscriptionPlan myPlan = SubscriptionPlan.premium;

  myPlan.displayInfo(); // Output: Plan: Premium, Price: $9.99/month
  print('Has unlimited access: ${myPlan.hasUnlimitedAccess}'); // Output: false

  SubscriptionPlan vipPlan = SubscriptionPlan.vip;
  print('Has unlimited access: ${vipPlan.hasUnlimitedAccess}'); // Output: true
}
```

---

### 2. Class Modifiers: Controlling Inheritance and Implementation

Class modifiers are keywords you place before `class` to control how other developers can use your class. They help you build more robust and predictable APIs.

| Modifier | Purpose | Can be `extended`? | Can be `implemented`? |
| :--- | :--- | :--- | :--- |
| **(none)** | Default class | Yes | Yes |
| **`interface`** | Defines a pure contract. | No | Yes |
| **`base`** | Forces subclasses to inherit from it, not implement it. Guarantees that the base class's constructor is called and that all private members are available to subclasses. | Yes | No |
| **`final`** | Prevents all inheritance/implementation. | No | No |
| **`sealed`** | Creates a "closed" set of subtypes known at compile time. | Yes (in same file) | Yes (in same file) |

#### Example: `sealed` for Exhaustive `switch`
A `sealed` class is particularly powerful because it tells the compiler exactly what all the possible subtypes are. This allows you to write `switch` statements without a `default` case, and the compiler will warn you if you forget to handle a type.

```dart
// A sealed class for representing the result of an operation.
sealed class Result {}

// The only two possible outcomes.
class Success extends Result {
  final String message;
  Success(this.message);
}

class Failure extends Result {
  final String error;
  Failure(this.error);
}

// This function MUST handle both Success and Failure.
String processResult(Result res) {
  return switch (res) {
    case Success(message: final m):
      return 'Operation succeeded: $m';
    case Failure(error: final e):
      return 'Operation failed: $e';
  };
  // No 'default' case is needed! The compiler knows we've covered all possibilities.
}

void main() {
  print(processResult(Success('Data saved.')));
  print(processResult(Failure('Network timeout.')));
}
```

---

### 3. Extension Methods: Adding Functionality to Existing Classes

Have you ever wished you could add a new method to `String` or `List`? **Extension methods** let you do just that. They allow you to add new functionality to existing classes, even if you don't have access to their source code.

This is a clean way to build "utility" functions without creating wrapper classes.

**Syntax:** `extension ExtensionName on ClassName { ...methods... }`

```dart
// Let's add some useful methods to the built-in String class.
extension StringParsing on String {
  // A getter to check if a string can be parsed as an integer.
  bool get isInt {
    return int.tryParse(this) != null;
  }

  // A method to capitalize the first letter.
  String capitalize() {
    if (isEmpty) return '';
    return '${this[0].toUpperCase()}${substring(1)}';
  }
}

void main() {
  String text = 'hello world';
  String number = '123';
  String notANumber = 'abc';

  // Call our new extension method as if it were part of the String class.
  print(text.capitalize()); // Output: Hello world

  print('"$number" is an integer: ${number.isInt}'); // Output: "123" is an integer: true
  print('"$notANumber" is an integer: ${notANumber.isInt}'); // Output: "abc" is an integer: false
}
```

---

### 4. Operator Overloading: Customizing Operators

Dart allows you to define the behavior of its existing operators for your own classes using the `operator` keyword. This can make your code more intuitive and readable. For example, you can define what `+` means for a `Vector` class or how `==` should compare two `Point` objects.

**Common Overloadable Operators:**
- Arithmetic: `+`, `-`, `*`, `/`
- Equality and Relational: `==`, `<`, `>`
- Access: `[]` (for index access), `[]=` (for index assignment)

```dart
// A simple 2D Vector class.
class Vector {
  final int x;
  final int y;

  const Vector(this.x, this.y);

  // 1. Overload the '+' operator to add two vectors.
  operator +(Vector other) {
    return Vector(x + other.x, y + other.y);
  }

  // 2. Overload the '*' operator for scalar multiplication.
  operator *(int scalar) {
    return Vector(x * scalar, y * scalar);
  }

  // 3. It's crucial to override '==' for meaningful equality checks.
  // If you override ==, you must also override hashCode.
  @override
  bool operator ==(Object other) {
    if (other is Vector) {
      return x == other.x && y == other.y;
    }
    return false;
  }

  @override
  int get hashCode => x.hashCode ^ y.hashCode;

  @override
  String toString() => 'Vector($x, $y)';
}

void main() {
  final v1 = Vector(2, 3);
  final v2 = Vector(4, 1);

  // Use the overloaded '+' operator.
  final sum = v1 + v2;
  print('$v1 + $v2 = $sum'); // Output: Vector(2, 3) + Vector(4, 1) = Vector(6, 4)

  // Use the overloaded '*' operator.
  final scaled = v1 * 3;
  print('$v1 * 3 = $scaled'); // Output: Vector(2, 3) * 3 = Vector(6, 9)
  
  final v3 = Vector(2, 3);
  // Use the overloaded '==' operator.
  print('$v1 == $v3: ${v1 == v3}'); // Output: Vector(2, 3) == Vector(2, 3): true
}
```

These modern features make Dart a more expressive, safe, and pleasant language to work with, enabling you to write cleaner and more maintainable object-oriented code.

[⬅ Previous](topic-7-1-advanced-blueprints.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-8-1-core-concepts.md)

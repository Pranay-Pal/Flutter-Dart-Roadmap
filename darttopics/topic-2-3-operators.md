# Topic 2.3: Operators

[⬅ Previous](topic-2-2-built-in-data-types.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-3-1-conditional-statements.md)

**Operators** are special symbols that perform operations on values (called operands). For example, in `5 + 3`, the `+` is the operator and `5` and `3` are the operands.

Dart provides a rich set of operators to handle everything from basic math to complex logical comparisons. Understanding them is key to writing expressive and concise code.

---

### 1. Arithmetic Operators

These are used for performing mathematical calculations.

| Operator | Description        | Example         | Result |
| :------- | :----------------- | :-------------- | :----- |
| `+`      | Addition           | `5 + 2`         | `7`    |
| `-`      | Subtraction        | `5 - 2`         | `3`    |
| `*`      | Multiplication     | `5 * 2`         | `10`   |
| `/`      | Division           | `5 / 2`         | `2.5`  |
| `~/`     | Integer Division   | `5 ~/ 2`        | `2`    |
| `%`      | Modulo (remainder) | `5 % 2`         | `1`    |

**Increment and Decrement Operators:**
- `++variable`: Increments the value *before* the expression is evaluated.
- `variable++`: Increments the value *after* the expression is evaluated.
- `--variable`: Decrements the value *before* evaluation.
- `variable--`: Decrements the value *after* evaluation.

```dart
void main() {
  int a = 10;
  
  // Pre-increment: increments `a` to 11, then prints 11.
  print('Pre-increment: ${++a}'); // Output: 11

  // Post-increment: prints the current value of `a` (11), then increments it to 12.
  print('Post-increment: ${a++}'); // Output: 11
  print('After post-increment: $a'); // Output: 12
}
```

---

### 2. Equality and Relational Operators

These operators compare two values and always return a boolean (`true` or `false`).

| Operator | Description              | Example      | Result  |
| :------- | :----------------------- | :----------- | :------ |
| `==`     | Equal to                 | `5 == 5`     | `true`  |
| `!=`     | Not equal to             | `5 != 3`     | `true`  |
| `>`      | Greater than             | `5 > 3`      | `true`  |
| `<`      | Less than                | `5 < 3`      | `false` |
| `>=`     | Greater than or equal to | `5 >= 5`     | `true`  |
| `<=`     | Less than or equal to    | `5 <= 3`     | `false` |

```dart
void main() {
  int score = 95;
  String name = 'Dart';

  print(score >= 60);   // true
  print(name == 'dart'); // false (comparison is case-sensitive)
  print(name != 'Java'); // true
}
```

---

### 3. Logical Operators

Logical operators are used to combine or invert boolean expressions.

| Operator | Description                                | Example               |
| :------- | :----------------------------------------- | :-------------------- |
| `!`      | **NOT**: Inverts the value (true becomes false, and vice versa). | `!true` is `false`    |
| `&&`     | **AND**: Returns `true` only if both expressions are `true`.     | `true && false` is `false` |
| `||`     | **OR**: Returns `true` if at least one expression is `true`.      | `true || false` is `true`  |

```dart
void main() {
  bool isRaining = false;
  bool isWeekend = true;

  // Using AND (&&)
  if (isWeekend && !isRaining) {
    print('It\'s a perfect day for a picnic!');
  }

  // Using OR (||)
  bool hasHoliday = true;
  if (isWeekend || hasHoliday) {
    print('Time to relax!');
  }
}
```

---

### 4. Type Test Operators

These operators are used to check the type of an object at runtime.

- `is`: Returns `true` if the object has the specified type.
- `is!`: Returns `true` if the object does not have the specified type.

```dart
void main() {
  void printValue(dynamic value) {
    if (value is String) {
      print('The value is a String: "${value.toUpperCase()}"');
    } else if (value is int) {
      print('The value is an integer: ${value * 2}');
    } else {
      print('The value is of an unknown type.');
    }
  }

  printValue('Hello Dart'); // The value is a String: "HELLO DART"
  printValue(100);         // The value is an integer: 200
  printValue(true);        // The value is of an unknown type.
}
```

---

### 5. Assignment Operators

These operators are used to assign a value to a variable.

| Operator | Shorthand For... | Example         |
| :------- | :--------------- | :-------------- |
| `=`      | `a = b`          | `x = 10`        |
| `+=`     | `a = a + b`      | `x += 5` (x is 15) |
| `-=`     | `a = a - b`      | `x -= 5` (x is 5)  |
| `*=`     | `a = a * b`      | `x *= 2` (x is 20) |
| `??=`    | `a ??= b`        | Assign `b` to `a` only if `a` is `null`. |

```dart
void main() {
  int a = 10;
  a += 5; // a is now 15
  print('a after += 5: $a');

  String? name; // name is null
  // If `name` is null, assign 'Guest' to it.
  name ??= 'Guest';
  print('Name: $name'); // Name: Guest

  // `name` is no longer null, so this does nothing.
  name ??= 'Admin';
  print('Name: $name'); // Name: Guest
}
```

---

### 6. Conditional and Null-Aware Operators

These operators provide concise ways to handle `null` values and conditional expressions.

- **Ternary Operator (`condition ? expr1 : expr2`)**: If `condition` is true, evaluates `expr1`; otherwise, evaluates `expr2`.
- **Null Coalescing Operator (`expr1 ?? expr2`)**: If `expr1` is not `null`, returns its value; otherwise, evaluates and returns `expr2`.
- **Conditional Member Access (`?.`)**: Like the `.` operator, but returns `null` if the object on the left is `null` (preventing a crash).

```dart
void main() {
  // Ternary operator
  int age = 20;
  String status = age >= 18 ? 'Adult' : 'Minor';
  print('Status: $status'); // Status: Adult

  // Null coalescing operator
  String? username; // null
  String displayName = username ?? 'Anonymous';
  print('Display Name: $displayName'); // Display Name: Anonymous

  // Conditional member access
  String? message; // null
  // This would crash: message.toUpperCase()
  // This is safe and returns null:
  print(message?.toUpperCase()); // null
}
```

---

### 7. Cascade Notation (`..`)

The cascade notation allows you to perform a sequence of operations on the same object. It provides a more fluent and readable syntax than calling each method on a separate line.

```dart
class Paint {
  String color = 'white';
  double strokeWidth = 1.0;

  void setColor(String newColor) {
    color = newColor;
    print('Color set to $color');
  }

  void setStroke(double newWidth) {
    strokeWidth = newWidth;
    print('Stroke width set to $strokeWidth');
  }
}

void main() {
  // Without cascade
  var paint1 = Paint();
  paint1.setColor('red');
  paint1.setStroke(5.0);

  // With cascade
  var paint2 = Paint()
    ..setColor('blue')
    ..setStroke(10.0);
}
```

---

### Operator Precedence

When an expression contains multiple operators, Dart follows a set order of precedence to decide which to evaluate first (e.g., `*` before `+`).

For example, `2 + 3 * 4` evaluates to `14`, not `20`, because multiplication has higher precedence. You can use parentheses `()` to change the order of evaluation.

`(2 + 3) * 4` evaluates to `20`.

For a complete list of operator precedence, refer to the [official Dart documentation](https://dart.dev/language/operators#operator-precedence).

[⬅ Previous](topic-2-2-built-in-data-types.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-3-1-conditional-statements.md)

# Topic 3.3: Switch and Case

[⬅ Previous](topic-3-2-loops.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-1-lists.md)

The **`switch` statement** provides a clean and powerful way to control program flow based on the value of a variable or expression. It is often a more readable alternative to a long chain of `if-else if` statements.

Modern Dart has significantly enhanced `switch` with **pattern matching** and **switch expressions**, making it a versatile tool for handling complex data structures and ensuring all possibilities are handled.

---

### 1. Traditional `switch` Statement

The classic `switch` statement compares a value against several `case` clauses. When a match is found, the code inside that `case` is executed until a `break` statement is encountered.

**Syntax:**
```dart
switch (variable) {
  case value1:
    // Code for value1
    break;
  case value2:
    // Code for value2
    break;
  default:
    // Code if no case matches
}
```

**Example:**
Let's identify the day of the week.

```dart
void main() {
  int dayOfWeek = 3;
  
  switch (dayOfWeek) {
    case 1:
      print('Monday');
      break;
    case 2:
      print('Tuesday');
      break;
    case 3:
      print('Wednesday');
      break;
    // ... other days
    default:
      print('Invalid day');
  }
}
```

You can also group cases that should execute the same code block.

```dart
void main() {
  String fruit = 'apple';
  
  switch (fruit) {
    case 'apple':
    case 'banana':
      print('It is a fruit.');
      break;
    case 'potato':
      print('It is a vegetable.');
      break;
  }
}
```

---

### 2. `switch` as an Expression

In modern Dart, `switch` can be used as an expression that returns a value. This form is more concise and often safer because the compiler can check that you've handled all possible cases (a property called **exhaustiveness**).

**Syntax:**
```dart
var result = switch (variable) {
  pattern1 => value1,
  pattern2 => value2,
  _ => defaultValue // The underscore `_` is a wildcard pattern.
};
```

**Example:**
Let's get a description for a status enum.

```dart
enum Status { pending, running, completed, failed }

void main() {
  var currentStatus = Status.completed;

  // The switch expression evaluates to a String.
  var message = switch (currentStatus) {
    Status.pending => 'The process is waiting to start.',
    Status.running => 'The process is currently running.',
    Status.completed => 'The process finished successfully.',
    Status.failed => 'The process encountered an error.'
    // No default case is needed because all enum values are handled.
  };

  print(message);
}
```

---

### 3. Pattern Matching in `switch`

Pattern matching supercharges `switch` by allowing you to check not just for equality but also for the **shape** and **properties** of your data.

**a. Matching on Type**

You can execute different code based on an object's type.

```dart
void main() {
  Object value = 123; // The type is Object, but the value is an int.

  var result = switch (value) {
    int() => 'It is an integer.',
    String() => 'It is a String.',
    bool() => 'It is a boolean.',
    _ => 'It is some other type.'
  };
  
  print(result); // Output: It is an integer.
}
```

**b. Matching with Guard Clauses (`when`)**

You can add an extra `if` condition to a `case` using the `when` keyword.

```dart
void main() {
  Object value = 100;

  var result = switch (value) {
    int() when value > 50 => 'An integer greater than 50.',
    int() => 'An integer 50 or less.',
    _ => 'Not an integer.'
  };
  
  print(result); // Output: An integer greater than 50.
}
```

**c. Destructuring and Matching on Object Properties**

You can "destructure" an object to match against its internal fields. This is incredibly powerful for working with custom classes.

```dart
// A simple class representing a point on a grid.
class Point {
  final int x, y;
  Point(this.x, this.y);
}

void main() {
  var point = Point(2, 0);

  var description = switch (point) {
    // Match a Point at the origin.
    Point(x: 0, y: 0) => 'The point is at the origin.',
    // Match any Point on the x-axis.
    Point(x: var x, y: 0) => 'On the x-axis at x = $x.',
    // Match any Point on the y-axis.
    Point(x: 0, y: var y) => 'On the y-axis at y = $y.',
    // Match any other Point.
    Point(x: var x, y: var y) => 'At coordinates ($x, $y).',
    // The default case is required if not all possibilities are covered.
    _ => 'Not a point.'
  };
  
  print(description); // Output: On the x-axis at x = 2.
}
```

Pattern matching makes your code more declarative and expressive, allowing you to describe *what* you're looking for rather than writing a series of imperative checks.

[⬅ Previous](topic-3-2-loops.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-1-lists.md)

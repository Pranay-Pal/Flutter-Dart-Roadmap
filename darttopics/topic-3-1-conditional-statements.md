# Topic 3.1: Conditional Statements

[⬅ Previous](topic-2-3-operators.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-3-2-loops.md)

**Conditional statements** allow your program to make decisions. They control which pieces of code are executed based on whether a condition is `true` or `false`. This is the foundation of creating dynamic and responsive applications.

---

### 1. The `if` Statement

The `if` statement is the simplest form of conditional logic. It executes a block of code **only if** a specified condition is true.

**Syntax:**
```dart
if (condition) {
  // This code runs if the condition is true.
}
```

**Example:**
Let's check if a number is positive.

```dart
void main() {
  int number = 10;

  // The condition `number > 0` evaluates to true.
  if (number > 0) {
    print('The number is positive.'); // This line will be executed.
  }

  print('This line runs regardless of the condition.');
}
```

---

### 2. The `if-else` Statement

The `if-else` statement provides an alternative path. If the condition is `true`, the `if` block runs. If it's `false`, the `else` block runs.

**Syntax:**
```dart
if (condition) {
  // This code runs if the condition is true.
} else {
  // This code runs if the condition is false.
}
```

**Example:**
Let's determine if a user is old enough to vote.

```dart
void main() {
  int age = 17;

  // The condition `age >= 18` is false.
  if (age >= 18) {
    print('You are eligible to vote.');
  } else {
    // So, the `else` block is executed.
    print('You are not eligible to vote yet.');
  }
}
```

---

### 3. The `if-else if-else` Chain

When you have multiple conditions to check, you can chain them together using `else if`. Dart checks each condition in order until it finds one that is `true`. Once a true condition is found, its block is executed, and the rest of the chain is skipped.

**Syntax:**
```dart
if (condition1) {
  // Runs if condition1 is true.
} else if (condition2) {
  // Runs if condition1 is false and condition2 is true.
} else {
  // Runs if all previous conditions are false.
}
```

**Example:**
Let's assign a letter grade based on a score.

```dart
void main() {
  int score = 85;

  if (score >= 90) {
    print('Grade: A');
  } else if (score >= 80) {
    // `score >= 90` was false, but `score >= 80` is true.
    print('Grade: B'); // This block runs.
  } else if (score >= 70) {
    print('Grade: C');
  } else {
    print('Grade: F');
  }
}
```

---

### 4. The Ternary Operator

The ternary operator (`? :`) is a concise, one-line alternative to a simple `if-else` statement. It's perfect for assigning a value to a variable based on a condition.

**Syntax:**
```dart
variable = condition ? valueIfTrue : valueIfFalse;
```

**Example:**
Let's rewrite the voting age check using a ternary operator.

```dart
void main() {
  int age = 20;

  // If `age >= 18` is true, assign 'Adult'. Otherwise, assign 'Minor'.
  String status = age >= 18 ? 'Adult' : 'Minor';

  print('The user is an $status.'); // Output: The user is an Adult.
}
```

While you can nest ternary operators, it often makes the code harder to read. For complex logic, a full `if-else if` chain is usually a better choice.

---

### Best Practices

- **Always use curly braces `{}`**: Even for single-line `if` statements, using braces makes your code less ambiguous and prevents bugs when you add more lines later.
- **Avoid deep nesting**: If you find yourself nesting `if` statements more than two or three levels deep, consider restructuring your code. You might be able to use logical operators (`&&`, `||`) or break the logic into separate functions.

[⬅ Previous](topic-2-3-operators.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-3-2-loops.md)

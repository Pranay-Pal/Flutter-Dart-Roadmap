# Topic 9.2: Practical Null Safety

[⬅ Previous](topic-9-1-understanding-the-system.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-10-1-records-patterns.md)

Now that you understand the difference between nullable and non-nullable types, let's explore the practical tools Dart gives you to work with nullable values safely and conveniently.

This topic covers the special operators for handling null, as well as the `late` keyword for variables that are initialized after they are declared.

---

### 1. Null-Aware Operators

Instead of writing `if (variable != null)` every time, Dart provides a set of handy operators to make your code more concise.

#### a. The Null-Aware Access Operator (`?.`)

This operator lets you access a property or method on an object *only if* that object is not `null`. If the object is `null`, the expression short-circuits and evaluates to `null` instead of throwing an error.

```dart
void main() {
  String? text = 'Hello';
  String? nullText;

  // Without ?., this would throw an error if text were null.
  print(text?.length);    // Output: 5

  // With a null value, it simply returns null instead of crashing.
  print(nullText?.length); // Output: null
}
```

#### b. The Null-Coalescing Operator (`??`)

This operator provides a default value to use if a variable is `null`. It reads as "if the value on the left is `null`, then use the value on the right."

```dart
void main() {
  String? name;
  
  // If 'name' is null, use 'Guest' as the default.
  String displayName = name ?? 'Guest';
  print(displayName); // Output: Guest

  name = 'Alice';
  displayName = name ?? 'Guest';
  print(displayName); // Output: Alice
}
```

#### c. The Null-Aware Assignment Operator (`??=`)

This operator assigns a value to a variable *only if* that variable is currently `null`.

```dart
void main() {
  int? score;

  // Since score is null, 100 is assigned to it.
  score ??= 100;
  print(score); // Output: 100

  // Now score is not null, so the assignment is skipped.
  score ??= 0;
  print(score); // Output: 100
}
```

---

### 2. The Null Assertion Operator (`!`) - Use with Caution!

The `!` operator, also called the "bang operator," is your way of telling the compiler, "I am 100% certain that this nullable variable is not `null` right now, so treat it as a non-nullable type."

This is a dangerous tool because it **overrides the compiler's safety checks**. If you are wrong and the variable *is* `null` at runtime, your app will throw an exception and crash.

**When should you use it?**
Only use `!` when you have special knowledge that the compiler can't figure out on its own. This is rare.

```dart
String? getFirstEvenNumber(List<String?> numbers) {
  // (Imagine a function that finds the first even number string)
  return '2'; // Simplified for example
}

void main() {
  List<String?> values = ['1', '2', '3'];

  // A common but risky pattern:
  if (values.isNotEmpty) {
    // In this specific case, we know getFirstEvenNumber will find '2'.
    // The compiler doesn't know this, so we use '!' to force it.
    String firstEven = getFirstEvenNumber(values)!;
    print('The first even number is $firstEven.');
  }
}
```
> **Rule of Thumb:** Always try to use a null check (`if`), `?.`, or `??` before resorting to `!`. Think of `!` as a last resort.

---

### 3. The `late` Keyword: A Promise to Initialize

Sometimes, you have a non-nullable variable that you can't initialize right away when it's declared, but you guarantee you will give it a value before you use it. This is common in classes.

The `late` keyword is a promise to the compiler: "Trust me, I will assign a value to this before it's ever read."

If you break that promise and try to read the `late` variable before initializing it, you will get a `LateInitializationError` at runtime.

```dart
class UserProfile {
  // We declare 'userName' as non-nullable, but we don't have the value yet.
  // 'late' tells the compiler we'll handle it.
  late String userName;

  // The value is fetched and assigned later.
  void fetchUserName() {
    print('Fetching user name...');
    userName = 'Alice'; // The promise is fulfilled here.
  }

  void displayGreeting() {
    // If fetchUserName() hasn't been called yet, this line will crash.
    print('Welcome, $userName!');
  }
}

void main() {
  var profile = UserProfile();
  
  // profile.displayGreeting(); // This would cause a LateInitializationError!

  profile.fetchUserName(); // Initialize the 'late' variable.
  profile.displayGreeting(); // Now it's safe to use. Output: Welcome, Alice!
}
```

The `late` keyword is also used for lazy initialization, where a variable's initial value is only computed the first time it's accessed, but that is a more advanced use case. For now, think of it as a tool for handling non-nullable instance variables that are set after creation.

[⬅ Previous](topic-9-1-understanding-the-system.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-10-1-records-patterns.md)

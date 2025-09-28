# Topic 9.1: Understanding Null Safety

[⬅ Previous](topic-8-2-advanced-asynchrony.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-9-2-practical-application.md)

One of the most common sources of bugs in many programming languages is the infamous "null pointer error" (or `NullPointerException`). This happens when your code tries to use a variable that it expects to contain an object, but the variable is actually `null`.

Dart's **sound null safety** is a powerful feature designed to eliminate these errors entirely. It does this by changing the default behavior of types and forcing you to handle the possibility of `null` values before your code can even run.

This topic introduces the core principles of this system.

---

### 1. The Core Principle: Non-Nullable by Default

In a null-safe language like modern Dart, the fundamental rule is:

**Variables cannot hold the value `null` unless you explicitly say they can.**

This is a complete reversal from many other languages. By default, every variable you declare must have a value.

```dart
void main() {
  // This is a non-nullable String. It MUST hold a String value.
  String greeting = 'Hello, Dart!';

  // You cannot assign null to it.
  // greeting = null; // COMPILE-TIME ERROR!

  // You also cannot declare it without initializing it and then try to use it.
  int age;
  // print(age); // COMPILE-TIME ERROR: Non-nullable variable 'age' must be assigned before it can be used.
}
```
This "non-nullable by default" behavior is the foundation of null safety. The compiler can now make a powerful guarantee: if a variable's type doesn't have a `?` on it, it will **never** be `null`.

---

### 2. Nullable Types: Opting in to `null` with `?`

Of course, sometimes a variable *needs* to be able to hold `null`. For example, a `middleName` property for a user might be optional.

To declare that a variable is allowed to be `null`, you append a question mark (`?`) to its type.

```dart
void main() {
  // This is a nullable String. It can hold a String or it can be null.
  String? middleName = 'Christopher';
  print('Middle Name: $middleName'); // Output: Middle Name: Christopher

  // It's perfectly valid to assign null to it.
  middleName = null;
  print('Middle Name: $middleName'); // Output: Middle Name: null
}
```

Now that we have two kinds of types (nullable and non-nullable), the compiler enforces a critical rule: you cannot use a nullable variable as if it were non-nullable without first checking if it's `null`.

```dart
void printNameLength(String name) {
  // This function requires a NON-nullable String.
  print(name.length);
}

void main() {
  String? maybeName = 'Alice';

  // printNameLength(maybeName); // COMPILE-TIME ERROR!
  // The compiler stops you because `maybeName` *could* be null,
  // and the `printNameLength` function isn't prepared to handle that.
}
```

---

### 3. Working with Nullable Types: Flow Analysis

So how do you use a nullable variable? You have to prove to the compiler that it's not `null` at the moment you want to use it. The simplest way is with a standard `if` check.

Dart's compiler uses a sophisticated process called **flow analysis**. When you perform a null check, the compiler is smart enough to "promote" the variable to a non-nullable type within that branch of your code.

```dart
void main() {
  String? maybeName; // Let's start with it being null.

  // This is the most common way to handle a nullable value.
  if (maybeName != null) {
    // Inside this 'if' block, the compiler KNOWS `maybeName` is not null.
    // It is temporarily treated as a non-nullable 'String'.
    print('The name has ${maybeName.length} letters.');
  } else {
    print('There is no name.');
  }
  
  // Let's give it a value.
  maybeName = 'Bob';

  if (maybeName != null) {
    // Now this block will execute.
    print('The name has ${maybeName.length} letters.'); // Output: The name has 3 letters.
  }
}
```

This system of non-nullable defaults, opt-in nullability, and smart flow analysis allows you to write code that is incredibly safe from null errors, with the compiler acting as your safety net. The next topic will cover the special operators (`?.`, `??`, `!`) that make working with these types even more convenient.

[⬅ Previous](topic-8-2-advanced-asynchrony.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-9-2-practical-application.md)

# Topic 2.1: Variables & Data

[⬅ Previous](topic-1-2-your-first-program.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-2-2-built-in-data-types.md)

In programming, a **variable** is a named container that holds a value. Think of it like a labeled box: you give the box a name (the variable name) and put something inside it (the value). You can then refer to the box by its label to get the value back.

Dart is a **type-safe** language. This means that once you declare a variable to hold a certain type of data (like a number or text), Dart guarantees it will *only* hold that type of data, which helps prevent many common bugs.

This topic covers the keywords you'll use to declare and manage variables: `var`, `final`, `const`, and `late`.

---

### 1. Declaring Variables: `var`, `final`, and `const`

These three keywords are the most common ways to declare a variable.

#### `var` - For Mutable (Changeable) Variables

Use `var` when you expect the variable's value to change. Dart infers the type based on the initial value.

```dart
void main() {
  // `var` declares a variable whose value can be changed.
  // Dart infers that `score` is an `int` because it's first assigned `100`.
  var score = 100;
  print('Initial score: $score'); // Output: Initial score: 100

  // Since `score` is a `var`, we can reassign it a new value.
  score = 150;
  print('Updated score: $score'); // Output: Updated score: 150

  // However, you cannot assign a value of a different type.
  // score = 'a lot'; // This would cause a compile-time error.
}
```

#### `final` - For Single-Assignment Variables

Use `final` when you have a variable that will be assigned a value only once. The value can be determined at **runtime** (when the program is running).

```dart
void main() {
  // `final` declares a variable that cannot be reassigned after its first assignment.
  // Useful for values you don't want to change, like a user's ID.
  final String userId = 'uid-12345';

  // The value of a `final` variable can be computed at runtime.
  // For example, `DateTime.now()` gets the current time when this line is executed.
  final DateTime loginTime = DateTime.now();

  print('User $userId logged in at $loginTime');

  // Any attempt to reassign a `final` variable will result in an error.
  // userId = 'uid-67890'; // Error: Can't assign to a final variable.
}
```

#### `const` - For Compile-Time Constants

Use `const` for variables whose values are known **before the program even runs**. These are called compile-time constants. They are deeply, immutably constant.

```dart
void main() {
  // `const` is used for values that are fixed and known at compile time.
  // Think of mathematical constants or application-wide settings.
  const double pi = 3.14159;
  const String appName = 'My Awesome App';

  print('The value of pi is approximately $pi.');
  print('Welcome to $appName!');

  // The value of a `const` variable MUST be known when the code is compiled.
  // The following line would cause an error because `DateTime.now()` can only be
  // known when the program is running.
  // const DateTime compileTime = DateTime.now(); // Error!
}
```

#### Key Differences: `var` vs. `final` vs. `const`

| Keyword | Mutable?          | Value Known At...      | Use Case Example                                  |
| :------ | :---------------- | :--------------------- | :------------------------------------------------ |
| `var`   | ✅ Yes            | Runtime                | A user's score that changes during a game.        |
| `final` | ❌ No (re-assign) | Runtime                | The time a user logs in, fetched from a server.   |
| `const` | ❌ No             | **Compile-Time**       | The value of Pi (`3.14...`) or a color hex code.  |

> **Best Practice:** Prefer `const` for values that can be constant. If not, use `final`. Only use `var` if you know the value needs to change. This makes your code more predictable and less error-prone.

---

### 2. Lazy Initialization with `late`

The `late` keyword tells Dart that a non-nullable variable will be initialized **later**, but *before* it is first used. This is a promise: you are telling the compiler, "Trust me, this won't be null when it's accessed."

**When to use `late`:**
1.  **Splitting declaration and initialization:** When you can't initialize a variable in its declaration line.
2.  **Lazy-loading a value:** To postpone an expensive calculation until the value is actually needed.

#### Basic `late` Example

```dart
// Declare a non-nullable variable without an initial value.
late String description;

void main() {
  print('Program started.');

  // If you were to access `description` here, you'd get a runtime error.
  // print(description); // Error: Late variable has not been initialized.

  // Assign a value to the `late` variable before using it.
  description = 'A variable initialized after its declaration.';

  // Now it's safe to use.
  print(description);
}
```

#### `late final` for Lazy Initialization

You can combine `late` with `final` to create a variable that is both lazily initialized and single-assignment.

```dart
// This function simulates an expensive operation, like reading a file or a network call.
String loadDataFromDatabase() {
  print('Connecting to database and loading data...');
  return 'Some Important Data';
}

// The value is fetched only when `userData` is first accessed.
late final String userData = loadDataFromDatabase();

void main() {
  print('User has opened the app.');
  // At this point, `loadDataFromDatabase()` has NOT been called yet.

  print('User is about to view their profile...');
  // The function is called now, at the moment of first access.
  print('User data: $userData');

  // If accessed again, the function is NOT called again. The value is cached.
  print('User data (cached): $userData');
}
```

[⬅ Previous](topic-1-2-your-first-program.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-2-2-built-in-data-types.md)

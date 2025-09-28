# Topic 10.1: Records & Patterns

[⬅ Previous](topic-9-2-practical-application.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-10-2-error-handling.md)

**Records** and **patterns** are powerful, modern Dart features that revolutionize how you structure and work with data. They allow you to write more expressive, concise, and type-safe code.

- **Records**: An anonymous, immutable aggregate type that lets you bundle multiple objects into a single, lightweight object. Think of them as temporary, on-the-fly data structures without the boilerplate of a class.
- **Patterns**: A syntactic tool for destructuring data. Patterns allow you to unpack values from records, collections, and objects, binding them to local variables.

This topic will guide you through using records and patterns to write cleaner and more maintainable Dart code.

- [x] Returning multiple values with Records
- [x] Destructuring data with patterns
- [x] Advanced logic with pattern matching (`switch`, `if-case`)

---

### 1. Returning Multiple Values with Records

Before records, returning multiple values from a function required defining a class or using a `Map`, which was often verbose or lacked type safety. **Records** provide a lightweight and type-safe solution.

A record is created with a comma-delimited list of expressions in parentheses.

**Key Features**:
- **Anonymous & Immutable**: Records don't have a name and their fields cannot be reassigned after creation.
- **Positional and Named Fields**: Fields can be positional (accessed by index like `$1`, `$2`) or named (accessed by name like `record.name`).
- **Type-Safe**: The types of the fields are checked at compile time.

**Example: Defining and Using Records**

This function returns a record containing a user's name and age.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=3a1e8b3c1e3d3b3a3b3a3b3a3b3a3b3a)
```dart
// This function returns a record with two positional fields.
(String, int) getUserProfile() {
  return ('Alice', 30);
}

// This function returns a record with two named fields.
({String name, bool isAdmin}) getPermissions() {
  return (name: 'Bob', isAdmin: true);
}

void main() {
  // 1. Destructuring a positional record into variables.
  final (name, age) = getUserProfile();
  print('User: $name, Age: $age'); // Output: User: Alice, Age: 30

  // 2. Accessing positional fields directly by index.
  final userRecord = getUserProfile();
  print('User: ${userRecord.$1}, Age: ${userRecord.$2}'); // Output: User: Alice, Age: 30

  // 3. Accessing named fields directly by name.
  final permissions = getPermissions();
  print('Permissions for ${permissions.name}: Admin = ${permissions.isAdmin}');
  // Output: Permissions for Bob: Admin = true
}
```

---

### 2. Destructuring Data with Patterns

**Patterns** allow you to **destructure** complex data structures into individual variables. This makes code cleaner and more readable by avoiding manual indexing or property access.

Patterns can be used in variable declarations, `for-in` loops, `if-case`, and `switch-case`.

**Example: Destructuring Lists, Maps, and Objects**

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b)
```dart
class User {
  final String name;
  final String role;
  User(this.name, this.role);
}

void main() {
  // List destructuring
  final list = [1, 2, 3];
  final [a, b, c] = list;
  print('List: a=$a, b=$b, c=$c'); // Output: List: a=1, b=2, c=3

  // Map destructuring
  final map = {'first': 'apple', 'second': 'banana'};
  final {'first': fruit1, 'second': fruit2} = map;
  print('Map: Fruit 1=$fruit1, Fruit 2=$fruit2'); // Output: Map: Fruit 1=apple, Fruit 2=banana

  // Object destructuring
  final user = User('Charlie', 'Engineer');
  final User(name: userName, role: userRole) = user;
  print('Object: Name=$userName, Role=$userRole'); // Output: Object: Name=Charlie, Role=Engineer

  // Destructuring in a for-in loop
  final users = [
    {'name': 'Alice', 'role': 'Admin'},
    {'name': 'Bob', 'role': 'User'}
  ];

  for (final {'name': name, 'role': role} in users) {
    print('$name is a $role');
  }
}
```

---

### 3. Advanced Logic with Pattern Matching

Pattern matching supercharges control flow statements like `switch` and `if`, allowing you to branch logic based on the *shape* of the data, not just its value.

**`switch` Expressions**

Modern `switch` expressions are exhaustive (meaning you must handle all possible cases) and can return a value, making them powerful for conditional logic.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=5c5c5c5c5c5c5c5c5c5c5c5c5c5c5c5c)
```dart
String getDayType(DateTime date) {
  return switch (date.weekday) {
    1 => 'Monday Blues',
    6 || 7 => 'Weekend!', // Combine cases with ||
    _ => 'Another day', // `_` is the wildcard/default case
  };
}

// Using a `when` guard clause in a switch
String checkNumber(int n) {
  return switch (n) {
    0 => 'Zero',
    > 0 when n % 2 == 0 => 'Positive Even',
    > 0 => 'Positive Odd',
    < 0 => 'Negative',
  };
}

void main() {
  print(getDayType(DateTime.now()));
  print(checkNumber(10)); // Output: Positive Even
  print(checkNumber(7));  // Output: Positive Odd
}
```

**`if-case` for Destructuring and Guard Clauses**

The `if-case` statement lets you match a pattern and, optionally, add a `when` clause (a guard) to further refine the logic. It's perfect for handling one-off cases without a full `switch`.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=6d6d6d6d6d6d6d6d6d6d6d6d6d6d6d6d)
```dart
void processMessage(Object message) {
  // Match a list with exactly two items, where the first is 'login'.
  if (message case ['login', String username]) {
    print('User $username is logging in.');
  }
  // Match a map and use a `when` clause to check a condition.
  else if (message case {'code': int code} when code >= 500) {
    print('Server error detected: $code');
  }
  // Match a record
  else if (message case (String action, String target)) {
    print('Action: $action on $target');
  }
  else {
    print('Unknown message format.');
  }
}

void main() {
  processMessage(['login', 'Alice']);
  processMessage({'code': 503, 'details': 'Service unavailable'});
  processMessage(('delete', 'file.txt'));
  processMessage('hello');
}
```

---

### Putting It All Together: A Practical Example

Let's combine these features to process a list of server responses. Each response is a record containing a status code and a message.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=7e7e7e7e7e7e7e7e7e7e7e7e7e7e7e7e)
```dart
// A type alias for our response record for better readability.
typedef ServerResponse = (int code, String message);

void processResponses(List<ServerResponse> responses) {
  for (final response in responses) {
    // Use a switch expression with patterns to handle each response.
    final summary = switch (response) {
      // Match a record with a specific code.
      (200, _) => 'Success: ${response.$2}',

      // Match a record with a code in a range and a `when` guard.
      (int code, String msg) when code >= 400 && code < 500 =>
        'Client Error ($code): $msg',

      // Match any other record.
      (int code, String msg) => 'Other ($code): $msg',
    };
    print(summary);
  }
}

void main() {
  final responses = <ServerResponse>[
    (200, 'OK'),
    (404, 'Not Found'),
    (503, 'Service Unavailable'),
    (302, 'Redirect'),
  ];

  processResponses(responses);
}
```
This example demonstrates how records and patterns lead to code that is not only more concise but also more descriptive of its intent.

[⬅ Previous](topic-9-2-practical-application.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-10-2-error-handling.md)

# Topic 5.2: Mastering Function Parameters in Dart

[⬅ Previous](topic-5-1-function-basics.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-5-3-advanced-function-concepts.md)

A key aspect of designing robust and easy-to-use functions is understanding how to define their parameters. Dart offers a flexible system that lets you control which arguments are required, which are optional, and whether they should be passed by their position or by their name.

This guide breaks down the different parameter types and provides best practices for when to use each.

---

### 1. Positional Parameters

Positional parameters are the most basic type. Their meaning is determined by their order in the function definition.

#### a. Required Positional Parameters
These are the standard parameters you've likely seen before. They are mandatory, and the order in which you pass the arguments matters.

**Syntax:** `returnType functionName(type param1, type param2)`

```dart
// This function requires two integers, in a specific order.
int subtract(int a, int b) {
  return a - b;
}

void main() {
  // The first argument corresponds to 'a', the second to 'b'.
  print(subtract(10, 4)); // Output: 6

  // Swapping the order changes the result.
  print(subtract(4, 10)); // Output: -6
}
```

#### b. Optional Positional Parameters
You can make positional parameters optional by wrapping them in square brackets `[]`. They must be placed after all required positional parameters.

Since they are optional, you must either make them nullable (e.g., `int?`) or provide a default value.

**Syntax:** `returnType functionName(type requiredParam, [type? optionalParam, type optionalParamWithDefault = defaultValue])`

```dart
// 'profession' is an optional positional parameter with no default.
// 'country' is an optional positional parameter with a default value.
void displayUser(String name, [String? profession, String country = 'USA']) {
  String message = '$name from $country';
  if (profession != null) {
    message += ' is a $profession';
  }
  print(message);
}

void main() {
  displayUser('Alice'); // Output: Alice from USA
  displayUser('Bob', 'Developer'); // Output: Bob from USA is a Developer
  displayUser('Charlie', 'Artist', 'Canada'); // Output: Charlie from Canada is a Artist
}
```

---

### 2. Named Parameters

Named parameters are defined using curly braces `{}`. They are identified by their name, so their order doesn't matter. This makes function calls much more readable, especially for functions with many parameters or boolean flags.

#### a. Optional Named Parameters (Default Behavior)
By default, named parameters are optional. Like optional positional parameters, you must either make them nullable or provide a default value.

**Syntax:** `returnType functionName({type? param1, type param2 = defaultValue})`

```dart
// Both 'isUrgent' and 'sender' are optional named parameters.
void showAlert({String? sender, bool isUrgent = false}) {
  String alert = isUrgent ? 'URGENT:' : 'Info:';
  if (sender != null) {
    alert += ' From $sender';
  }
  print(alert);
}

void main() {
  showAlert(); // Output: Info:
  showAlert(isUrgent: true); // Output: URGENT:
  showAlert(sender: 'System'); // Output: Info: From System
  
  // The order does not matter.
  showAlert(isUrgent: true, sender: 'Security'); // Output: URGENT: From Security
}
```

#### b. Required Named Parameters
Sometimes, you want the readability of named parameters but still want to enforce that an argument is provided. You can achieve this by using the `required` keyword.

**Syntax:** `returnType functionName({required type param1, type? param2})`

```dart
// 'name' and 'email' are mandatory, but their order doesn't matter.
// 'role' is optional.
void createUser({required String name, required String email, String role = 'guest'}) {
  print('Creating user...');
  print('  Name: $name');
  print('  Email: $email');
  print('  Role: $role');
}

void main() {
  createUser(name: 'Alice', email: 'alice@example.com');
  // Output:
  // Creating user...
  //   Name: Alice
  //   Email: alice@example.com
  //   Role: guest

  createUser(email: 'bob@example.com', name: 'Bob', role: 'admin');
  // Output:
  // Creating user...
  //   Name: Bob
  //   Email: bob@example.com
  //   Role: admin

  // createUser(name: 'Charlie'); // This would cause a compile-time error because 'email' is missing.
}
```

---

### 3. Combining Parameter Types

You can mix parameter types, but you must follow one rule: **positional parameters always come first, followed by named or optional positional parameters.** You cannot have both optional positional and named parameters in the same function.

```dart
// A real-world example for making a network request.
void makeApiCall(String endpoint, {String method = 'GET', bool requiresAuth = false}) {
  print('Calling $endpoint...');
  print('  Method: $method');
  print('  Authentication: ${requiresAuth ? 'Yes' : 'No'}');
}

void main() {
  // 'endpoint' is required and positional.
  // 'method' and 'requiresAuth' are named and optional.
  makeApiCall('/users');
  makeApiCall('/products', method: 'POST', requiresAuth: true);
}
```

---

### Best Practices: Which Parameter Type to Choose?

| Parameter Type | When to Use | Example |
| :--- | :--- | :--- |
| **Required Positional** | For 1-3 essential parameters where the order is logical and unambiguous. | `subtract(10, 5)` |
| **Optional Positional** | When you have a few optional parameters where the order is still clear. Less common than named. | `displayUser('Alice', 'Admin')` |
| **Named (`required`)** | For mandatory parameters where the name adds critical context and improves readability. | `createUser({required name, required email})` |
| **Named (Optional)** | Ideal for configuration, settings, or boolean flags. Perfect for functions with many parameters. | `showAlert({isUrgent: true})` |

> **Pro Tip:** Favor named parameters over optional positional parameters for any function with more than two optional arguments. The clarity they provide is invaluable for code maintenance.

[⬅ Previous](topic-5-1-function-basics.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-5-3-advanced-function-concepts.md)

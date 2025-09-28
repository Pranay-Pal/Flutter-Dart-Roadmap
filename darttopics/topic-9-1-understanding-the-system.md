# Topic 9.1: Understanding the System

[⬅ Previous](topic-8-2-advanced-asynchrony.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-9-2-practical-application.md)

## Overview
- [ ] The principle of sound null safety
- [ ] Nullable (`?`) vs. non-nullable types

## The Principle of Sound Null Safety

Dart's null safety prevents null reference errors at compile time, making your code more reliable and easier to debug.

**Key Concepts:**
- **Non-nullable by default**: Variables cannot be null unless explicitly declared as nullable
- **Compile-time guarantees**: The compiler ensures you can't access potentially null values
- **Sound null safety**: If a type says it's non-nullable, it's guaranteed to never be null

```dart
String formatUsername(String username) {
  // username is non-nullable, so callers must provide a value.
  return username.trim().toLowerCase();
}

String? tryFindUserEmail(Map<String, String?> users, String username) {
  // Return type is nullable because a user might not have an email stored.
  return users[username];
}

void main() {
  final name = formatUsername('  Alice ');
  final users = {'alice': 'alice@example.com', 'bob': null};

  final email = tryFindUserEmail(users, name);

  if (email == null) {
    print('No email for $name, prompting user to add one.');
    return;
  }

  // Inside this block Dart promotes `email` to non-nullable, so it is safe to use.
  print('Sending confirmation to ${email.toLowerCase()}');
}
```

## Nullable vs. Non-nullable Types

**Non-nullable types (default):**
```dart
String name = 'Alice';  // Cannot be null
int age = 25;           // Cannot be null
List<String> items = []; // Cannot be null
```

**Nullable types (with `?`):**
```dart
String? optionalName;      // Can be null
int? optionalAge;          // Can be null
List<String>? optionalList; // Can be null

// You must handle null cases
if (optionalName != null) {
  print('Name: $optionalName');
}

// Null-aware access keeps code concise
print('Length: ${optionalName?.length ?? 0}');
```

**Migration Benefits:**
- Catches null pointer exceptions at compile time
- Makes code more explicit about null handling
- Improves code reliability and maintainability

For practical examples and advanced usage, see [Topic 9.2: Practical Application](topic-9-2-practical-application.md).

### **Module 9: Null Safety**

[⬅ Previous](topic-8-2-advanced-asynchrony.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-9-2-practical-application.md)

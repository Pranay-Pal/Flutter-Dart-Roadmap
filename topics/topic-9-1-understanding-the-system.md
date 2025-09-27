# Topic 9.1: Understanding the System

## Overview
- [ ] The principle of sound null safety
- [ ] Nullable (`?`) vs. non-nullable types

## The Principle of Sound Null Safety

Dart's null safety prevents null reference errors at compile time, making your code more reliable and easier to debug.

**Key Concepts:**
- **Non-nullable by default**: Variables cannot be null unless explicitly declared as nullable
- **Compile-time guarantees**: The compiler ensures you can't access potentially null values
- **Sound null safety**: If a type says it's non-nullable, it's guaranteed to never be null

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
```

**Migration Benefits:**
- Catches null pointer exceptions at compile time
- Makes code more explicit about null handling
- Improves code reliability and maintainability

For practical examples and advanced usage, see [Topic 9.2: Practical Application](topic-9-2-practical-application.md).

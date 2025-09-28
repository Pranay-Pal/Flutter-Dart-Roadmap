# Topic 14.1: Writing Quality Code

[⬅ Previous](topic-13-10-third-party-packages.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-14-2-compilation-and-deployment.md)

Writing code that works is only the first step. Writing code that is clean, readable, maintainable, and efficient is the mark of a professional developer. High-quality code is easier to debug, extend, and onboard new team members to. The Dart team provides official guidance and tools to help you achieve this.

This topic covers the official "Effective Dart" guidelines and how to enforce them using the Dart static analyzer.

- [x] Following the "Effective Dart" style and usage guides
- [x] Enforcing code quality with `analysis_options.yaml`

---

### 1. Following "Effective Dart"

**Effective Dart** is the official guide to writing good Dart code. It's a collection of best practices covering **style**, **documentation**, **usage**, and **design**. Following these guidelines makes your code consistent with the wider Dart ecosystem, making it easier for others to read, use, and maintain.

You can read the full guide here: [https://dart.dev/effective-dart](https://dart.dev/effective-dart)

**Key Principles from Effective Dart**:

#### Style
Focuses on consistent formatting, naming, and structure. The `dart format` tool automates most of this.
- **Identifiers**:
  - `UpperCamelCase` for types (classes, enums, typedefs, extensions).
  - `lowerCamelCase` for variables, parameters, and function/method names.
  - `lowercase_with_underscores` for libraries, packages, and source file names.
- **Control Flow**: Always use curly braces `{}` for all control flow statements, even single-line ones.
- **Line Length**: Keep lines under 80 characters for better readability.

#### Documentation
How to write clear and useful comments that tools can parse.
- Use `///` for doc comments. This allows `dart doc` to generate beautiful HTML documentation for your API.
- Start doc comments with a single-sentence summary ending with a period.
- Use square brackets to refer to in-scope identifiers, like `[variableName]`.

#### Usage
Guidelines on how to use language features correctly and idiomatically.
- Use `?.` (null-aware access) and `??` (null-coalescing operator) instead of explicit `null` checks.
- Avoid using the null assertion operator `!` unless you are absolutely certain a value is not null.
- Prefer `final` for local variables and fields that are set once and never reassigned.

**Example: Applying Effective Dart Principles**

```dart
// GOOD: This code follows Effective Dart guidelines.

import 'dart:convert';

/// A service that fetches user profiles from a remote server.
class UserProfileService {
  /// The HTTP client used to make network requests.
  final HttpClient _client;

  /// Creates a new service with the given [HttpClient].
  UserProfileService({required HttpClient client}) : _client = client;

  /// Fetches the profile for a user with the given [userId].
  ///
  /// Throws a [UserNotFoundException] if the user does not exist.
  Future<User> fetchProfile(String userId) async {
    final url = Uri.parse('/users/$userId');
    final response = await _client.get(url);

    if (response.statusCode == 404) {
      throw UserNotFoundException(userId);
    }

    // Assumes User class has a factory constructor fromJson.
    return User.fromJson(jsonDecode(response.body));
  }
}

/// Represents a user of the application.
class User {
  final String id;
  final String name;

  User({required this.id, required this.name});

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] as String,
      name: json['name'] as String,
    );
  }
}

/// An exception thrown when a user profile cannot be found.
class UserNotFoundException implements Exception {
  final String userId;
  UserNotFoundException(this.userId);

  @override
  String toString() => 'UserNotFoundException: No user found with ID $userId.';
}
```

---

### 2. Enforcing Code Quality with `analysis_options.yaml`

The Dart analyzer is a powerful static analysis tool that can be configured to enforce strict rules and catch potential problems before you even run your code. You configure the analyzer by creating an `analysis_options.yaml` file in the root of your project.

**Key Sections**:
- **`include`**: You can include a set of recommended rules from a package. The two most common are:
  - `package:lints/recommended.yaml`: A curated set of rules that catch common errors and enforce good style. **This is a great starting point.**
  - `package:lints/core.yaml`: A smaller, core set of rules for critical issues.
  - For Flutter projects, `package:flutter_lints/flutter.yaml` is the default.
- **`linter`**: This is where you can enable or disable specific lint rules to customize your ruleset. You can find a full list of available lints at [https://dart.dev/tools/linter-rules](https://dart.dev/tools/linter-rules).
- **`analyzer`**: This section allows you to configure the analyzer itself, such as treating certain issues as errors, warnings, or ignoring them completely.

**Example `analysis_options.yaml`**

This configuration starts with the recommended set of lints and then adds a few more for even stricter code quality.

```yaml
# analysis_options.yaml

# Start with the recommended set of lints from the `lints` package.
# To use this, add `lints` to your dev_dependencies in pubspec.yaml.
include: package:lints/recommended.yaml

# Customize the linter rules.
linter:
  rules:
    # STYLE: Prefer single quotes for strings.
    - prefer_single_quotes

    # STYLE: Require type annotations for all public APIs to improve readability.
    - always_declare_return_types
    - always_specify_types

    # USAGE: Avoid using print() in production code, as it's easy to forget.
    # Use a proper logger instead.
    - avoid_print

    # DESIGN: Ensure all public members have doc comments.
    - public_member_api_docs

# You can also configure the severity of analyzer diagnostics.
analyzer:
  errors:
    # Treat the "avoid_print" lint as an error, failing the build on CI.
    avoid_print: error

    # Treat missing required parameters as a warning instead of an error.
    missing_required_param: warning
```

// ... existing code ...
By using a well-configured `analysis_options.yaml` file and running `dart analyze` regularly (or using the analyzer in your IDE), you and your team can ensure that all code adheres to the same high standard of quality automatically.

---

### 3. Using `assert` for Debugging and Preconditions

The `assert` statement is a powerful tool for validating conditions during development. An `assert` takes a condition and an optional message. If the condition is `false`, it throws an `AssertionError`.

**Key Characteristics:**
- **Development Only**: `assert` statements are only enabled in debug mode. They are completely ignored in production code, so they have **zero performance impact** on your released app.
- **Purpose**: Use `assert` to check for "impossible" situations or to enforce preconditions on a function's inputs. It helps you catch bugs early by failing fast.

**Syntax:** `assert(condition, [optionalMessage]);`

```dart
// A function to calculate the total price with a discount.
// The discount percentage must be between 0 and 1.
double calculateDiscountedPrice(double originalPrice, double discount) {
  // 1. Use assert to enforce a precondition.
  // This check only runs in debug mode.
  assert(
    discount >= 0 && discount <= 1,
    'Discount must be between 0.0 and 1.0',
  );

  final discountAmount = originalPrice * discount;
  return originalPrice - discountAmount;
}

void main() {
  // This call is valid and will work fine.
  final price1 = calculateDiscountedPrice(100.0, 0.2);
  print('Price 1: $price1'); // Output: Price 1: 80.0

  // This call violates the precondition.
  try {
    // In debug mode, this will throw an AssertionError.
    // In production mode, this would silently produce a wrong result (-100.0).
    final price2 = calculateDiscountedPrice(100.0, 2.0); 
    print('Price 2: $price2');
  } catch (e) {
    print('
Caught an error:');
    print(e); // Output: AssertionError: 'Discount must be between 0.0 and 1.0'
  }
}
```

**`assert` vs. `throw`**:
- Use `assert` for internal logic checks and programmer errors that should never happen in a correctly functioning program.
- Use `throw` for runtime errors that you expect might happen (e.g., invalid user input, network failure) and need to be handled gracefully with `try-catch` in production.

[⬅ Previous](topic-12-2-platform-interoperability.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-2-compilation-and-deployment.md)


[⬅ Previous](topic-12-2-platform-interoperability.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-2-compilation-and-deployment.md)

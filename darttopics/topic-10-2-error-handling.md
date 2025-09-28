# Topic 10.2: Error Handling

[⬅ Previous](topic-10-1-records-patterns.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-11-1-libraries-and-packages.md)

**Error handling** is a critical part of writing robust and reliable applications. No matter how well you write your code, unexpected situations can occur, such as invalid user input, network failures, or file system errors. Dart provides a comprehensive set of tools for managing these situations gracefully.

Dart's error handling mechanism is based on **exceptions**. When an error occurs, an exception is "thrown." If the exception is not "caught," it will propagate up the call stack, potentially causing your program to terminate unexpectedly.

This topic covers how to use `try`, `catch`, and `finally` to handle exceptions, how to `rethrow` them, and how to create your own custom exception types.

- [x] Understanding `Error` vs. `Exception`
- [x] Using `try`, `catch`, and `finally` to handle exceptions
- [x] Re-throwing exceptions with `rethrow`
- [x] Throwing exceptions with `throw`
- [x] Creating custom exceptions

---

### 1. Understanding `Error` vs. `Exception`

In Dart, all throwable objects inherit from the base class `Object`, but the two most important classes to understand are `Exception` and `Error`.

- **`Exception`**: Intended for expected, programmatic errors that a developer can anticipate and handle. Think of these as "user-facing" or environmental errors. Examples include `FormatException` (e.g., parsing an invalid number) or `IOException` (e.g., a file not found). You should almost always catch `Exception`s.

- **`Error`**: Intended for unexpected, programmatic failures that indicate a bug in your code. The program is likely in an unrecoverable state. Examples include `NullThrownError` or `ArgumentError`. You typically do not catch `Error`s; they are meant to signal a problem that should be fixed during development.

**Analogy**: An `Exception` is like a customer giving you the wrong credit card number—you can handle it and ask them to try again. An `Error` is like your cash register's software crashing—it's a fundamental problem you need to fix, not just handle.

---

### 2. Using `try`, `catch`, and `finally`

This is the fundamental structure for handling exceptions.

- **`try`**: You wrap the code that might throw an exception in a `try` block.
- **`catch`**: If an exception occurs in the `try` block, Dart looks for a matching `catch` block to handle it. You can have multiple `catch` blocks to handle different types of exceptions.
- **`finally`**: The `finally` block **always** executes, whether an exception was thrown or not. It is perfect for cleanup code, like closing a file or a network connection.

**Example: Basic Exception Handling**

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=8f8f8f8f8f8f8f8f8f8f8f8f8f8f8f8f)
```dart
void main() {
  try {
    // This code is likely to fail.
    final number = int.parse('abc');
    print('Parsed number: $number');
  } on FormatException catch (e) {
    // Use `on` to specify the type of exception to catch.
    print('Caught a FormatException: $e');
  } catch (e, s) {
    // The general `catch` block handles any other exception.
    // The second optional argument is the StackTrace object.
    print('Caught an unknown exception: $e');
    print('Stack trace:\n$s');
  } finally {
    // This code always runs, for cleanup.
    print('This is the finally block. It always executes.');
  }
}
```

---

### 3. Re-throwing Exceptions with `rethrow`

Sometimes you want to catch an exception, perform an action (like logging), and then let the exception continue propagating up the call stack. The `rethrow` keyword preserves the original stack trace, which is crucial for debugging.

**Example: Logging and Re-throwing**

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=9g9g9g9g9g9g9g9g9g9g9g9g9g9g9g)
```dart
void processData() {
  try {
    // Simulate a data processing error.
    throw FormatException('Invalid data format');
  } catch (e) {
    // Log the error before passing it on.
    print('Logging error: $e');
    rethrow; // The exception continues up the call stack.
  }
}

void main() {
  try {
    processData();
  } catch (e) {
    // The re-thrown exception is caught here.
    print('Main function caught: $e');
  }
}
```

---

### 4. Throwing Exceptions with `throw`

You can signal an error in your own code by **throwing** an exception. This is useful for enforcing preconditions or validating input. You can throw an instance of any class that implements `Exception`, or even built-in ones.

**Example: Validating Input**

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=ah0h0h0h0h0h0h0h0h0h0h0h0h0h0h0h)
```dart
void validateEmail(String email) {
  if (email.isEmpty) {
    // Use a built-in error type for invalid arguments.
    throw ArgumentError('Email cannot be empty.');
  }
  if (!email.contains('@')) {
    // Use a built-in exception for format issues.
    throw FormatException('Not a valid email address.');
  }
  print('Email is valid.');
}

void main() {
  try {
    validateEmail('');
  } catch (e) {
    print(e); // Output: Invalid argument(s): Email cannot be empty.
  }

  try {
    validateEmail('test.com');
  } catch (e) {
    print(e); // Output: FormatException: Not a valid email address.
  }
}
```

---

### 5. Creating Custom Exceptions

For application-specific errors, it's a best practice to create your own exception classes. This makes your error handling code more specific, readable, and easier to manage. To do this, simply create a class that implements the built-in `Exception` interface.

**Example: A Custom Exception for a Bank Account**

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=bibibibibibibibibibibibibibibibi)
```dart
// A custom exception for a specific domain error.
class InsufficientFundsException implements Exception {
  final double amount;
  final double balance;

  InsufficientFundsException(this.amount, this.balance);

  @override
  String toString() {
    final deficit = amount - balance;
    return 'InsufficientFundsException: Cannot withdraw \$$amount. You are short by \$$deficit.';
  }
}

class BankAccount {
  double _balance = 100.0;

  void withdraw(double amount) {
    if (amount <= 0) {
      throw ArgumentError('Withdrawal amount must be positive.');
    }
    if (amount > _balance) {
      // Throw our custom exception.
      throw InsufficientFundsException(amount, _balance);
    }
    _balance -= amount;
    print('Withdrew \$$amount. New balance: \$$_balance');
  }
}

void main() {
  final account = BankAccount();
  try {
    account.withdraw(50.0);  // This will succeed.
    account.withdraw(80.0);  // This will fail with our custom exception.
  } on InsufficientFundsException catch (e) {
    // Catching the specific custom exception.
    print('Error: $e');
  } catch (e) {
    // Catching any other potential errors.
    print('An unexpected error occurred: $e');
  }
}
```

By mastering these error handling techniques, you can build applications that are more predictable, debuggable, and user-friendly.

[⬅ Previous](topic-10-1-records-patterns.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-11-1-libraries-and-packages.md)

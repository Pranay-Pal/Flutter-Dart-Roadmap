# Topic 1.2: Your First Program

[⬅ Previous](topic-1-1-setup-tooling.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-2-1-variables-data.md)

    * [ ] The `main()` function: Dart's entry point
    * [ ] Writing to the console with `print()`
    * [ ] Understanding code comments (`//`, `/*...*/`, `///`)

#### The `main()` Function: Dart's Entry Point

Every Dart program must have a `main()` function, which is the entry point where program execution begins.

**Basic structure:**
```dart
void main() {
  // Your code goes here
}
```

**Alternative forms:**
```dart
// With command-line arguments
void main(List<String> arguments) {
  print('Arguments: $arguments');
}

// Async main function (for asynchronous operations)
Future<void> main() async {
  // Async code here
}
```

#### Writing to the Console with `print()`

The `print()` function outputs text to the console. It's your primary tool for displaying information and debugging.

**Basic usage:**
```dart
void main() {
  print('Hello, World!');
  print('Welcome to Dart programming!');
}
```

**Printing different data types:**
```dart
void main() {
  print(42);           // Numbers
  print(3.14);         // Decimals
  print(true);         // Booleans
  print('Hello');      // Strings
  print([1, 2, 3]);    // Lists
}
```

**String interpolation in print:**
```dart
void main() {
  String name = 'Alice';
  int age = 25;
  print('My name is $name and I am $age years old.');
  print('Next year I will be ${age + 1} years old.');
}
```

#### Understanding Code Comments

Comments are essential for documenting your code and making it readable for yourself and others.

**Single-line comments (`//`):**
```dart
void main() {
  // This is a single-line comment
  print('Hello, World!'); // Comment at the end of a line
}
```

**Multi-line comments (`/* */`):**
```dart
void main() {
  /*
   * This is a multi-line comment
   * It can span multiple lines
   * Useful for longer explanations
   */
  print('Hello, World!');
}
```

**Documentation comments (`///`):**
```dart
/// This function greets a person by name.
/// 
/// The [name] parameter specifies who to greet.
/// Returns a greeting message as a String.
void greetPerson(String name) {
  print('Hello, $name!');
}

void main() {
  greetPerson('Alice');
}
```

**Your First Complete Program:**
```dart
/// A simple Dart program that demonstrates basic concepts
void main() {
  // Print a welcome message
  print('=== My First Dart Program ===');
  
  // Declare and use variables
  String programmer = 'Alice';
  int experience = 2;
  
  /*
   * Display information about the programmer
   * using string interpolation
   */
  print('Programmer: $programmer');
  print('Years of experience: $experience');
  print('Future experience: ${experience + 1} years');
  
  // End the program
  print('Program completed successfully!');
}
```

**Expected Output:**
```
=== My First Dart Program ===
Programmer: Alice
Years of experience: 2
Future experience: 3 years
Program completed successfully!
```

[⬅ Previous](topic-1-1-setup-tooling.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-2-1-variables-data.md)

### **Module 2: Language Fundamentals**

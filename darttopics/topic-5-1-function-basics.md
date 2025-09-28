# Topic 5.1: Function Basics

[⬅ Previous](topic-4-4-collection-tools.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-5-2-parameter-types.md)

**Functions** are the primary building blocks of any Dart program. They are self-contained, reusable blocks of code designed to perform a single, specific task. By breaking down a complex problem into smaller, manageable functions, you can write code that is more organized, readable, and easier to debug.

This topic covers the fundamentals of defining and using functions in Dart, including how to pass data to them using **parameters** and how to get data back using **return values**.

---

### 1. The `main` Function: The Entry Point

Every Dart application must have a top-level `main()` function, which serves as the starting point for code execution.

```dart
void main() {
  print('Hello, Dart! This is where the program begins.');
}
```

---

### 2. Anatomy of a Function

A function consists of a few key parts:

```dart
// 1. Return Type
//    |
//    |   2. Function Name
//    |      |
//    |      |         3. Parameters
//    |      |            |
//   vvv   vvvvvvv   vvvvvvvvvvvvvvv
   int   addNumbers(int a, int b) {
//   ^--------------------------------- 4. Function Body
     return a + b; //------------------'
   }
//   ^---------------------------------'
```

1.  **Return Type**: The type of value the function will send back (e.g., `int`, `String`, `void`). `void` means the function does not return any value.
2.  **Function Name**: A unique name to identify the function (e.g., `addNumbers`).
3.  **Parameters**: A list of variables passed into the function, enclosed in parentheses `()`.
4.  **Function Body**: The block of code, enclosed in curly braces `{}`, that gets executed.

---

### 3. Defining and Calling a Simple Function

Here is a function that takes no input and returns no value.

```dart
// Defines a function named `sayHello`.
// The return type `void` means it doesn't return anything.
void sayHello() {
  print('Hello, world!');
}

void main() {
  // To execute the function, you "call" it by its name.
  sayHello();
}
```

---

### 4. Parameters: Passing Data to a Function

Parameters allow you to pass data *into* a function, making it more flexible and dynamic.

```dart
// This function accepts one parameter: a String named `name`.
void greet(String name) {
  print('Hello, $name! Welcome.');
}

void main() {
  greet('Alice'); // 'Alice' is the argument passed to the `name` parameter.
  greet('Bob');   // You can reuse the function with different arguments.
}
```

---

### 5. Return Values: Getting Data from a Function

Functions can process data and send a result back using the `return` keyword. The type of the returned value must match the function's declared **return type**.

```dart
// This function takes two integers and returns their sum as an integer.
int calculateSum(int x, int y) {
  int result = x + y;
  return result; // Sends the value of `result` back.
}

void main() {
  // Call the function and store its return value in a variable.
  int sum = calculateSum(5, 3);
  
  print('The sum is $sum.'); // Output: The sum is 8.
}
```

---

### 6. Arrow Syntax (`=>`): For Concise Functions

For functions that contain just a single expression, Dart provides a handy shorthand syntax using `=>` (the "arrow" or "fat arrow" syntax).

The expression to the right of the arrow is what the function returns.

**Standard Syntax:**
```dart
int multiply(int a, int b) {
  return a * b;
}
```

**Arrow Syntax:**
```dart
// The `=>` replaces the curly braces `{}` and the `return` keyword.
int multiply(int a, int b) => a * b;

void main() {
  var product = multiply(7, 6);
  print(product); // Output: 42
}
```
This makes your code cleaner and more readable for simple functions.

[⬅ Previous](topic-4-4-collection-tools.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-5-2-parameter-types.md)

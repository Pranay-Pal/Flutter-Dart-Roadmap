# Topic 5.3: Advanced Function Concepts in Dart

[⬅ Previous](topic-5-2-parameter-types.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-6-1-classes-and-objects.md)

Beyond the basics, Dart provides powerful features that enable more concise, flexible, and functional programming styles. Mastering these concepts will allow you to write elegant and efficient code, especially when working with collections and asynchronous operations.

This guide covers arrow syntax, anonymous functions (lambdas), lexical scope and closures, and how to create readable function aliases with `typedef`.

---

### 1. Arrow Syntax (`=>`)

For functions that contain only a single expression, you can use the arrow syntax (`=>`) as a shorthand. It's a clean way to write simple functions.

**Syntax:** `returnType functionName(params) => expression;`

The expression after the `=>` is evaluated, and its value is implicitly returned.

```dart
// Traditional function syntax
int add(int a, int b) {
  return a + b;
}

// Equivalent function with arrow syntax
int addArrow(int a, int b) => a + b;

void main() {
  print(add(5, 3));      // Output: 8
  print(addArrow(5, 3)); // Output: 8

  // Arrow syntax is great for simple, readable one-liners.
  bool isEven(int number) => number % 2 == 0;
  print('Is 10 even? ${isEven(10)}'); // Output: Is 10 even? true

  String greet(String name) => 'Hello, $name!';
  print(greet('Alice')); // Output: Hello, Alice!
}
```

> **Best Practice:** Use arrow syntax for simple functions where the logic is a single, clear expression. For functions with multiple lines, statements, or complex logic, stick to the standard block body `{}`.

---

### 2. Anonymous Functions (Lambdas)

An anonymous function, or lambda, is a function without a name. They are incredibly useful for "on-the-fly" operations, especially when passed as arguments to other functions.

**Syntax:** `(parameters) { statements; }` or `(parameters) => expression`

Anonymous functions are most commonly used with collection methods like `forEach`, `map`, and `where`.

```dart
void main() {
  List<String> names = ['Alice', 'Bob', 'Charlie'];

  // 1. Using with forEach to perform an action on each item.
  print('Greetings:');
  names.forEach((name) {
    print('  Hello, $name!');
  });

  // 2. Using with map to transform a list.
  // The anonymous function (n) => n.toUpperCase() is applied to each item.
  List<String> upperCaseNames = names.map((name) => name.toUpperCase()).toList();
  print('Uppercase Names: $upperCaseNames'); // Output: [ALICE, BOB, CHARLIE]

  // 3. Using with where to filter a list.
  // The anonymous function (n) => n.startsWith('A') returns true or false.
  List<String> namesStartingWithA = names.where((name) => name.startsWith('A')).toList();
  print('Names starting with A: $namesStartingWithA'); // Output: [Alice]
}
```

---

### 3. Lexical Scope and Closures

These two concepts are fundamental to how functions work in Dart.

#### a. Lexical Scope
Lexical scope means that a function's scope is determined by its location in the source code. A function can access variables in its own scope and in any outer (parent) scopes.

```dart
String topLevelMessage = 'I am at the top level.';

void main() {
  String mainMessage = 'I am inside main().';

  void innerFunction() {
    String innerMessage = 'I am inside innerFunction().';

    // This function can access variables from all parent scopes.
    print(innerMessage);
    print(mainMessage);
    print(topLevelMessage);
  }

  innerFunction();
}
```

#### b. Closures
A closure is a special function object that **"closes over" and remembers variables from the scope in which it was created**, even if that scope has already exited.

This allows you to create functions that maintain a state.

**Example: A Counter Factory**
The `createCounter` function returns another function. The returned function is a closure because it "remembers" the `count` variable from its creation environment.

```dart
// This function returns a function.
Function createCounter() {
  int count = 0; // This variable is "closed over" by the returned function.
  
  return () {
    count++;
    return count;
  };
}

void main() {
  // Each call to createCounter() creates a new, independent closure.
  Function counter1 = createCounter();
  Function counter2 = createCounter();

  print('Counter 1: ${counter1()}'); // Output: 1
  print('Counter 1: ${counter1()}'); // Output: 2
  
  print('Counter 2: ${counter2()}'); // Output: 1 (This is a separate counter)
  
  print('Counter 1: ${counter1()}'); // Output: 3
}
```

Closures are a powerful pattern for creating functions with persistent state, like factory functions or event handlers.

---

### 4. Function Type Aliases (`typedef`)

A `typedef` allows you to create a custom name for a function type. This is extremely useful for improving code readability when you have complex function signatures that are used in multiple places.

**Modern Syntax:** `typedef AliasName = ReturnType Function(Parameters);`

```dart
// Create an alias 'Calculator' for any function that takes two integers
// and returns an integer.
typedef Calculator = int Function(int a, int b);

// This function now takes a 'Calculator' instead of a generic 'Function'.
// This is much more readable and type-safe.
void performOperation(int x, int y, Calculator operation, String opName) {
  int result = operation(x, y);
  print('$opName($x, $y) = $result');
}

// Define some functions that match the Calculator signature.
int add(int a, int b) => a + b;
int multiply(int a, int b) => a * b;

void main() {
  performOperation(10, 5, add, 'Add');       // Output: Add(10, 5) = 15
  performOperation(10, 5, multiply, 'Multiply'); // Output: Multiply(10, 5) = 50

  // You can also use an anonymous function if it matches the signature.
  performOperation(10, 5, (a, b) => a - b, 'Subtract'); // Output: Subtract(10, 5) = 5
}
```

> **When to use `typedef`?**
> Use `typedef` when a function signature is complex or is reused as a parameter in multiple functions. For simple, one-off cases, it's often clearer to define the function type inline: `void myFunc(void Function(String) callback)`.

[⬅ Previous](topic-5-2-parameter-types.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-6-1-classes-and-objects.md)

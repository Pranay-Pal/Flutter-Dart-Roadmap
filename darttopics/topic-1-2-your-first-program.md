# Topic 1.2: Your First Program

[⬅ Previous](topic-1-1-setup-tooling.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-2-1-variables-data.md)

Now that your environment is set up, it's time to write your first Dart program. We'll cover the three most fundamental concepts: the `main()` function, printing to the console, and writing comments.

---

### How to Run the Examples in This Topic

As you follow along, you have a few options to execute the code snippets:

1.  **Using a Local File:**
    *   Create a new file in your project folder (e.g., `lib/my_program.dart`).
    *   Copy and paste a full code example (including `void main()`) into the file.
    *   Run it from your terminal with `dart run lib/my_program.dart`.

2.  **Using DartPad:**
    *   Open [DartPad](https://dartpad.dev), a free online editor.
    *   Paste the code into the editor and click **Run**. This is a great way to experiment without installing anything.

---

### 1. The `main()` Function: Dart's Entry Point

Every Dart application begins execution in the `main()` function. Think of it as the front door to your program—Dart always looks for `main()` to know where to start.

**What does `void main()` mean?**
-   `void`: This is a return type, which means the `main` function doesn't return any value when it finishes.
-   `main`: This is the required name for the entry point function.
-   `()`: The parentheses are where you can define parameters (inputs) for the function. For now, we'll leave them empty.

Here is the most basic structure of a Dart program:

```dart
// The main function, where our program begins.
void main() {
  // All of our Dart code will go inside these curly braces.
}
```

<details>
<summary><b>Advanced:</b> Other Forms of `main()`</summary>

As you advance, you may see `main()` written in other ways to handle more complex scenarios, such as accepting command-line arguments or performing asynchronous operations. You don't need these now, but it's good to know they exist.

```dart
// A main function that accepts command-line arguments.
void main(List<String> arguments) {
  print('Received arguments: $arguments');
}

// An async main function, used for programs that perform
// non-blocking operations, like fetching data from the internet.
Future<void> main() async {
  print('This is an async main function.');
}
```

</details>

---

### 2. Writing to the Console with `print()`

The `print()` function is a built-in tool for displaying output in the console. It is essential for seeing the results of your code and for debugging (finding and fixing issues).

```dart
void main() {
  // The text inside the parentheses will be printed to the console.
  print('Hello, World!');
  print('Welcome to the world of Dart!');
  
  // You can print more than just text.
  print(42);         // Prints the number 42
  print(true);       // Prints the boolean value true
}
```

#### Using String Interpolation

String interpolation is a powerful feature that lets you embed expressions and variables directly inside a string. Use a dollar sign (`) followed by the variable name, or `${}` for more complex expressions.

```dart
void main() {
  String name = 'Alex';
  int age = 30;

  // Use $variableName to insert a variable's value.
  print('My name is $name and I am $age years old.');

  // Use ${expression} for more complex logic, like calculations.
  print('Next year, I will be ${age + 1} years old.');
}
```

---

### 3. Understanding Code Comments

Comments are notes in your code that are ignored by the Dart compiler. They are crucial for explaining what your code does, both for your future self and for other developers.

**Single-Line Comments (`//`)**

Use `//` to write a comment that lasts for a single line.

```dart
void main() {
  // This is a single-line comment. It explains the next line of code.
  print('Hello from a commented program!'); // Comments can also go at the end of a line.
}
```

**Multi-Line Comments (`/* ... */`)**

Use `/*` and `*/` to write comments that span multiple lines.

```dart
void main() {
  /*
   * This is a multi-line comment.
   * It's useful for providing more detailed explanations about a
   * function, a block of code, or a complex algorithm.
   */
  print('This line is not commented out.');
}
```

**Documentation Comments (`///`)**

Use `///` to generate documentation for your functions, classes, or variables. These are special comments that development tools can use to provide information about your code.

```dart
/// A simple function that greets a user by their [name].
///
/// This is a documentation comment. It will appear in generated docs
/// and when a developer hovers over the function name in an IDE.
void greet(String name) {
  print('Hello, $name!');
}

void main() {
  greet('Maria');
}
```

---

### Your First Complete Program

Let's combine everything we've learned into a single, well-documented program.

```dart
// The program's entry point.
void main() {
  // 1. Print a header to the console to make the output easy to read.
  print('--- My First Dart Program ---');

  // 2. Declare a variable to hold a name.
  //    Variables store data that can be used and changed.
  String developerName = 'Chris';

  // 3. Use the variable in a print statement via string interpolation.
  print('Hello, my name is $developerName.');

  /*
   * 4. Perform a simple calculation and display the result.
   *    This demonstrates how expressions can be embedded in strings.
   */
  int currentYear = 2025;
  int birthYear = 1995;
  print('I am ${currentYear - birthYear} years old.');

  // 5. Print a closing message to signify the end of the program.
  print('--- Program finished ---');
}
```

**Expected Output:**

```text
--- My First Dart Program ---
Hello, my name is Chris.
I am 30 years old.
--- Program finished ---
```

[⬅ Previous](topic-1-1-setup-tooling.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-2-1-variables-data.md)

# Topic 3.2: Loops

[⬅ Previous](topic-3-1-conditional-statements.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-3-3-switch-and-case.md)

**Loops** are control flow structures that allow you to execute a block of code repeatedly. They are essential for iterating over collections of data, performing repetitive tasks, and implementing algorithms that require repeated steps.

---

### 1. The `for` Loop

The `for` loop is ideal when you know in advance how many times you want to execute a block of code. It consists of three parts: an initializer, a condition, and an updater.

**Syntax:**
```dart
for (initializer; condition; updater) {
  // Code to be executed in each iteration.
}
```

- **Initializer**: Executed once before the loop starts. Typically used to declare and initialize a counter variable (e.g., `int i = 0`).
- **Condition**: Evaluated before each iteration. If it's `true`, the loop continues. If it's `false`, the loop terminates.
- **Updater**: Executed at the end of each iteration. Typically used to increment or decrement the counter (e.g., `i++`).

**Example:**
Let's count from 1 to 5.

```dart
void main() {
  for (int i = 1; i <= 5; i++) {
    print('Count: $i');
  }
}
```
**Output:**
```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

---

### 2. The `for-in` Loop

The `for-in` loop provides a cleaner, more readable way to iterate over the elements of a collection, such as a `List`, `Set`, or `Map`.

**Syntax:**
```dart
for (var item in collection) {
  // Code to be executed for each item.
}
```

**Example:**
Let's print a list of fruits.

```dart
void main() {
  List<String> fruits = ['Apple', 'Banana', 'Cherry'];

  for (String fruit in fruits) {
    print('I love $fruit!');
  }
}
```
**Output:**
```
I love Apple!
I love Banana!
I love Cherry!
```

---

### 3. The `while` Loop

A `while` loop executes a block of code **as long as** a specified condition remains `true`. The condition is checked *before* each iteration.

This loop is useful when you don't know the exact number of iterations beforehand, but you know the condition that must be met to stop.

**Syntax:**
```dart
while (condition) {
  // Code to execute as long as the condition is true.
}
```

**Example:**
Let's simulate a countdown that stops when it reaches zero.

```dart
void main() {
  int countdown = 3;

  while (countdown > 0) {
    print('$countdown...');
    countdown--; // Decrement the counter.
  }

  print('Blast off! 🚀');
}
```
**Output:**
```
3...
2...
1...
Blast off! 🚀
```
**Caution**: Be careful with `while` loops! If the condition never becomes `false`, you'll create an **infinite loop**, which will crash your program.

---

### 4. The `do-while` Loop

The `do-while` loop is similar to the `while` loop, but with one key difference: the code block is executed **at least once** before the condition is checked.

**Syntax:**
```dart
do {
  // Code to be executed.
} while (condition);
```

**Example:**
Let's create a simple menu that always displays at least once.

```dart
import 'dart:io';

void main() {
  String? choice;
  
  do {
    print('\n--- Menu ---');
    print('1. View Profile');
    print('2. Exit');
    stdout.write('Enter your choice: ');
    
    choice = stdin.readLineSync();
    
    if (choice == '1') {
      print('Displaying user profile...');
    }
    
  } while (choice != '2'); // The loop continues until the user chooses to exit.
  
  print('Goodbye!');
}
```

---

### 5. Controlling Loops: `break` and `continue`

You can alter the flow of a loop using the `break` and `continue` statements.

- **`break`**: Immediately terminates the entire loop, regardless of the loop's condition. Execution continues at the first line of code *after* the loop.
- **`continue`**: Skips the current iteration and proceeds to the next one.

**Example with `break`:**
Find the first even number in a list.

```dart
void main() {
  List<int> numbers = [1, 3, 5, 6, 7, 9];
  
  for (int number in numbers) {
    if (number % 2 == 0) {
      print('Found the first even number: $number');
      break; // Exit the loop immediately.
    }
    print('Checking $number...');
  }
}
```
**Output:**
```
Checking 1...
Checking 3...
Checking 5...
Found the first even number: 6
```

**Example with `continue`:**
Print only the odd numbers from a list.

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  for (int number in numbers) {
    if (number % 2 == 0) {
      continue; // Skip the rest of the loop for even numbers.
    }
    print('Odd number: $number');
  }
}
```
**Output:**
```
Odd number: 1
Odd number: 3
Odd number: 5
```

[⬅ Previous](topic-3-1-conditional-statements.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-3-3-switch-and-case.md)

# Topic 8.1: Core Asynchronous Concepts

[⬅ Previous](topic-7-2-modern-oop-features.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-8-2-advanced-asynchrony.md)

Most applications need to perform long-running operations, like fetching data from a network, reading a file, or querying a database. If your application's code waited for these operations to complete, the user interface would freeze and become unresponsive. This is called **blocking**.

**Asynchronous programming** is the solution. It's a way of writing code that can start a long-running task and then continue with other work without waiting for that task to finish. When the task is done, your code is notified and can process the result.

This topic introduces the fundamental building blocks of asynchronous programming in Dart: `Future`, `async`, and `await`.

---

### 1. The `Future`: A Promise of a Value

The central concept in Dart's async model is the `Future`. A `Future` is an object that represents a value that will be available *at some time in the future*. Think of it as a receipt you get when you order food: you don't have the food yet, but you have a promise that it will be delivered to you later.

A `Future` can be in one of two states:
- **Uncompleted:** The operation hasn't finished yet.
- **Completed:** The operation has finished, and the `Future` holds either:
  - A **value** (if the operation was successful).
  - An **error** (if the operation failed).

Let's create a function that returns a `Future`.

```dart
// This function simulates fetching a user's name from a network.
// It returns a Future that will complete with a String value after 2 seconds.
Future<String> fetchUserName() {
  return Future.delayed(Duration(seconds: 2), () => 'Alice');
}

// This function simulates an operation that fails.
Future<String> fetchFailingData() {
  return Future.delayed(Duration(seconds: 1), () => throw Exception('Network error!'));
}

void main() {
  print('Requesting user name...');
  final futureName = fetchUserName();
  print('Main function continues while waiting for the future to complete.');
  print('The future has not completed yet. Its runtime type is: ${futureName.runtimeType}');
}
```

---

### 2. Handling Futures: The Callback-Based Approach (`.then`)

Once you have a `Future`, how do you get the value out of it when it completes? The traditional way is to use the `.then()` method.

- `.then((value) { ... })`: Registers a callback function that will run when the `Future` completes successfully.
- `.catchError((error) { ... })`: Registers a callback for when the `Future` completes with an error.
- `.whenComplete(() { ... })`: Registers a callback that runs after the `Future` completes, regardless of success or failure.

```dart
void main() {
  print('Fetching user order...');
  
  fetchUserOrder()
    .then((order) {
      // This code runs ONLY after the future completes successfully.
      print('Success! Your order is: $order');
    })
    .catchError((error) {
      // This code runs ONLY if the future completes with an error.
      print('Oops! Something went wrong: $error');
    })
    .whenComplete(() {
      // This code ALWAYS runs after the future is done.
      print('The order process is complete.');
    });

  print('The main function does not wait and finishes its execution.');
}

Future<String> fetchUserOrder() {
  // Simulate a network request that takes 2 seconds.
  return Future.delayed(Duration(seconds: 2), () => 'Large Espresso');
  // To see the error case, uncomment the line below and comment out the line above.
  // return Future.delayed(Duration(seconds: 2), () => throw Exception('Out of milk!'));
}
```
While `.then()` works, it can become difficult to read when you need to chain multiple asynchronous operations, a situation often called "Callback Hell."

---

### 3. Handling Futures: The Modern Approach (`async` and `await`)

Dart provides the `async` and `await` keywords as a cleaner, more readable way to work with `Future`s. This modern syntax makes your asynchronous code look almost like synchronous code.

- **`async`**: A keyword used to mark a function as asynchronous. This allows you to use `await` inside it and automatically makes the function return a `Future`.
- **`await`**: A keyword that tells Dart to pause the execution of the current `async` function until the `Future` you're awaiting completes. It can only be used inside an `async` function.

Let's rewrite the previous example using `async`/`await`.

```dart
// The function signature now includes the 'async' keyword.
Future<void> processUserOrder() async {
  print('Fetching user order...');
  try {
    // 'await' pauses this function until fetchUserOrder() completes.
    // It then "unwraps" the Future to give you the String value directly.
    String order = await fetchUserOrder();
    print('Success! Your order is: $order');
  } catch (error) {
    print('Oops! Something went wrong: $error');
  } finally {
    print('The order process is complete.');
  }
}

// 'main' must also be marked 'async' to use 'await'.
void main() async {
  await processUserOrder();
  print('The main function waited for the process to finish.');
}

Future<String> fetchUserOrder() {
  return Future.delayed(Duration(seconds: 2), () => 'Large Espresso');
}
```
Notice how much cleaner this is! The logic flows top-to-bottom, and you can use standard `try/catch/finally` blocks for error handling, just like with synchronous code.

---

### 4. Running Multiple Futures in Parallel (`Future.wait`)

Sometimes you need to run several asynchronous operations and wait for all of them to finish. `Future.wait` is perfect for this. It takes a list of `Future`s and returns a single `Future` that completes with a list of their results when all of them have completed.

```dart
Future<String> fetchFirstName() async {
  await Future.delayed(Duration(seconds: 1));
  return 'John';
}

Future<String> fetchLastName() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Doe';
}

void main() async {
  print('Fetching user name parts simultaneously...');
  
  // Future.wait starts both operations at the same time.
  // It will complete after the longest future (fetchLastName) is done.
  List<String> nameParts = await Future.wait([
    fetchFirstName(),
    fetchLastName(),
  ]);

  print('Full name: ${nameParts[0]} ${nameParts[1]}');
}
```

---

### 5. How Does It Work? The Event Loop

Dart is a single-threaded language, meaning it can only do one thing at a time. So how does it handle `await` without freezing? The answer is the **Event Loop**.

When you `await` an operation, Dart doesn't actually block. Instead, it tells the event loop: "I'm waiting for this `Future` to complete. While I wait, feel free to do other work. When it's done, come back and continue executing this function from where I left off."

This allows Dart to handle UI events, timers, and other asynchronous tasks while long-running operations are in progress, keeping your application responsive.

[⬅ Previous](topic-7-2-modern-oop-features.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-8-2-advanced-asynchrony.md)

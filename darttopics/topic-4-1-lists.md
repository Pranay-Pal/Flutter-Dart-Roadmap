# Topic 4.1: Lists

[⬅ Previous](topic-3-3-switch-and-case.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-2-sets.md)

**Collections** are data structures that group multiple items into a single unit. The most common collection in Dart is the **`List`**, which represents an ordered sequence of objects. Lists are incredibly versatile and are used for everything from storing a simple sequence of numbers to managing complex UI elements.

A `List` in Dart is an ordered, indexable collection that allows duplicate values.

---

### 1. Creating Lists

You can create lists in several ways, depending on your needs.

**a. List Literals**
The most common way is with a list literal: `[]`.

```dart
void main() {
  // A list of strings. Dart infers the type `List<String>`.
  var fruits = ['Apple', 'Banana', 'Orange'];

  // A list of integers.
  List<int> numbers = [1, 2, 3, 4, 5];

  // An empty list. You must specify the type.
  var emptyList = <String>[];
  
  print(fruits);
  print(numbers);
  print('Is the list empty? ${emptyList.isEmpty}');
}
```

**b. Generated Lists**
You can create lists programmatically using constructors.

- **`List.filled()`**: Creates a list of a given size with a fixed value.
- **`List.generate()`**: Creates a list by running a function for each index.

```dart
void main() {
  // Creates a list with 5 elements, all of which are 'a'.
  var filledList = List.filled(5, 'a'); // [a, a, a, a, a]
  print(filledList);

  // Creates a list of the first 5 square numbers: [0, 1, 4, 9, 16]
  var generatedList = List.generate(5, (index) => index * index);
  print(generatedList);
}
```

---

### 2. Accessing Elements

You can access elements by their index, which starts at 0.

```dart
void main() {
  var colors = ['Red', 'Green', 'Blue'];

  // Access by index
  print(colors[0]); // Output: Red
  print(colors[2]); // Output: Blue

  // Get the first and last elements
  print(colors.first); // Output: Red
  print(colors.last);  // Output: Blue

  // Get the total number of elements
  print(colors.length); // Output: 3
}
```

---

### 3. Modifying a List

You can add, update, and remove elements from a "growable" list (the default kind).

**a. Adding Elements**

- **`add()`**: Appends one element to the end.
- **`addAll()`**: Appends multiple elements to the end.
- **`insert()`**: Adds an element at a specific index.

```dart
void main() {
  var shoppingCart = ['Milk', 'Bread'];
  
  // Add a single item
  shoppingCart.add('Eggs');
  print(shoppingCart); // [Milk, Bread, Eggs]

  // Add multiple items
  shoppingCart.addAll(['Butter', 'Cheese']);
  print(shoppingCart); // [Milk, Bread, Eggs, Butter, Cheese]
  
  // Insert an item at index 1
  shoppingCart.insert(1, 'Yogurt');
  print(shoppingCart); // [Milk, Yogurt, Bread, Eggs, Butter, Cheese]
}
```

**b. Updating Elements**
You can change an element at a specific index using the assignment operator.

```dart
void main() {
  var tasks = ['Morning walk', 'Eat breakfast', 'Code'];
  tasks[1] = 'Eat a healthy breakfast'; // Update the element at index 1
  print(tasks); // [Morning walk, Eat a healthy breakfast, Code]
}
```

**c. Removing Elements**

- **`remove()`**: Removes the first occurrence of a specific value.
- **`removeAt()`**: Removes the element at a specific index.
- **`removeLast()`**: Removes and returns the last element.
- **`clear()`**: Removes all elements.

```dart
void main() {
  var items = ['A', 'B', 'C', 'B', 'D'];
  
  items.remove('B'); // Removes the first 'B'
  print(items); // [A, C, B, D]
  
  items.removeAt(2); // Removes the element at index 2 ('B')
  print(items); // [A, C, D]
  
  items.removeLast(); // Removes 'D'
  print(items); // [A, C]
  
  items.clear();
  print(items); // []
}
```

---

### 4. Common List Operations

**a. Iterating Over a List**
Use a `for-in` loop or the `forEach` method.

```dart
void main() {
  var planets = ['Earth', 'Mars', 'Jupiter'];

  // Using a for-in loop
  for (var planet in planets) {
    print('Hello, $planet!');
  }

  // Using forEach
  planets.forEach((planet) {
    print('Welcome to $planet!');
  });
}
```

**b. Transforming a List (`map`)**
The `map` method creates a new list by applying a function to each element of an existing list.

```dart
void main() {
  var numbers = [1, 2, 3];
  
  // Create a new list where each number is doubled.
  var doubled = numbers.map((n) => n * 2).toList();
  
  print(doubled); // [2, 4, 6]
}
```

**c. Filtering a List (`where`)**
The `where` method creates a new list containing only the elements that satisfy a condition.

```dart
void main() {
  var scores = [100, 45, 88, 92, 53];
  
  // Create a new list of only passing scores.
  var passingScores = scores.where((score) => score >= 60).toList();
  
  print(passingScores); // [100, 88, 92]
}
```

**d. Sorting a List (`sort`)**
The `sort` method sorts the list in place.

```dart
void main() {
  var letters = ['c', 'a', 'd', 'b'];
  letters.sort();
  print(letters); // [a, b, c, d]

  // Custom sort (e.g., by string length)
  var names = ['Mia', 'Alexander', 'Leo'];
  names.sort((a, b) => a.length.compareTo(b.length));
  print(names); // [Leo, Mia, Alexander]
}
```

---

### 5. Spread Operator and Collection `if`/`for`

These modern features provide a powerful, declarative way to build lists.

- **Spread Operator (`...`)**: Unpacks a list's elements into another list.
- **Collection `if`**: Includes an element conditionally.
- **Collection `for`**: Creates elements using a loop.

```dart
void main() {
  var list1 = [1, 2, 3];
  var list2 = [4, 5, 6];
  bool addExtra = true;

  var combined = [
    0,
    ...list1, // Spread operator
    ...list2,
    if (addExtra) 7, // Collection if
    for (var i = 8; i < 11; i++) i, // Collection for
  ];

  print(combined); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
}
```

[⬅ Previous](topic-3-3-switch-and-case.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-2-sets.md)

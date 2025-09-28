# Topic 4.2: Sets

[⬅ Previous](topic-4-1-lists.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-3-maps.md)

A **`Set`** in Dart is an unordered collection of **unique** items. Unlike a `List`, a `Set` does not allow duplicate elements. This makes `Set`s particularly useful when you need to store a collection of distinct values and quickly check for the presence of an item.

Think of a `Set` as a bag of items where order doesn't matter and every item is unique.

---

### 1. Creating Sets

You can create a `Set` using a set literal `{}` or from another collection.

**a. Set Literals** This is the most direct way to create a set.

```dart
void main() {
  // A set of strings. Dart infers the type `Set<String>`.
  var elements = {'Hydrogen', 'Helium', 'Lithium'};

  // An empty set. Note the type annotation to distinguish it from a Map.
  var emptySet = <String>{};
  // Or more explicitly:
  // Set<String> emptySet = {};

  print(elements);
  print('Is the set empty? ${emptySet.isEmpty}');
}
```

**b. Creating a Set from a List** This is a common way to remove duplicate values from a list.

```dart
void main() {
  var numbersWithDuplicates = [1, 2, 2, 3, 1, 4];
  
  // The `toSet()` method creates a set with only the unique items.
  var uniqueNumbers = numbersWithDuplicates.toSet();
  
  print(uniqueNumbers); // {1, 2, 3, 4}
}
```

---

### 2. Adding and Removing Elements

You can easily add and remove items from a set.

- **`add()`**: Adds an element. If the element is already in the set, nothing changes.
- **`addAll()`**: Adds multiple elements from another collection.
- **`remove()`**: Removes a specific element.

```dart
void main() {
  var fruits = {'apple', 'banana'};
  
  // Add a new element
  fruits.add('orange');
  print(fruits); // {apple, banana, orange}

  // Try to add an existing element (it will be ignored)
  fruits.add('apple');
  print(fruits); // {apple, banana, orange} - no change

  // Add multiple elements
  fruits.addAll(['kiwi', 'mango']);
  print(fruits); // {apple, banana, orange, kiwi, mango}

  // Remove an element
  fruits.remove('banana');
  print(fruits); // {apple, orange, kiwi, mango}
}
```

---

### 3. Checking for Elements

The most efficient operation on a `Set` is checking for the existence of an item using `contains()`.

```dart
void main() {
  var permissions = {'read', 'write', 'execute'};

  // Check if the set contains specific items.
  print(permissions.contains('write'));    // true
  print(permissions.contains('admin'));    // false
}
```

---

### 4. Core Set Operations

`Set`s are powerful because they support mathematical operations like union, intersection, and difference. These are useful for comparing and combining collections of data.

Let's use two sets for our examples:

```dart
final setA = {1, 2, 3, 4};
final setB = {3, 4, 5, 6};
```

**a. Intersection (`intersection`)** Finds the elements that are common to both sets.

```dart
void main() {
  final setA = {1, 2, 3, 4};
  final setB = {3, 4, 5, 6};

  var commonElements = setA.intersection(setB);
  print(commonElements); // {3, 4}
}
```

**b. Union (`union`)** Combines both sets into a new set, containing all unique elements from both.

```dart
void main() {
  final setA = {1, 2, 3, 4};
  final setB = {3, 4, 5, 6};

  var allElements = setA.union(setB);
  print(allElements); // {1, 2, 3, 4, 5, 6}
}
```

**c. Difference (`difference`)** Finds the elements that are in the first set but **not** in the second set.

```dart
void main() {
  final setA = {1, 2, 3, 4};
  final setB = {3, 4, 5, 6};

  // Elements in A but not in B
  var differenceA = setA.difference(setB);
  print(differenceA); // {1, 2}

  // Elements in B but not in A
  var differenceB = setB.difference(setA);
  print(differenceB); // {5, 6}
}
```

---

### 5. Iterating Over a Set

You can iterate over the items in a set using a `for-in` loop or `forEach`. Note that the order of iteration is **not guaranteed**.

```dart
void main() {
  var ingredients = {'flour', 'sugar', 'eggs'};

  for (var ingredient in ingredients) {
    print('You will need: $ingredient');
  }
}
```

[⬅ Previous](topic-4-1-lists.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-3-maps.md)

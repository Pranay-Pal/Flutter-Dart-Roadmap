# Topic 4.1: Lists

    * [ ] Creating and manipulating ordered lists of items

#### Creating and Manipulating Lists

Lists are ordered collections of items that can contain duplicates. They are one of the most commonly used data structures in Dart.

**Creating lists:**
```dart
void main() {
  // Creating lists with different approaches
  List<String> fruits = ['apple', 'banana', 'orange'];
  List<int> numbers = [1, 2, 3, 4, 5];
  List<double> prices = [10.99, 25.50, 5.75];
  
  // Creating an empty list
  List<String> emptyList = [];
  List<String> emptyList2 = <String>[];
  List<String> emptyList3 = List<String>.empty(growable: true);
  
  // Creating list with initial values
  List<int> filledList = List.filled(5, 0); // [0, 0, 0, 0, 0]
  List<int> generatedList = List.generate(5, (index) => index + 1); // [1, 2, 3, 4, 5]
  
  print('Fruits: $fruits');
  print('Numbers: $numbers');
  print('Prices: $prices');
  print('Filled list: $filledList');
  print('Generated list: $generatedList');
}
```

**Accessing list elements:**
```dart
void main() {
  List<String> colors = ['red', 'green', 'blue', 'yellow', 'purple'];
  
  // Accessing by index (0-based)
  print('First color: ${colors[0]}');
  print('Third color: ${colors[2]}');
  print('Last color: ${colors[colors.length - 1]}');
  
  // Safe access methods
  print('First color (safe): ${colors.first}');
  print('Last color (safe): ${colors.last}');
  
  // Get element at index with default value
  print('Element at index 10: ${colors.elementAtOrNull(10) ?? 'Not found'}');
  
  // List properties
  print('List length: ${colors.length}');
  print('Is empty: ${colors.isEmpty}');
  print('Is not empty: ${colors.isNotEmpty}');
}

// Extension method for safe element access (Dart 3.0+)
extension SafeList<T> on List<T> {
  T? elementAtOrNull(int index) {
    if (index >= 0 && index < length) {
      return this[index];
    }
    return null;
  }
}
```

**Adding and removing elements:**
```dart
void main() {
  List<String> shoppingCart = ['milk', 'bread'];
  
  print('Initial cart: $shoppingCart');
  
  // Adding elements
  shoppingCart.add('eggs');                    // Add single item
  shoppingCart.addAll(['butter', 'cheese']);   // Add multiple items
  shoppingCart.insert(1, 'yogurt');           // Insert at specific position
  shoppingCart.insertAll(0, ['fruits', 'vegetables']); // Insert multiple at position
  
  print('After adding: $shoppingCart');
  
  // Removing elements
  shoppingCart.remove('bread');               // Remove first occurrence
  shoppingCart.removeAt(0);                  // Remove at specific index
  String removed = shoppingCart.removeLast(); // Remove and return last element
  shoppingCart.removeWhere((item) => item.startsWith('v')); // Remove by condition
  
  print('After removing: $shoppingCart');
  print('Removed item: $removed');
  
  // Clear all elements
  List<String> tempList = ['a', 'b', 'c'];
  tempList.clear();
  print('Cleared list: $tempList');
}
```

**List operations and methods:**
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 3, 2, 1];
  
  // Searching
  print('Contains 3: ${numbers.contains(3)}');
  print('Index of 3: ${numbers.indexOf(3)}');
  print('Last index of 3: ${numbers.lastIndexOf(3)}');
  
  // Sublist operations
  List<int> subList = numbers.sublist(1, 4); // [2, 3, 4]
  print('Sublist (1 to 4): $subList');
  
  // Reversing
  List<int> reversed = numbers.reversed.toList();
  print('Reversed: $reversed');
  
  // Sorting
  List<String> names = ['Charlie', 'Alice', 'Bob', 'Diana'];
  names.sort(); // Sorts in place
  print('Sorted names: $names');
  
  List<int> scores = [85, 92, 78, 96, 88];
  scores.sort((a, b) => b.compareTo(a)); // Sort descending
  print('Sorted scores (desc): $scores');
  
  // Joining
  String joined = names.join(', ');
  print('Joined names: $joined');
}
```

**Advanced list operations:**
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  
  // Filtering
  List<int> evenNumbers = numbers.where((n) => n % 2 == 0).toList();
  print('Even numbers: $evenNumbers');
  
  // Mapping (transforming)
  List<int> squared = numbers.map((n) => n * n).toList();
  print('Squared numbers: $squared');
  
  // Reducing
  int sum = numbers.reduce((a, b) => a + b);
  print('Sum: $sum');
  
  // Folding
  int product = numbers.fold(1, (prev, current) => prev * current);
  print('Product: $product');
  
  // Any and every
  bool hasEven = numbers.any((n) => n % 2 == 0);
  bool allPositive = numbers.every((n) => n > 0);
  print('Has even number: $hasEven');
  print('All positive: $allPositive');
  
  // Take and skip
  List<int> firstThree = numbers.take(3).toList();
  List<int> skipFirstTwo = numbers.skip(2).toList();
  print('First three: $firstThree');
  print('Skip first two: $skipFirstTwo');
}
```

# Topic 4.4: Collection Tools

[⬅ Previous](topic-4-3-maps.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-5-1-function-basics.md)

Beyond the basic methods for creating and modifying collections, Dart offers a powerful set of syntactic features that make collection manipulation more declarative and readable. These tools, including the **spread operator (`...`)** and **collection `if` and `for`**, allow you to build complex collections in an intuitive and concise way.

This topic covers these modern collection manipulation techniques, which are essential for writing clean and efficient Dart code.

---

### 1. The Spread Operator (`...` and `...?`)

The spread operator unpacks the elements from one collection and inserts them into another. It's a concise way to combine collections.

**a. Spreading a List**
You can easily merge two lists into one.

```dart
void main() {
  var list1 = [1, 2, 3];
  var list2 = [4, 5, 6];

  var combinedList = [...list1, ...list2];
  
  print(combinedList); // [1, 2, 3, 4, 5, 6]
}
```

**b. Null-Aware Spread Operator (`...?`)**
If you have a collection that might be `null`, the null-aware spread operator (`...?`) will simply do nothing instead of throwing an error.

```dart
void main() {
  List<int>? list1 = [1, 2, 3];
  List<int>? list2; // This list is null

  var combinedList = [...?list1, ...?list2, 4, 5];
  
  print(combinedList); // [1, 2, 3, 4, 5]
}
```

**c. Spreading Maps and Sets**
The spread operator works just as well with maps and sets. For maps, if both have the same key, the value from the later map overwrites the earlier one.

```dart
void main() {
  // Spreading a Set
  var set1 = {'a', 'b'};
  var set2 = {'b', 'c', 'd'};
  var combinedSet = {...set1, ...set2};
  print(combinedSet); // {a, b, c, d}

  // Spreading a Map
  var map1 = {'name': 'Alice', 'age': 30};
  var map2 = {'age': 31, 'country': 'Canada'}; // 'age' will be updated
  var combinedMap = {...map1, ...map2};
  print(combinedMap); // {name: Alice, age: 31, country: Canada}
}
```

---

### 2. Collection `if`

Collection `if` allows you to conditionally include an element in a collection literal.

**Syntax:**
```dart
var myList = [
  'item1',
  if (condition) 'item2',
  'item3',
];
```

**Example:**
Let's build a list of command-line arguments, adding a flag only if a condition is met.

```dart
void main() {
  bool verbose = true;
  
  var args = [
    '--input',
    'file.txt',
    if (verbose) '--verbose', // This element is included only if `verbose` is true.
    '--output',
    'out.txt'
  ];
  
  print(args); // [--input, file.txt, --verbose, --output, out.txt]
}
```

This also works for conditionally including key-value pairs in maps.

```dart
void main() {
  bool isAdmin = false;
  
  var userProfile = {
    'name': 'John',
    'role': 'user',
    if (isAdmin) 'permissions': 'all'
  };
  
  print(userProfile); // {name: John, role: user}
}
```

---

### 3. Collection `for`

Collection `for` lets you generate elements inside a collection literal using a loop. It's a concise alternative to creating an empty list and then using a `for` loop to add items to it.

**Syntax:**
```dart
var myList = [
  for (var i = 0; i < 5; i++) i,
];
```

**Example:**
Let's create a list of the first five square numbers.

```dart
void main() {
  var squares = [
    for (var i = 1; i <= 5; i++) i * i
  ];
  
  print(squares); // [1, 4, 9, 16, 25]
}
```

You can also use it to transform one collection into another.

```dart
void main() {
  var fruits = ['apple', 'banana', 'orange'];
  
  var loudFruits = [
    for (var fruit in fruits) fruit.toUpperCase()
  ];
  
  print(loudFruits); // [APPLE, BANANA, ORANGE]
}
```

---

### 4. Putting It All Together

These features are most powerful when combined to build complex collections declaratively.

**Example:**
Let's create a shopping cart UI widget list.

```dart
// A simple placeholder for a UI widget
class Widget {
  final String name;
  Widget(this.name);
  @override
  String toString() => 'Widget($name)';
}

void main() {
  var regularItems = ['Milk', 'Bread'];
  List<String>? specialOfferItems; // Might be null
  bool showPromo = true;

  var cartWidgets = [
    Widget('Cart Header'),
    for (var item in regularItems) Widget(item), // Collection for
    ..._getRecommendedWidgets(),                 // Spread operator
    if (showPromo) Widget('Promo Banner'),       // Collection if
    ...?_getSpecialOfferWidgets(specialOfferItems), // Null-aware spread
    Widget('Cart Footer'),
  ];

  print(cartWidgets);
}

// Helper functions to simulate fetching other widgets
List<Widget> _getRecommendedWidgets() => [Widget('Recommended: Eggs')];
List<Widget>? _getSpecialOfferWidgets(List<String>? items) {
  if (items == null) return null;
  return [for (var item in items) Widget('Special: $item')];
}
```
This example demonstrates how you can build a list in a single, readable expression, handling loops, conditionals, and nulls inline.

[⬅ Previous](topic-4-3-maps.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-5-1-function-basics.md)

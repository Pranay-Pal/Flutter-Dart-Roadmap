# Topic 4.3: Maps

[⬅ Previous](topic-4-2-sets.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-4-collection-tools.md)

A **`Map`** is a dynamic collection that represents a set of values as **key-value pairs**. Each key in a map must be unique, and it is used to retrieve the corresponding value. Maps are also known as dictionaries or associative arrays in other programming languages.

Maps are incredibly useful for storing and retrieving data when you can associate it with a unique identifier. For example, you could map a user ID to a user profile, a product SKU to its details, or a configuration setting to its value.

---

### 1. Creating Maps

You can create a `Map` using a map literal `{}` or a constructor.

**a. Map Literals** This is the most common and concise way to create a map.

```dart
void main() {
  // A map of string keys to integer values.
  var ages = {
    'Alice': 30,
    'Bob': 25,
    'Charlie': 35,
  };

  // A map of string keys to string values.
  Map<String, String> capitals = {
    'USA': 'Washington, D.C.',
    'Japan': 'Tokyo',
  };

  // An empty map.
  var emptyMap = <String, int>{};

  print(ages);
  print(capitals);
  print('Is the map empty? ${emptyMap.isEmpty}');
}
```

**b. Using the `Map` Constructor** You can also create an empty map using the `Map()` constructor.

```dart
void main() {
  var scores = Map<String, int>();
  scores['math'] = 95;
  scores['english'] = 88;
  print(scores); // {math: 95, english: 88}
}
```

---

### 2. Accessing and Modifying Values

You can access, add, and update values using the subscript operator `[]`.

**a. Accessing Values** Use the key to look up the corresponding value. If the key doesn't exist, it returns `null`.

```dart
void main() {
  var user = {
    'name': 'John Doe',
    'email': 'john.doe@example.com',
  };

  print(user['name']); // Output: John Doe
  print(user['age']); // Output: null (because the key 'age' does not exist)
}
```

**b. Adding and Updating Values** If the key already exists, its value is updated. If it doesn't exist, a new key-value pair is added.

```dart
void main() {
  var config = {
    'theme': 'dark',
  };

  // Update an existing value
  config['theme'] = 'light';
  print(config); // {theme: light}

  // Add a new key-value pair
  config['fontSize'] = 16;
  print(config); // {theme: light, fontSize: 16}
}
```

---

### 3. Removing Entries

You can remove key-value pairs using the `remove()` method, which returns the value of the removed key.

```dart
void main() {
  var inventory = {
    'apples': 10,
    'bananas': 20,
    'oranges': 15,
  };

  // Remove the 'bananas' entry and get its value.
  int? removedValue = inventory.remove('bananas');

  print('Removed $removedValue bananas.'); // Removed 20 bananas.
  print(inventory); // {apples: 10, oranges: 15}
}
```

---

### 4. Common Properties and Methods

Maps have several useful properties and methods for checking their state.

- **`keys`**: Returns an iterable of all the keys.
- **`values`**: Returns an iterable of all the values.
- **`length`**: Returns the number of key-value pairs.
- **`isEmpty` / `isNotEmpty`**: Checks if the map is empty.
- **`containsKey()`**: Checks if a key exists.
- **`containsValue()`**: Checks if a value exists.

```dart
void main() {
  var product = {
    'name': 'Laptop',
    'price': 999.99,
    'inStock': true,
  };

  print('Keys: ${product.keys}'); // (name, price, inStock)
  print('Values: ${product.values}'); // (Laptop, 999.99, true)
  print('Length: ${product.length}'); // 3

  print('Does it have a price? ${product.containsKey('price')}'); // true
  print('Is anything free? ${product.containsValue(0)}'); // false
}
```

---

### 5. Iterating Over a Map

You can iterate over a map's keys, values, or entries.

**a. Iterating Over Entries** The `forEach` method or a `for-in` loop on `entries` gives you access to both the key and value.

```dart
void main() {
  var scores = {'Math': 90, 'Science': 85, 'History': 92};

  // Using forEach
  scores.forEach((subject, score) {
    print('$subject score: $score');
  });

  // Using a for-in loop with entries
  for (var entry in scores.entries) {
    print('${entry.key}: ${entry.value}');
  }
}
```

**b. Iterating Over Keys or Values**

```dart
void main() {
  var scores = {'Math': 90, 'Science': 85};

  // Loop over keys
  for (var key in scores.keys) {
    print('Subject: $key');
  }

  // Loop over values
  for (var value in scores.values) {
    print('Score: $value');
  }
}
```

---

### 6. Spread Operator and Collection `if`/`for`

Just like with lists, you can use these modern features to build maps declaratively.

```dart
void main() {
  var baseProfile = {
    'name': 'User',
    'email': 'user@example.com',
  };
  bool includeAdmin = true;

  var fullProfile = {
    ...baseProfile, // Spread the base profile
    'id': '12345',
    if (includeAdmin) 'isAdmin': true, // Collection if
  };

  print(fullProfile); // {name: User, email: user@example.com, id: 12345, isAdmin: true}
}
```

[⬅ Previous](topic-4-2-sets.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-4-4-collection-tools.md)

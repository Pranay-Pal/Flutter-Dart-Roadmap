# Topic 6.2: Initializing Objects with Constructors

[⬅ Previous](topic-6-1-classes-and-objects.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-6-3-inheritance.md)

In the last topic, we created a `Dog` object but had to set its properties manually after creation. This is clumsy and error-prone. What if we want to ensure every `Dog` has a name and breed from the very beginning?

This is the job of a **constructor**: a special method that runs automatically when you create a new object. Its purpose is to initialize the object's properties and get it ready for use.

Dart provides a flexible and powerful set of constructor options, which we'll explore here.

---

### 1. The Default Constructor

The most common type of constructor is the default constructor. It has the same name as the class. Dart provides a handy "syntactic sugar" for assigning parameters directly to properties.

**Syntax:** `ClassName(this.property1, this.property2);`

This is a shorthand for:
`ClassName(String property1, String property2) {
  this.property1 = property1;
  this.property2 = property2;
}`

```dart
class Car {
  String model;
  int year;

  // Default constructor with syntactic sugar.
  // This automatically assigns the 'model' and 'year' arguments
  // to the instance properties of the same name.
  Car(this.model, this.year);

  void display() {
    print('Car: $year $model');
  }
}

void main() {
  // Now, we must provide the initial values when creating the object.
  Car myCar = Car('Toyota Camry', 2022);
  myCar.display(); // Output: Car: 2022 Toyota Camry

  // Car anotherCar = Car(); // This would cause an error because the constructor requires arguments.
}
```

---

### 2. Named Constructors

Sometimes, you need more than one way to create an object. **Named constructors** give you this flexibility. They let you define additional constructors with descriptive names to clarify their purpose.

**Syntax:** `ClassName.constructorName(parameters)`

Let's create a `Color` class that can be created from RGB values or from a common name.

```dart
class Color {
  int red;
  int green;
  int blue;

  // Default constructor
  Color(this.red, this.green, this.blue);

  // Named constructor for creating a black color
  Color.black() {
    red = 0;
    green = 0;
    blue = 0;
  }

  // Named constructor for creating a white color
  Color.white() {
    red = 255;
    green = 255;
    blue = 255;
  }

  void display() {
    print('RGB($red, $green, $blue)');
  }
}

void main() {
  Color customColor = Color(13, 188, 121);
  Color black = Color.black();
  Color white = Color.white();

  customColor.display(); // Output: RGB(13, 188, 121)
  black.display();       // Output: RGB(0, 0, 0)
  white.display();       // Output: RGB(255, 255, 255)
}
```

---

### 3. Constant Constructors

If your class creates objects that are immutable (their properties cannot change), you can make its constructor `const`. This offers a significant performance benefit: if you create multiple constant objects with the same property values, Dart is smart enough to reuse a **single instance** in memory.

**Rules for `const` constructors:**
1. All properties of the class must be `final`.
2. The constructor must be marked with the `const` keyword.

```dart
class ImmutablePoint {
  // 1. All properties must be final.
  final double x;
  final double y;

  // 2. The constructor is marked as 'const'.
  const ImmutablePoint(this.x, this.y);
  
  // You can also have named const constructors.
  const ImmutablePoint.origin() : x = 0, y = 0;
}

void main() {
  // Create two const objects with the same values.
  const point1 = ImmutablePoint(10, 20);
  const point2 = ImmutablePoint(10, 20);
  const origin = ImmutablePoint.origin();

  // Because they are identical constants, Dart reuses the same object.
  // The 'identical()' function checks if two references point to the exact same object in memory.
  print(identical(point1, point2)); // Output: true

  // A non-const object would create a new instance every time.
  final nonConst1 = ImmutablePoint(30, 40);
  final nonConst2 = ImmutablePoint(30, 40);
  print(identical(nonConst1, nonConst2)); // Output: false
}
```

> **Use Case:** `const` constructors are heavily used in Flutter for widgets. Since widgets are immutable, this allows the framework to be highly efficient by rebuilding only what has changed.

---

### 4. Factory Constructors

A `factory` constructor is a special type of constructor that gives you more control over the object creation process. Unlike other constructors, a `factory` constructor **does not always have to create a new instance of its class**.

It can:
- Return an instance from a cache (like in the Singleton pattern).
- Return an instance of a subclass.
- Decide which type of object to create based on input parameters.

**Syntax:** `factory ClassName(parameters) { ... }`

**Example: The Singleton Pattern**
A singleton is a class that can only have one instance. A factory constructor is perfect for this.

```dart
class Logger {
  final String name;
  static final Map<String, Logger> _cache = {};

  // 1. A private named constructor prevents creating instances from outside.
  Logger._internal(this.name);

  // 2. The factory constructor decides whether to create a new instance
  //    or return an existing one from the cache.
  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name]!;
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  void log(String message) {
    print('[$name] $message');
  }
}

void main() {
  // Requesting a logger for 'UI'.
  var logger1 = Logger('UI');
  logger1.log('Button clicked.');

  // Requesting the 'UI' logger again.
  var logger2 = Logger('UI');
  
  // The factory constructor returns the *same* instance.
  print(identical(logger1, logger2)); // Output: true

  var apiLogger = Logger('API');
  apiLogger.log('Fetched data.');
  
  // This is a different instance.
  print(identical(logger1, apiLogger)); // Output: false
}
```

By mastering these constructor types, you gain fine-grained control over how your objects are created, leading to more robust, readable, and efficient code.

[⬅ Previous](topic-6-1-classes-and-objects.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-6-3-inheritance.md)

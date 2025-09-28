# Topic 7.1: Advanced Blueprints: Abstract Classes, Interfaces, and Mixins

[⬅ Previous](topic-6-3-inheritance.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-7-2-modern-oop-features.md)

While `class` and `extends` are powerful, Dart provides more advanced ways to define class blueprints. These tools help you write flexible, decoupled, and scalable code by separating a class's capabilities from its implementation.

This topic covers three key concepts:
- **Abstract Classes:** Partial blueprints that can't be instantiated.
- **Interfaces:** Contracts that a class must follow.
- **Mixins:** Reusable chunks of behavior that can be added to any class.

---

### 1. Abstract Classes: Blueprints That Can't Be Instantiated

An **abstract class** is like a regular class, but it cannot be turned into an object directly. Its purpose is to serve as a base class for other classes to extend.

Abstract classes are useful when you want to define a common set of behaviors or properties that several related classes will share, but you also want to force subclasses to provide their own specific implementation for certain methods.

**Key Features:**
- Defined with the `abstract` keyword.
- Cannot be instantiated: `var shape = Shape();` will cause an error.
- Can contain **abstract methods**: methods without a body (just a signature). Subclasses **must** provide an implementation for these methods.
- Can also contain regular methods with bodies, which subclasses inherit.

```dart
// This abstract class defines the "idea" of a shape.
abstract class Shape {
  // A regular method, inherited by all subclasses.
  void describe() {
    print('I am a shape.');
  }

  // An abstract method. It has no body.
  // Any class that extends Shape MUST provide its own 'calculateArea' method.
  double calculateArea();
}

class Square extends Shape {
  final double side;
  Square(this.side);

  // We MUST implement the abstract method from Shape.
  @override
  double calculateArea() {
    return side * side;
  }
}

class Circle extends Shape {
  final double radius;
  Circle(this.radius);

  // We also MUST implement the abstract method.
  @override
  double calculateArea() {
    return 3.14159 * radius * radius;
  }
}

void main() {
  // var myShape = Shape(); // Error: Abstract classes can't be instantiated.

  var mySquare = Square(10);
  mySquare.describe(); // Inherited from Shape -> Output: I am a shape.
  print('Square Area: ${mySquare.calculateArea()}'); // Output: 100.0

  var myCircle = Circle(5);
  myCircle.describe(); // Inherited from Shape -> Output: I am a shape.
  print('Circle Area: ${myCircle.calculateArea()}'); // Output: 78.53975
}
```

---

### 2. Interfaces: Defining Capabilities with `implements`

In Dart, there is no special `interface` keyword. Instead, **every class implicitly defines an interface.**

When a class `implements` an interface, it is signing a contract. It agrees to provide its own implementation for **all** the properties and methods of the interface class. Unlike `extends`, it does not inherit any code.

**Key Features:**
- Use the `implements` keyword.
- A class can implement multiple interfaces, allowing it to wear "many hats."
- Forces the implementing class to provide its own version of every method and property in the interface. It's about defining *what* an object can do, not *how* it does it.

```dart
// We can define a class to serve as an interface.
// Often, these are abstract so they can't be instantiated.
abstract class Logger {
  void log(String message);
}

abstract class Storable {
  void save();
  void load();
}

// This User class implements BOTH Logger and Storable.
// It must provide its own implementation for all their methods.
class User implements Logger, Storable {
  String name;
  User(this.name);

  @override
  void log(String message) {
    print('Log for user $name: $message');
  }

  @override
  void save() {
    print('Saving user $name to database...');
  }

  @override
  void load() {
    print('Loading user $name from database...');
  }
}

void main() {
  var user = User('Alice');
  user.log('User created.');
  user.save();
}
```

**`extends` vs. `implements`**
- `extends`: "Is-a" relationship. A `Dog` is an `Animal`. You get code reuse. A class can only extend one superclass.
- `implements`: "Can-do" relationship. A `User` can do logging. You get a contract, but no code reuse. A class can implement many interfaces.

---

### 3. Mixins: Adding Reusable Behavior with `with`

Mixins are a way to reuse code in multiple class hierarchies. They solve the problem of single inheritance: what if you want to share behavior between classes that aren't in the same inheritance chain?

A mixin is a chunk of code (properties and methods) that you can "mix in" to a class using the `with` keyword.

**Key Features:**
- Defined with the `mixin` keyword.
- Added to a class using the `with` keyword.
- A class can use multiple mixins.
- They are a form of "composition over inheritance," allowing you to add capabilities without creating deep inheritance trees.

```dart
// A mixin that provides the ability to fly.
mixin Flyer {
  void fly() {
    print('I am flying!');
  }
}

// A mixin that provides the ability to swim.
mixin Swimmer {
  void swim() {
    print('I am swimming!');
  }
}

class Animal {
  String name;
  Animal(this.name);
}

// A Duck can do everything an Animal can do, PLUS it can fly and swim.
class Duck extends Animal with Flyer, Swimmer {
  Duck(String name) : super(name);
}

// A Bat can fly but cannot swim.
class Bat extends Animal with Flyer {
  Bat(String name) : super(name);
}

// A Fish can swim but cannot fly.
class Fish extends Animal with Swimmer {
  Fish(String name) : super(name);
}

void main() {
  var duck = Duck('Donald');
  var bat = Bat('Bruce');
  var fish = Fish('Nemo');

  duck.fly();   // From Flyer mixin
  duck.swim();  // From Swimmer mixin

  bat.fly();    // From Flyer mixin
  // bat.swim(); // Error: 'swim' isn't defined for the class 'Bat'.

  fish.swim();  // From Swimmer mixin
}
```

By combining classes, abstract classes, interfaces, and mixins, you can build incredibly robust and well-structured applications in Dart.

[⬅ Previous](topic-6-3-inheritance.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-7-2-modern-oop-features.md)

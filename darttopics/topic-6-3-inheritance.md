# Topic 6.3: Code Reuse with Inheritance

[⬅ Previous](topic-6-2-constructors.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-7-1-advanced-blueprints.md)

**Inheritance** is a fundamental principle of OOP that allows you to create a new class that is a more specialized version of an existing class. This new class, called the **subclass** (or child class), inherits the properties and methods of the existing class, called the **superclass** (or parent class).

This is powerful because it promotes code reuse. Instead of copying and pasting code, you can define common behavior in a superclass and then extend it to create more specific subclasses.

---

### 1. Extending a Class with `extends`

You use the `extends` keyword to create a subclass. The subclass automatically gains access to all non-private properties and methods of its superclass.

Let's define a general `Animal` class and then create a more specific `Dog` class that inherits from it.

```dart
// The Superclass (Parent)
class Animal {
  String name;
  int age;

  Animal(this.name, this.age);

  void eat() {
    print('$name is eating.');
  }

  void makeSound() {
    print('$name makes a generic sound.');
  }
}

// The Subclass (Child)
// Dog inherits 'name', 'age', 'eat()', and 'makeSound()' from Animal.
class Dog extends Animal {
  String breed;

  // How do we initialize the properties from the Animal class?
  // We need to call the parent's constructor. See the next section!
  Dog(String name, int age, this.breed) : super(name, age);

  // Dog can also have its own methods.
  void wagTail() {
    print('$name is wagging its tail.');
  }
}

void main() {
  Dog myDog = Dog('Buddy', 4, 'Golden Retriever');

  // Using methods inherited from Animal.
  myDog.eat(); // Output: Buddy is eating.

  // Using a method specific to Dog.
  myDog.wagTail(); // Output: Buddy is wagging its tail.
}
```

---

### 2. Calling the Superclass Constructor with `super`

A subclass's constructor must always call the constructor of its superclass to ensure that all inherited properties are properly initialized. You do this using the `super` keyword in the constructor's initializer list (the part after the colon `:`).

**Syntax:** `SubclassConstructor(params) : super(paramsForSuperclass);`

```dart
class Vehicle {
  String brand;
  int year;

  Vehicle(this.brand, this.year) {
    print('Vehicle constructor finished.');
  }
}

class Car extends Vehicle {
  String model;

  // The Car constructor takes all three parameters.
  // It passes 'brand' and 'year' up to the Vehicle constructor using 'super'.
  Car(String brand, int year, this.model) : super(brand, year) {
    print('Car constructor finished.');
  }
}

void main() {
  Car myCar = Car('Ford', 2023, 'Mustang');
  // Output:
  // Vehicle constructor finished.
  // Car constructor finished.
}
```

---

### 3. Overriding Methods with `@override`

A subclass can provide its own specific implementation of a method that it inherited from its superclass. This is called **method overriding**.

To override a method, you define a method in the subclass with the exact same name and signature. It is a strong convention and a good practice to use the `@override` annotation to make your intention clear.

```dart
class Animal {
  String name;
  Animal(this.name);

  void makeSound() {
    print('$name makes a generic sound.');
  }
}

class Cat extends Animal {
  Cat(String name) : super(name);

  // The Cat class provides its own version of makeSound().
  @override
  void makeSound() {
    print('$name meows.');
  }
}

class Dog extends Animal {
  Dog(String name) : super(name);

  // The Dog class also provides its own version.
  @override
  void makeSound() {
    // You can still execute the original superclass method using 'super'.
    super.makeSound(); 
    print('$name also barks!');
  }
}

void main() {
  Animal genericAnimal = Animal('Creature');
  Cat myCat = Cat('Whiskers');
  Dog myDog = Dog('Buddy');

  genericAnimal.makeSound(); // Output: Creature makes a generic sound.
  myCat.makeSound();         // Output: Whiskers meows.
  myDog.makeSound();         // Output:
                             // Buddy makes a generic sound.
                             // Buddy also barks!
}
```

The `@override` annotation is not strictly required by the Dart compiler, but it's a critical tool for developers. It tells the compiler (and other programmers) that you are intentionally replacing a superclass method. If you misspell the method name or change its parameters, the compiler will give you an error, preventing subtle bugs.

Inheritance is a cornerstone of building complex, hierarchical systems in OOP. By understanding `extends`, `super`, and `@override`, you can create clean, reusable, and maintainable code.

---

### 4. The `covariant` Keyword: Relaxing Parameter Types

Sometimes when overriding a method, you need to accept a more specific type in the subclass method than what the superclass method allows. By default, Dart's type system prevents this to ensure type safety.

The `covariant` keyword allows you to "relax" this rule. It tells the compiler that you are intentionally overriding a method parameter with a subtype, and you are taking responsibility for ensuring it's used correctly.

This is most common in Flutter widget code but is a general Dart feature.

```dart
// Superclass: A general-purpose storage for any kind of 'Item'.
class Item {
  String name;
  Item(this.name);
}

class DigitalStorage {
  void store(Item item) {
    print('Storing ${item.name} in the cloud.');
  }
}

// Subclass: A specialized storage that should only handle 'Book' items.
class Book extends Item {
  String author;
  Book(String name, this.author) : super(name);
}

class Bookshelf extends DigitalStorage {
  // Here, we want to override 'store' to only accept 'Book' objects.
  // Without 'covariant', this would be a compile-time error.
  @override
  void store(covariant Book book) {
    print('Placing "${book.name}" by ${book.author} on the bookshelf.');
  }
}

void main() {
  Bookshelf myBookshelf = Bookshelf();
  Book myBook = Book('The Hobbit', 'J.R.R. Tolkien');
  
  // This is safe and works as expected.
  myBookshelf.store(myBook); // Output: Placing "The Hobbit" by J.R.R. Tolkien on the bookshelf.

  // The danger of 'covariant':
  // If you treat the subclass as its superclass, you can pass the wrong type.
  DigitalStorage storage = myBookshelf;
  Item genericItem = Item('A Movie');

  // This line will compile but will cause a runtime error!
  // You are responsible for preventing this when using 'covariant'.
  try {
    storage.store(genericItem); 
  } catch (e) {
    print('\nError: $e'); // A type error occurs here.
  }
}
```
**When to use `covariant`:** Use it sparingly and only when you are certain that the subclass method will only be called with the correct subtype. It's a trade-off between flexibility and compile-time safety.


[⬅ Previous](topic-6-2-constructors.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-7-1-advanced-blueprints.md)

# Topic 7.1: Advanced Blueprints

    * [ ] Abstract classes and methods
    * [ ] Implicit interfaces with `implements`
    * [ ] Reusing code with `mixins`

#### Abstract Classes and Methods

Abstract classes define a contract that subclasses must follow. They cannot be instantiated directly.

```dart
// Abstract class
abstract class Shape {
  String name;
  
  Shape(this.name);
  
  // Abstract methods - must be implemented by subclasses
  double calculateArea();
  double calculatePerimeter();
  
  // Concrete method - can be used by subclasses
  void displayInfo() {
    print('Shape: $name');
    print('Area: ${calculateArea().toStringAsFixed(2)}');
    print('Perimeter: ${calculatePerimeter().toStringAsFixed(2)}');
  }
}

class Circle extends Shape {
  double radius;
  
  Circle(this.radius) : super('Circle');
  
  @override
  double calculateArea() => 3.14159 * radius * radius;
  
  @override
  double calculatePerimeter() => 2 * 3.14159 * radius;
}

class Rectangle extends Shape {
  double width, height;
  
  Rectangle(this.width, this.height) : super('Rectangle');
  
  @override
  double calculateArea() => width * height;
  
  @override
  double calculatePerimeter() => 2 * (width + height);
}

void main() {
  // Shape shape = Shape('Generic'); // Error: Cannot instantiate abstract class
  
  List<Shape> shapes = [
    Circle(5.0),
    Rectangle(4.0, 6.0),
  ];
  
  for (Shape shape in shapes) {
    shape.displayInfo();
    print('---');
  }
}
```

#### Implicit Interfaces with implements

Every class defines an implicit interface. Use `implements` to ensure a class provides all methods of another class.

```dart
class Flyable {
  void fly() => print('Flying through the air');
}

class Swimmable {
  void swim() => print('Swimming in water');
}

// Bird implements both interfaces
class Bird implements Flyable, Swimmable {
  String species;
  
  Bird(this.species);
  
  @override
  void fly() => print('$species is flying with wings');
  
  @override
  void swim() => print('$species is swimming on water surface');
}

class Fish implements Swimmable {
  String species;
  
  Fish(this.species);
  
  @override
  void swim() => print('$species is swimming underwater');
  
  // Fish doesn't need to implement fly()
}

void main() {
  Bird duck = Bird('Duck');
  Fish salmon = Fish('Salmon');
  
  duck.fly();
  duck.swim();
  salmon.swim();
}
```

#### Reusing Code with Mixins

Mixins allow you to reuse code across multiple class hierarchies.

```dart
mixin Walkable {
  void walk() => print('Walking on legs');
}

mixin Flyable {
  void fly() => print('Flying with wings');
}

mixin Swimmable {
  void swim() => print('Swimming in water');
}

class Animal {
  String name;
  Animal(this.name);
}

class Duck extends Animal with Walkable, Flyable, Swimmable {
  Duck(String name) : super(name);
  
  @override
  void fly() => print('$name is flying low over water');
}

class Penguin extends Animal with Walkable, Swimmable {
  Penguin(String name) : super(name);
  
  @override
  void swim() => print('$name is swimming underwater');
}

void main() {
  Duck duck = Duck('Donald');
  Penguin penguin = Penguin('Pingu');
  
  duck.walk();
  duck.fly();
  duck.swim();
  
  penguin.walk();
  penguin.swim();
  // penguin.fly(); // Error: Penguin doesn't have fly()
}
```

# Topic 6.3: Inheritance

    * [ ] Extending classes with `extends`, using `super`, and `@override`

#### Inheritance with extends, super, and @override

Inheritance allows a class to inherit properties and methods from another class, promoting code reuse.

**Basic inheritance:**
```dart
// Base class (parent/superclass)
class Animal {
  String name;
  int age;
  
  Animal(this.name, this.age);
  
  void eat() {
    print('$name is eating');
  }
  
  void sleep() {
    print('$name is sleeping');
  }
  
  void makeSound() {
    print('$name makes a sound');
  }
  
  void displayInfo() {
    print('Animal: $name, Age: $age');
  }
}

// Derived class (child/subclass)
class Dog extends Animal {
  String breed;
  
  Dog(String name, int age, this.breed) : super(name, age);
  
  // Override parent method
  @override
  void makeSound() {
    print('$name barks: Woof! Woof!');
  }
  
  // Additional method specific to Dog
  void wagTail() {
    print('$name is wagging tail happily');
  }
  
  // Override displayInfo to include breed
  @override
  void displayInfo() {
    super.displayInfo(); // Call parent method
    print('Breed: $breed');
  }
}

class Cat extends Animal {
  bool isIndoor;
  
  Cat(String name, int age, this.isIndoor) : super(name, age);
  
  @override
  void makeSound() {
    print('$name meows: Meow! Meow!');
  }
  
  void purr() {
    print('$name is purring contentedly');
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Indoor cat: $isIndoor');
  }
}

void main() {
  // Creating instances of derived classes
  Dog dog = Dog('Buddy', 3, 'Golden Retriever');
  Cat cat = Cat('Whiskers', 2, true);
  
  // Using inherited methods
  dog.eat();
  dog.sleep();
  cat.eat();
  cat.sleep();
  
  // Using overridden methods
  dog.makeSound();
  cat.makeSound();
  
  // Using class-specific methods
  dog.wagTail();
  cat.purr();
  
  // Using overridden displayInfo
  print('\nDog Information:');
  dog.displayInfo();
  
  print('\nCat Information:');
  cat.displayInfo();
}
```

**Advanced inheritance with method overriding:**
```dart
class Vehicle {
  String brand;
  String model;
  int year;
  double speed = 0;
  
  Vehicle(this.brand, this.model, this.year);
  
  void start() {
    print('$brand $model is starting...');
  }
  
  void stop() {
    speed = 0;
    print('$brand $model has stopped');
  }
  
  void accelerate(double increment) {
    speed += increment;
    print('$brand $model is now going ${speed.toStringAsFixed(1)} km/h');
  }
  
  virtual void displaySpeedLimit() {
    print('General speed limit applies');
  }
  
  void displayInfo() {
    print('Vehicle: $brand $model ($year)');
    print('Current speed: ${speed.toStringAsFixed(1)} km/h');
  }
}

class Car extends Vehicle {
  int numberOfDoors;
  String fuelType;
  
  Car(String brand, String model, int year, this.numberOfDoors, this.fuelType) 
      : super(brand, model, year);
  
  @override
  void start() {
    print('Starting car engine...');
    super.start(); // Call parent start method
    print('Car is ready to drive');
  }
  
  @override
  void accelerate(double increment) {
    if (speed + increment > 180) {
      print('Warning: Speed limit for cars is 180 km/h');
      speed = 180;
    } else {
      super.accelerate(increment);
    }
  }
  
  @override
  void displaySpeedLimit() {
    print('Speed limit for cars: 180 km/h');
  }
  
  void honk() {
    print('$brand $model: Beep! Beep!');
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Doors: $numberOfDoors, Fuel: $fuelType');
  }
}

class Motorcycle extends Vehicle {
  bool hasSidecar;
  
  Motorcycle(String brand, String model, int year, this.hasSidecar) 
      : super(brand, model, year);
  
  @override
  void start() {
    print('Kick-starting motorcycle...');
    super.start();
    print('Motorcycle is ready to ride');
  }
  
  @override
  void accelerate(double increment) {
    if (speed + increment > 120) {
      print('Warning: Speed limit for motorcycles is 120 km/h');
      speed = 120;
    } else {
      super.accelerate(increment);
    }
  }
  
  @override
  void displaySpeedLimit() {
    print('Speed limit for motorcycles: 120 km/h');
  }
  
  void wheelie() {
    if (hasSidecar) {
      print('Cannot perform wheelie with sidecar');
    } else {
      print('$brand $model is doing a wheelie!');
    }
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Has sidecar: $hasSidecar');
  }
}

void main() {
  Car car = Car('Toyota', 'Camry', 2023, 4, 'Gasoline');
  Motorcycle bike = Motorcycle('Harley-Davidson', 'Sportster', 2022, false);
  
  print('=== Car Operations ===');
  car.start();
  car.accelerate(50);
  car.accelerate(100);
  car.accelerate(50); // Should trigger speed limit warning
  car.honk();
  car.displaySpeedLimit();
  car.displayInfo();
  car.stop();
  
  print('\n=== Motorcycle Operations ===');
  bike.start();
  bike.accelerate(60);
  bike.accelerate(80); // Should trigger speed limit warning
  bike.wheelie();
  bike.displaySpeedLimit();
  bike.displayInfo();
  bike.stop();
  
  // Polymorphism - treating derived classes as base class
  print('\n=== Polymorphism Demo ===');
  List<Vehicle> vehicles = [car, bike];
  
  for (Vehicle vehicle in vehicles) {
    vehicle.displaySpeedLimit(); // Calls overridden method
    vehicle.accelerate(30);
  }
}
```

**Using super in constructors and methods:**
```dart
class Employee {
  String name;
  String id;
  double baseSalary;
  
  Employee(this.name, this.id, this.baseSalary) {
    print('Employee constructor called for $name');
  }
  
  double calculatePay() {
    return baseSalary;
  }
  
  void displayInfo() {
    print('Employee: $name (ID: $id)');
    print('Base Salary: \$${baseSalary.toStringAsFixed(2)}');
  }
}

class Manager extends Employee {
  double bonus;
  int teamSize;
  
  Manager(String name, String id, double baseSalary, this.bonus, this.teamSize) 
      : super(name, id, baseSalary) {
    print('Manager constructor called for $name');
  }
  
  @override
  double calculatePay() {
    double basePay = super.calculatePay(); // Get base salary from parent
    return basePay + bonus;
  }
  
  @override
  void displayInfo() {
    super.displayInfo(); // Call parent displayInfo first
    print('Bonus: \$${bonus.toStringAsFixed(2)}');
    print('Team Size: $teamSize');
    print('Total Pay: \$${calculatePay().toStringAsFixed(2)}');
  }
  
  void conductMeeting() {
    print('$name is conducting a team meeting with $teamSize members');
  }
}

class Developer extends Employee {
  String programmingLanguage;
  int experienceYears;
  
  Developer(String name, String id, double baseSalary, this.programmingLanguage, this.experienceYears) 
      : super(name, id, baseSalary) {
    print('Developer constructor called for $name');
  }
  
  @override
  double calculatePay() {
    double basePay = super.calculatePay();
    // Add experience bonus
    double experienceBonus = experienceYears * 1000;
    return basePay + experienceBonus;
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('Programming Language: $programmingLanguage');
    print('Experience: $experienceYears years');
    print('Total Pay: \$${calculatePay().toStringAsFixed(2)}');
  }
  
  void writeCode() {
    print('$name is writing code in $programmingLanguage');
  }
}

void main() {
  print('Creating employees...\n');
  
  Manager manager = Manager('Alice Johnson', 'MGR001', 80000, 15000, 5);
  Developer developer = Developer('Bob Smith', 'DEV001', 70000, 'Dart', 3);
  
  print('\n=== Manager Information ===');
  manager.displayInfo();
  manager.conductMeeting();
  
  print('\n=== Developer Information ===');
  developer.displayInfo();
  developer.writeCode();
  
  // Demonstrating polymorphism
  print('\n=== Payroll Processing ===');
  List<Employee> employees = [manager, developer];
  
  double totalPayroll = 0;
  for (Employee emp in employees) {
    double pay = emp.calculatePay(); // Calls overridden method
    totalPayroll += pay;
    print('${emp.name}: \$${pay.toStringAsFixed(2)}');
  }
  
  print('Total Payroll: \$${totalPayroll.toStringAsFixed(2)}');
}
```

### **Module 7: Object-Oriented Programming (OOP) - Part 2**

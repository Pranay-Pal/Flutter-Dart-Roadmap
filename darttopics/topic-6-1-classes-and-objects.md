# Topic 6.1: The Foundation of OOP: Classes and Objects

[⬅ Previous](topic-5-3-advanced-function-concepts.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-6-2-constructors.md)

Welcome to Object-Oriented Programming (OOP)! This is a powerful way of thinking about and structuring your code. Instead of just writing functions that operate on data, we'll start creating "objects" that bundle data and the functions that operate on that data together.

Think of it like this:
- A **Class** is a blueprint. For example, a blueprint for a car defines that it will have a color, a brand, and the ability to accelerate or brake.
- An **Object** is the actual thing created from that blueprint. For example, a red Toyota that you can actually drive. You can create many objects (many cars) from the same class (the single blueprint).

This topic covers the absolute basics: creating the blueprint (class) and then creating and using the real thing (object).

---

### 1. Defining a Class

A class is a blueprint for an object. You define it using the `class` keyword. Inside the class, you define its properties (data) and methods (behavior).

- **Properties** (also called fields or instance variables) are variables that store the object's data.
- **Methods** are functions that define the object's behavior.

Let's create a simple blueprint for a `Dog`.

```dart
class Dog {
  // 1. Properties: Data associated with each dog object.
  String name = 'Unnamed';
  String breed = 'Unknown';
  int age = 0;

  // 2. Methods: Actions the dog can perform.
  void bark() {
    print('Woof! My name is $name.');
  }

  void haveBirthday() {
    age++;
    print('Happy birthday! I am now $age years old.');
  }
}
```

---

### 2. Creating an Object (An Instance of a Class)

Once you have a class (the blueprint), you can create actual objects from it. This is called **instantiation**.

You create an object by calling the class name like a function. You can then use dot notation (`.`) to access its properties and methods.

```dart
void main() {
  // Create an object (instance) of the Dog class.
  Dog myDog = Dog();

  // Note: The `new` keyword is optional in modern Dart.
  // `Dog myDog = new Dog();` is also valid but less common now.

  // Access and modify its properties.
  myDog.name = 'Buddy';
  myDog.breed = 'Golden Retriever';
  myDog.age = 3;

  // Call its methods.
  myDog.bark(); // Output: Woof! My name is Buddy.
  myDog.haveBirthday(); // Output: Happy birthday! I am now 4 years old.

  // You can create multiple, independent objects from the same class.
  Dog anotherDog = Dog();
  anotherDog.name = 'Lucy';
  anotherDog.bark(); // Output: Woof! My name is Lucy.
  
  print('${myDog.name} is ${myDog.age} years old.'); // Output: Buddy is 4 years old.
  print('${anotherDog.name} is ${anotherDog.age} years old.'); // Output: Lucy is 0 years old.
}
```

---

### 3. The `this` Keyword

The `this` keyword refers to the **current instance** of the object. It's used to eliminate ambiguity between instance variables and parameters with the same name. While we'll see its most important use in constructors (Topic 6.2), it can also clarify which variable you're referring to inside a method.

```dart
class Car {
  String model;
  int year;

  // A constructor (more on this in the next topic!)
  Car(this.model, this.year);

  // This method uses 'this' to refer to the instance's properties.
  void displayInfo() {
    // Here, 'this.model' explicitly refers to the 'model' property of the Car instance.
    print('Car Model: ${this.model}, Year: ${this.year}');
  }
}

void main() {
  Car myCar = Car('Honda Civic', 2021);
  myCar.displayInfo(); // Output: Car Model: Honda Civic, Year: 2021
}
```

---

### 4. Encapsulation: Private Properties, Getters, and Setters

**Encapsulation** is a core OOP principle. It means bundling the data (properties) and methods that operate on the data within a single unit (the class) and controlling access to that data from the outside world.

In Dart, you make a property or method private by prefixing its name with an underscore (`_`).

To allow controlled access to private properties, we use **getters** and **setters**.
- A **getter** is a special method that reads the value of a private property.
- A **setter** is a special method that updates the value of a private property, often including validation logic.

```dart
class BankAccount {
  // Private properties cannot be accessed directly from outside the file.
  String _accountHolder;
  double _balance;

  BankAccount(this._accountHolder, this._balance);

  // Public Getter for the balance.
  // Allows others to read the balance but not change it directly.
  double get balance => _balance;

  // Public Getter for the account holder.
  String get accountHolder => _accountHolder;

  // Public Setter for the account holder.
  // Allows changing the name, but with validation.
  set accountHolder(String name) {
    if (name.trim().isNotEmpty) {
      _accountHolder = name;
    } else {
      print('Error: Account holder name cannot be empty.');
    }
  }

  // Public methods to operate on the private data.
  void deposit(double amount) {
    if (amount > 0) {
      _balance += amount;
      print('Deposited \$$amount. New balance: \$$_balance');
    }
  }

  void withdraw(double amount) {
    if (amount > 0 && amount <= _balance) {
      _balance -= amount;
      print('Withdrew \$$amount. New balance: \$$_balance');
    } else {
      print('Error: Invalid withdrawal amount or insufficient funds.');
    }
  }
}

void main() {
  BankAccount myAccount = BankAccount('John Doe', 500.0);

  // Accessing data via getters.
  print('Account Holder: ${myAccount.accountHolder}'); // John Doe
  print('Balance: \$${myAccount.balance}'); // $500.0

  // myAccount.balance = 10000; // This would cause an error because there is no setter for balance.

  // Using a setter with validation.
  myAccount.accountHolder = 'Jane Doe';
  print('New Account Holder: ${myAccount.accountHolder}'); // Jane Doe

  myAccount.accountHolder = ''; // Error: Account holder name cannot be empty.

  // Using public methods to modify private state.
  myAccount.deposit(200.0); // Deposited $200.0. New balance: $700.0
  myAccount.withdraw(1000.0); // Error: Invalid withdrawal amount or insufficient funds.
}
```

---

### 5. Static Properties and Methods

Sometimes, a property or method should belong to the **class itself**, not to any single instance of the class. These are called **static** members.

You access them directly on the class, without creating an object.

- **Static Properties:** Constants or variables that are shared across all instances of a class.
- **Static Methods:** Utility functions that are related to the class but don't need an instance to work.

```dart
class MathUtils {
  // Static property (a constant shared by all).
  static const double pi = 3.14159;
  
  // Static property to track state at the class level.
  static int _operationCount = 0;

  // Static method. It doesn't make sense to create a "MathUtils" object
  // just to add two numbers.
  static int add(int a, int b) {
    _operationCount++;
    return a + b;
  }
  
  // Static getter for a private static property.
  static int get operationCount => _operationCount;
}

void main() {
  // You call static members directly on the class.
  print('Value of Pi: ${MathUtils.pi}');

  int sum = MathUtils.add(10, 5);
  print('10 + 5 = $sum');
  
  MathUtils.add(3, 4);
  
  print('Total operations performed: ${MathUtils.operationCount}'); // Output: 2
}
```

With these fundamentals, you're ready to dive deeper into how objects are initialized with constructors in the next topic!

[⬅ Previous](topic-5-3-advanced-function-concepts.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-6-2-constructors.md)

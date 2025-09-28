# Topic 6.1: Classes and Objects

[⬅ Previous](topic-5-3-advanced-function-concepts.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-6-2-constructors.md)

    * [ ] Creating classes, properties (fields), and methods

#### Creating Classes, Properties (Fields), and Methods

Classes are blueprints for creating objects. They encapsulate data (properties) and behavior (methods) into a single unit.

**Basic class structure:**
```dart
class Person {
  // Properties (fields)
  String name;
  int age;
  String email;
  
  // Constructor
  Person(this.name, this.age, this.email);
  
  // Methods
  void introduce() {
    print('Hi, I\'m $name, $age years old. Email: $email');
  }
  
  void celebrateBirthday() {
    age++;
    print('Happy birthday! $name is now $age years old.');
  }
  
  String getInfo() {
    return 'Name: $name, Age: $age, Email: $email';
  }
}

void main() {
  // Creating objects (instances of the class)
  Person person1 = Person('Alice', 25, 'alice@example.com');
  Person person2 = Person('Bob', 30, 'bob@example.com');
  
  // Using object methods
  person1.introduce();
  person2.introduce();
  
  // Accessing and modifying properties
  print('${person1.name} is ${person1.age} years old');
  person1.celebrateBirthday();
  
  // Using getter methods
  print(person1.getInfo());
}
```

**Class with private properties and getters/setters:**
```dart
class BankAccount {
  String _accountNumber; // Private property (starts with _)
  String _holderName;
  double _balance;
  
  BankAccount(this._accountNumber, this._holderName, this._balance);
  
  // Getters
  String get accountNumber => _accountNumber;
  String get holderName => _holderName;
  double get balance => _balance;
  
  // Setter with validation
  set holderName(String name) {
    if (name.trim().isEmpty) {
      throw ArgumentError('Name cannot be empty');
    }
    _holderName = name;
  }
  
  // Methods
  void deposit(double amount) {
    if (amount <= 0) {
      print('Deposit amount must be positive');
      return;
    }
    _balance += amount;
    print('Deposited \$${amount.toStringAsFixed(2)}. New balance: \$${_balance.toStringAsFixed(2)}');
  }
  
  bool withdraw(double amount) {
    if (amount <= 0) {
      print('Withdrawal amount must be positive');
      return false;
    }
    if (amount > _balance) {
      print('Insufficient funds. Available balance: \$${_balance.toStringAsFixed(2)}');
      return false;
    }
    _balance -= amount;
    print('Withdrew \$${amount.toStringAsFixed(2)}. New balance: \$${_balance.toStringAsFixed(2)}');
    return true;
  }
  
  void displayAccountInfo() {
    print('Account: $_accountNumber');
    print('Holder: $_holderName');
    print('Balance: \$${_balance.toStringAsFixed(2)}');
  }
}

void main() {
  BankAccount account = BankAccount('ACC123456', 'John Doe', 1000.0);
  
  account.displayAccountInfo();
  
  // Using getters
  print('Account holder: ${account.holderName}');
  print('Current balance: \$${account.balance.toStringAsFixed(2)}');
  
  // Using methods
  account.deposit(500.0);
  account.withdraw(200.0);
  account.withdraw(2000.0); // Should fail
  
  // Using setter
  account.holderName = 'John Smith';
  account.displayAccountInfo();
}
```

**Static properties and methods:**
```dart
class MathUtils {
  // Static property
  static const double pi = 3.14159;
  static int _calculationCount = 0;
  
  // Static methods
  static double calculateCircleArea(double radius) {
    _calculationCount++;
    return pi * radius * radius;
  }
  
  static double calculateRectangleArea(double width, double height) {
    _calculationCount++;
    return width * height;
  }
  
  static int getCalculationCount() {
    return _calculationCount;
  }
  
  static void resetCalculationCount() {
    _calculationCount = 0;
  }
}

class Student {
  String name;
  int grade;
  static int totalStudents = 0; // Tracks total number of students
  
  Student(this.name, this.grade) {
    totalStudents++; // Increment when new student is created
  }
  
  void displayInfo() {
    print('Student: $name, Grade: $grade');
  }
  
  static void displayTotalStudents() {
    print('Total students: $totalStudents');
  }
}

void main() {
  // Using static methods without creating instances
  double circleArea = MathUtils.calculateCircleArea(5.0);
  double rectArea = MathUtils.calculateRectangleArea(4.0, 6.0);
  
  print('Circle area: ${circleArea.toStringAsFixed(2)}');
  print('Rectangle area: ${rectArea.toStringAsFixed(2)}');
  print('Total calculations: ${MathUtils.getCalculationCount()}');
  
  // Using static properties
  print('Value of Pi: ${MathUtils.pi}');
  
  // Tracking students
  Student.displayTotalStudents(); // 0
  
  Student student1 = Student('Alice', 90);
  Student student2 = Student('Bob', 85);
  Student student3 = Student('Charlie', 92);
  
  Student.displayTotalStudents(); // 3
  
  student1.displayInfo();
  student2.displayInfo();
  student3.displayInfo();
}
```

### **Module 6: Object-Oriented Programming (OOP) - Part 1**

[⬅ Previous](topic-5-3-advanced-function-concepts.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-6-2-constructors.md)

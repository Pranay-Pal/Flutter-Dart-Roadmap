# Topic 5.1: Function Basics

    * [ ] Defining functions with parameters and return values

#### Defining Functions with Parameters and Return Values

Functions are reusable blocks of code that perform specific tasks. They help organize code and avoid repetition.

**Basic function structure:**
```dart
// Function declaration syntax:
// returnType functionName(parameters) {
//   // function body
//   return value; // if return type is not void
// }

void main() {
  // Calling functions
  greetUser();
  sayHello('Alice');
  
  int result = addNumbers(5, 3);
  print('5 + 3 = $result');
  
  double area = calculateCircleArea(5.0);
  print('Area of circle with radius 5: ${area.toStringAsFixed(2)}');
}

// Function with no parameters and no return value
void greetUser() {
  print('Hello, welcome to Dart programming!');
}

// Function with parameter but no return value
void sayHello(String name) {
  print('Hello, $name!');
}

// Function with parameters and return value
int addNumbers(int a, int b) {
  return a + b;
}

// Function with calculation
double calculateCircleArea(double radius) {
  const double pi = 3.14159;
  return pi * radius * radius;
}
```

**Functions with different return types:**
```dart
void main() {
  // String function
  String message = createWelcomeMessage('Bob', 25);
  print(message);
  
  // Boolean function
  bool isEligible = checkVotingEligibility(20);
  print('Can vote: $isEligible');
  
  // List function
  List<int> numbers = generateNumbers(5);
  print('Generated numbers: $numbers');
  
  // Map function
  Map<String, dynamic> profile = createUserProfile('Charlie', 30, 'Engineer');
  print('User profile: $profile');
}

String createWelcomeMessage(String name, int age) {
  return 'Welcome $name! You are $age years old.';
}

bool checkVotingEligibility(int age) {
  return age >= 18;
}

List<int> generateNumbers(int count) {
  List<int> numbers = [];
  for (int i = 1; i <= count; i++) {
    numbers.add(i);
  }
  return numbers;
}

Map<String, dynamic> createUserProfile(String name, int age, String job) {
  return {
    'name': name,
    'age': age,
    'job': job,
    'createdAt': DateTime.now().toString(),
  };
}
```

**Function documentation and best practices:**
```dart
/// Calculates the Body Mass Index (BMI) for a person.
/// 
/// The [weight] should be in kilograms and [height] in meters.
/// Returns the BMI value as a double.
/// 
/// Example:
/// ```dart
/// double bmi = calculateBMI(70.0, 1.75);
/// print('BMI: ${bmi.toStringAsFixed(1)}'); // BMI: 22.9
/// ```
double calculateBMI(double weight, double height) {
  if (height <= 0) {
    throw ArgumentError('Height must be greater than 0');
  }
  return weight / (height * height);
}

/// Determines BMI category based on BMI value.
/// 
/// Returns a string describing the BMI category:
/// - 'Underweight' for BMI < 18.5
/// - 'Normal' for BMI 18.5-24.9
/// - 'Overweight' for BMI 25-29.9
/// - 'Obese' for BMI >= 30
String getBMICategory(double bmi) {
  if (bmi < 18.5) {
    return 'Underweight';
  } else if (bmi < 25.0) {
    return 'Normal';
  } else if (bmi < 30.0) {
    return 'Overweight';
  } else {
    return 'Obese';
  }
}

void main() {
  try {
    double bmi = calculateBMI(70.0, 1.75);
    String category = getBMICategory(bmi);
    
    print('BMI: ${bmi.toStringAsFixed(1)}');
    print('Category: $category');
  } catch (e) {
    print('Error: $e');
  }
}
```

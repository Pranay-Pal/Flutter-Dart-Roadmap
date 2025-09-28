# Topic 3.1: Conditional Statements

    * [ ] `if`, `else`, and `else if`

#### If, Else, and Else If Statements

Conditional statements allow your program to make decisions and execute different code paths based on conditions.

**Basic if statement:**
```dart
void main() {
  int temperature = 75;
  
  if (temperature > 80) {
    print('It\'s hot outside!');
  }
  
  print('Temperature check complete.');
}
```

**If-else statement:**
```dart
void main() {
  int age = 17;
  
  if (age >= 18) {
    print('You are eligible to vote.');
  } else {
    print('You are not eligible to vote yet.');
  }
  
  // Another example
  bool isRaining = true;
  
  if (isRaining) {
    print('Take an umbrella!');
  } else {
    print('Enjoy the sunshine!');
  }
}
```

**If-else if-else chain:**
```dart
void main() {
  int score = 85;
  
  if (score >= 90) {
    print('Grade: A (Excellent!)');
  } else if (score >= 80) {
    print('Grade: B (Good job!)');
  } else if (score >= 70) {
    print('Grade: C (Average)');
  } else if (score >= 60) {
    print('Grade: D (Needs improvement)');
  } else {
    print('Grade: F (Failed)');
  }
  
  // Multiple conditions
  int temperature = 75;
  bool isSunny = true;
  
  if (temperature > 70 && isSunny) {
    print('Perfect weather for a picnic!');
  } else if (temperature > 70 && !isSunny) {
    print('Warm but cloudy day.');
  } else if (temperature <= 70 && isSunny) {
    print('Cool but sunny day.');
  } else {
    print('Cold and cloudy day.');
  }
}
```

**Nested if statements:**
```dart
void main() {
  bool hasLicense = true;
  int age = 20;
  bool hasInsurance = true;
  
  if (hasLicense) {
    if (age >= 18) {
      if (hasInsurance) {
        print('You can drive!');
      } else {
        print('You need insurance to drive.');
      }
    } else {
      print('You must be 18 or older to drive.');
    }
  } else {
    print('You need a valid license to drive.');
  }
}
```

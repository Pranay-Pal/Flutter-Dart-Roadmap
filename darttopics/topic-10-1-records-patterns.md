# Topic 10.1: Records & Patterns

    * [ ] Returning multiple values with Records
    * [ ] Destructuring data with patterns
    * [ ] Advanced logic with pattern matching (`switch`, `if-case`)

#### Records and Pattern Matching

```dart
// Function returning multiple values using records
(String, int, bool) getUserInfo(String userId) {
  // Simulate database lookup
  return ('John Doe', 25, true); // (name, age, isActive)
}

// Pattern matching with switch expressions
String categorizeAge(int age) => switch (age) {
  < 13 => 'Child',
  >= 13 && < 20 => 'Teenager',
  >= 20 && < 60 => 'Adult',
  _ => 'Senior'
};

void main() {
  // Record destructuring
  var (name, age, isActive) = getUserInfo('123');
  print('User: $name, Age: $age, Active: $isActive');
  
  // Pattern matching with switch
  List<dynamic> data = ['Alice', 30, true];
  
  switch (data) {
    case [String name, int age, bool active] when age >= 18:
      print('Adult user: $name');
    case [String name, int age, bool active]:
      print('Minor user: $name');
    default:
      print('Invalid data format');
  }
  
  print('Age category: ${categorizeAge(age)}');
}
```

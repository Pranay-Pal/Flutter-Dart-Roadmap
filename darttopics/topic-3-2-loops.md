# Topic 3.2: Loops

    * [ ] `for` (including `for-in`), `while`, and `do-while` loops
    * [ ] Using `break` and `continue`

#### For Loops

For loops are used when you know how many times you want to repeat a block of code.

**Traditional for loop:**
```dart
void main() {
  // Basic for loop
  print('Counting from 1 to 5:');
  for (int i = 1; i <= 5; i++) {
    print('Count: $i');
  }
  
  // Counting backwards
  print('\nCountdown:');
  for (int i = 5; i >= 1; i--) {
    print('$i...');
  }
  print('Blast off! 🚀');
  
  // Different increments
  print('\nEven numbers from 0 to 10:');
  for (int i = 0; i <= 10; i += 2) {
    print(i);
  }
}
```

**For-in loops (iterating over collections):**
```dart
void main() {
  // Iterating over a list
  List<String> fruits = ['apple', 'banana', 'orange', 'grape'];
  
  print('My favorite fruits:');
  for (String fruit in fruits) {
    print('- ${fruit.toUpperCase()}');
  }
  
  // Iterating over a set
  Set<int> numbers = {1, 2, 3, 4, 5};
  int sum = 0;
  
  for (int number in numbers) {
    sum += number;
  }
  print('Sum of numbers: $sum');
  
  // Iterating over map keys and values
  Map<String, int> ages = {
    'Alice': 25,
    'Bob': 30,
    'Charlie': 22
  };
  
  print('\nAges:');
  for (String name in ages.keys) {
    print('$name is ${ages[name]} years old');
  }
  
  // Alternative way to iterate over map entries
  for (MapEntry<String, int> entry in ages.entries) {
    print('${entry.key}: ${entry.value}');
  }
}
```

**Nested loops:**
```dart
void main() {
  // Multiplication table
  print('Multiplication Table (1-5):');
  for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
      print('$i x $j = ${i * j}');
    }
    print('---');
  }
  
  // Pattern printing
  print('\nPyramid pattern:');
  for (int i = 1; i <= 5; i++) {
    String spaces = ' ' * (5 - i);
    String stars = '*' * (2 * i - 1);
    print('$spaces$stars');
  }
}
```

#### While Loops

While loops continue executing as long as a condition is true.

**Basic while loop:**
```dart
void main() {
  int count = 1;
  
  print('While loop counting:');
  while (count <= 5) {
    print('Count: $count');
    count++; // Important: increment to avoid infinite loop
  }
  
  // User input simulation
  int attempts = 0;
  int maxAttempts = 3;
  bool success = false;
  
  while (attempts < maxAttempts && !success) {
    attempts++;
    print('Attempt $attempts');
    
    // Simulate some condition for success
    if (attempts == 2) {
      success = true;
      print('Success on attempt $attempts!');
    }
  }
  
  if (!success) {
    print('Failed after $maxAttempts attempts');
  }
}
```

#### Do-While Loops

Do-while loops execute the code block at least once before checking the condition.

```dart
void main() {
  // Menu system example
  int choice;
  
  do {
    print('\n=== Menu ===');
    print('1. View Profile');
    print('2. Settings');
    print('3. Exit');
    print('Enter your choice (1-3):');
    
    // Simulating user input
    choice = 2; // In real app, this would be user input
    
    switch (choice) {
      case 1:
        print('Viewing profile...');
        break;
      case 2:
        print('Opening settings...');
        break;
      case 3:
        print('Goodbye!');
        break;
      default:
        print('Invalid choice. Please try again.');
    }
    
    // For demo purposes, exit after one iteration
    choice = 3;
  } while (choice != 3);
  
  // Another example: input validation
  int number;
  do {
    print('Enter a number between 1 and 10:');
    number = 15; // Simulating invalid input first
    
    if (number < 1 || number > 10) {
      print('Invalid input. Please try again.');
      number = 5; // Simulating valid input on retry
    }
  } while (number < 1 || number > 10);
  
  print('Valid number entered: $number');
}
```

#### Using Break and Continue

Break and continue statements control loop execution flow.

**Break statement:**
```dart
void main() {
  print('Finding first even number greater than 10:');
  
  for (int i = 11; i <= 20; i++) {
    print('Checking: $i');
    
    if (i % 2 == 0) {
      print('Found first even number: $i');
      break; // Exit the loop
    }
  }
  
  // Break in nested loops
  print('\nSearching in nested loops:');
  bool found = false;
  
  for (int i = 1; i <= 3 && !found; i++) {
    for (int j = 1; j <= 3; j++) {
      print('Checking position ($i, $j)');
      
      if (i == 2 && j == 2) {
        print('Target found at ($i, $j)');
        found = true;
        break; // Only breaks inner loop
      }
    }
  }
}
```

**Continue statement:**
```dart
void main() {
  print('Printing odd numbers from 1 to 10:');
  
  for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
      continue; // Skip even numbers
    }
    print(i); // Only prints odd numbers
  }
  
  // Skip specific values
  print('\nProcessing list (skipping negative numbers):');
  List<int> numbers = [1, -2, 3, -4, 5, -6, 7];
  
  for (int number in numbers) {
    if (number < 0) {
      print('Skipping negative number: $number');
      continue;
    }
    
    print('Processing: $number');
    print('Square: ${number * number}');
  }
}
```

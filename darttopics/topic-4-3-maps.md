# Topic 4.3: Maps

[⬅ Previous](topic-4-2-sets.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-4-4-collection-tools.md)

    * [ ] Using key-value pairs to store data

#### Using Key-Value Pairs with Maps

Maps store data as key-value pairs, where each key is unique and maps to a specific value.

**Creating maps:**
```dart
void main() {
  // Creating maps with different approaches
  Map<String, int> ages = {
    'Alice': 25,
    'Bob': 30,
    'Charlie': 22
  };
  
  Map<String, double> prices = {
    'coffee': 4.50,
    'tea': 3.25,
    'sandwich': 8.99
  };
  
  // Creating empty maps
  Map<String, String> emptyMap = {};
  Map<String, String> emptyMap2 = <String, String>{};
  Map<String, String> emptyMap3 = Map<String, String>();
  
  // Creating map from iterables
  List<String> keys = ['a', 'b', 'c'];
  List<int> values = [1, 2, 3];
  Map<String, int> mapFromLists = Map.fromIterables(keys, values);
  
  print('Ages: $ages');
  print('Prices: $prices');
  print('Map from lists: $mapFromLists');
}
```

**Accessing and modifying map values:**
```dart
void main() {
  Map<String, String> countries = {
    'US': 'United States',
    'UK': 'United Kingdom',
    'CA': 'Canada',
    'AU': 'Australia'
  };
  
  print('Initial countries: $countries');
  
  // Accessing values
  print('US: ${countries['US']}');
  print('Non-existent key: ${countries['XX']}'); // Returns null
  
  // Safe access with default value
  String country = countries['XX'] ?? 'Unknown Country';
  print('Country with default: $country');
  
  // Adding new key-value pairs
  countries['IN'] = 'India';
  countries['JP'] = 'Japan';
  print('After adding: $countries');
  
  // Updating existing values
  countries['US'] = 'United States of America';
  print('After updating: $countries');
  
  // Adding multiple entries
  countries.addAll({
    'FR': 'France',
    'DE': 'Germany'
  });
  print('After adding multiple: $countries');
  
  // Removing entries
  String? removed = countries.remove('AU');
  print('Removed value: $removed');
  print('After removal: $countries');
}
```

**Map operations and methods:**
```dart
void main() {
  Map<String, int> inventory = {
    'apples': 50,
    'bananas': 30,
    'oranges': 25,
    'grapes': 15
  };
  
  print('Inventory: $inventory');
  
  // Map properties
  print('Number of items: ${inventory.length}');
  print('Is empty: ${inventory.isEmpty}');
  print('Keys: ${inventory.keys}');
  print('Values: ${inventory.values}');
  
  // Checking for keys and values
  print('Contains apples: ${inventory.containsKey('apples')}');
  print('Contains quantity 30: ${inventory.containsValue(30)}');
  
  // Iterating over map
  print('\nInventory details:');
  inventory.forEach((fruit, quantity) {
    print('$fruit: $quantity');
  });
  
  // Iterating with entries
  print('\nUsing entries:');
  for (MapEntry<String, int> entry in inventory.entries) {
    print('${entry.key}: ${entry.value}');
  }
  
  // Convert to lists
  List<String> fruits = inventory.keys.toList();
  List<int> quantities = inventory.values.toList();
  print('Fruits list: $fruits');
  print('Quantities list: $quantities');
}
```

**Advanced map operations:**
```dart
void main() {
  Map<String, double> grades = {
    'Alice': 92.5,
    'Bob': 87.0,
    'Charlie': 95.5,
    'Diana': 89.5,
    'Eve': 78.5
  };
  
  // Filtering maps
  Map<String, double> highGrades = Map.fromEntries(
    grades.entries.where((entry) => entry.value >= 90)
  );
  print('High grades (>=90): $highGrades');
  
  // Transforming values
  Map<String, String> letterGrades = grades.map(
    (name, grade) => MapEntry(name, getLetterGrade(grade))
  );
  print('Letter grades: $letterGrades');
  
  // Finding maximum and minimum
  double highestGrade = grades.values.reduce((a, b) => a > b ? a : b);
  double lowestGrade = grades.values.reduce((a, b) => a < b ? a : b);
  
  String topStudent = grades.entries
      .where((entry) => entry.value == highestGrade)
      .first
      .key;
  
  print('Highest grade: $highestGrade (by $topStudent)');
  print('Lowest grade: $lowestGrade');
  
  // Group by condition
  Map<String, List<String>> groupedStudents = {
    'excellent': [],
    'good': [],
    'average': []
  };
  
  grades.forEach((name, grade) {
    if (grade >= 90) {
      groupedStudents['excellent']!.add(name);
    } else if (grade >= 80) {
      groupedStudents['good']!.add(name);
    } else {
      groupedStudents['average']!.add(name);
    }
  });
  
  print('Grouped students: $groupedStudents');
}

String getLetterGrade(double grade) {
  if (grade >= 90) return 'A';
  if (grade >= 80) return 'B';
  if (grade >= 70) return 'C';
  if (grade >= 60) return 'D';
  return 'F';
}
```

**Nested maps and complex data structures:**
```dart
void main() {
  // Student database with nested information
  Map<String, Map<String, dynamic>> students = {
    'student1': {
      'name': 'Alice Johnson',
      'age': 20,
      'grades': [92, 88, 95, 87],
      'address': {
        'street': '123 Main St',
        'city': 'Boston',
        'zipCode': '02101'
      }
    },
    'student2': {
      'name': 'Bob Smith',
      'age': 19,
      'grades': [78, 82, 85, 80],
      'address': {
        'street': '456 Oak Ave',
        'city': 'New York',
        'zipCode': '10001'
      }
    }
  };
  
  // Accessing nested data
  String studentName = students['student1']!['name'];
  List<int> grades = students['student1']!['grades'];
  String city = students['student1']!['address']['city'];
  
  print('Student: $studentName');
  print('Grades: $grades');
  print('City: $city');
  
  // Calculate average grade for each student
  students.forEach((id, studentData) {
    List<int> studentGrades = studentData['grades'];
    double average = studentGrades.reduce((a, b) => a + b) / studentGrades.length;
    print('${studentData['name']} average: ${average.toStringAsFixed(1)}');
  });
}
```

### **Module 4: Collections (Grouping Data)**

[⬅ Previous](topic-4-2-sets.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-4-4-collection-tools.md)

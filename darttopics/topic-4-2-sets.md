# Topic 4.2: Sets

    * [ ] Working with unordered collections of unique items

#### Working with Sets

Sets are unordered collections that contain only unique elements. They're perfect when you need to ensure no duplicates.

**Creating sets:**
```dart
void main() {
  // Creating sets with different approaches
  Set<String> colors = {'red', 'green', 'blue'};
  Set<int> numbers = {1, 2, 3, 4, 5};
  
  // Creating empty sets
  Set<String> emptySet = <String>{};
  Set<String> emptySet2 = Set<String>();
  
  // Creating set from list (removes duplicates)
  List<int> duplicateNumbers = [1, 2, 2, 3, 3, 3, 4, 5];
  Set<int> uniqueNumbers = duplicateNumbers.toSet();
  
  print('Colors: $colors');
  print('Numbers: $numbers');
  print('Unique numbers: $uniqueNumbers');
  print('Empty set: $emptySet');
}
```

**Set operations:**
```dart
void main() {
  Set<String> fruits = {'apple', 'banana', 'orange'};
  
  print('Initial fruits: $fruits');
  
  // Adding elements
  fruits.add('grape');
  fruits.addAll(['kiwi', 'mango']);
  print('After adding: $fruits');
  
  // Try adding duplicate (won't be added)
  bool added = fruits.add('apple'); // Returns false
  print('Added apple again: $added');
  print('Fruits after duplicate add: $fruits');
  
  // Removing elements
  fruits.remove('banana');
  print('After removing banana: $fruits');
  
  // Set properties
  print('Number of fruits: ${fruits.length}');
  print('Contains apple: ${fruits.contains('apple')}');
  print('Is empty: ${fruits.isEmpty}');
}
```

**Set mathematical operations:**
```dart
void main() {
  Set<int> setA = {1, 2, 3, 4, 5};
  Set<int> setB = {4, 5, 6, 7, 8};
  
  print('Set A: $setA');
  print('Set B: $setB');
  
  // Union (all elements from both sets)
  Set<int> union = setA.union(setB);
  print('Union: $union');
  
  // Intersection (common elements)
  Set<int> intersection = setA.intersection(setB);
  print('Intersection: $intersection');
  
  // Difference (elements in A but not in B)
  Set<int> difference = setA.difference(setB);
  print('Difference (A - B): $difference');
  
  // Symmetric difference (elements in either set, but not both)
  Set<int> symmetricDifference = setA.union(setB).difference(setA.intersection(setB));
  print('Symmetric difference: $symmetricDifference');
  
  // Check subset/superset relationships
  Set<int> subset = {2, 3};
  print('Is {2, 3} subset of A: ${subset.every((element) => setA.contains(element))}');
}
```

**Practical set examples:**
```dart
void main() {
  // User permissions system
  Set<String> adminPermissions = {'read', 'write', 'delete', 'admin'};
  Set<String> userPermissions = {'read', 'write'};
  Set<String> guestPermissions = {'read'};
  
  print('Admin permissions: $adminPermissions');
  print('User permissions: $userPermissions');
  print('Guest permissions: $guestPermissions');
  
  // Check if user can delete
  bool canDelete = userPermissions.contains('delete');
  print('User can delete: $canDelete');
  
  // Find what additional permissions user needs to become admin
  Set<String> additionalNeeded = adminPermissions.difference(userPermissions);
  print('Additional permissions needed: $additionalNeeded');
  
  // Unique visitors tracking
  Set<String> dailyVisitors = {'user1', 'user2', 'user3'};
  Set<String> weeklyVisitors = {'user1', 'user4', 'user5'};
  
  // All unique visitors this week
  Set<String> allVisitors = dailyVisitors.union(weeklyVisitors);
  print('All unique visitors: $allVisitors');
  
  // Returning visitors
  Set<String> returningVisitors = dailyVisitors.intersection(weeklyVisitors);
  print('Returning visitors: $returningVisitors');
}
```

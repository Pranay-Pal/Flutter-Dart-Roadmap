# Topic 8.1: Core Concepts

    * [ ] Understanding the event loop
    * [ ] Working with `Future` for one-time async results
    * [ ] Using `async` and `await` for clean code

#### Understanding Futures and Async/Await

```dart
import 'dart:async';
import 'dart:math';

// Simulate network delay
Future<String> fetchUserData(String userId) async {
  print('Fetching user data for $userId...');
  await Future.delayed(Duration(seconds: 2));
  
  if (Random().nextBool()) {
    return 'User data for $userId';
  } else {
    throw Exception('Failed to fetch user data');
  }
}

Future<List<String>> fetchMultipleUsers(List<String> userIds) async {
  List<String> results = [];
  
  for (String id in userIds) {
    try {
      String userData = await fetchUserData(id);
      results.add(userData);
    } catch (e) {
      results.add('Error: $e');
    }
  }
  
  return results;
}

void main() async {
  print('Starting async operations...');
  
  try {
    // Single async operation
    String user = await fetchUserData('123');
    print('Received: $user');
    
    // Multiple async operations
    List<String> users = await fetchMultipleUsers(['1', '2', '3']);
    users.forEach(print);
    
  } catch (e) {
    print('Error: $e');
  }
  
  print('Program completed');
}
```

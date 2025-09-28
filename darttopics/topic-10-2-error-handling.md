# Topic 10.2: Error Handling

[⬅ Previous](topic-10-1-records-patterns.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-11-1-libraries-and-packages.md)

    * [ ] Managing exceptions with `try`, `catch`, `finally`, and `throw`

#### Exception Handling

```dart
class CustomException implements Exception {
  final String message;
  CustomException(this.message);
  
  @override
  String toString() => 'CustomException: $message';
}

int divide(int a, int b) {
  if (b == 0) {
    throw CustomException('Division by zero is not allowed');
  }
  return a ~/ b;
}

void main() {
  List<List<int>> testCases = [
    [10, 2],
    [15, 3],
    [8, 0], // This will throw an exception
    [20, 4]
  ];
  
  for (var testCase in testCases) {
    try {
      int result = divide(testCase[0], testCase[1]);
      print('${testCase[0]} ÷ ${testCase[1]} = $result');
    } on CustomException catch (e) {
      print('Custom error: $e');
    } catch (e) {
      print('Unexpected error: $e');
    } finally {
      print('Division operation completed');
    }
  }
}
```

#### Handling Errors in Asynchronous Code

```dart
import 'dart:async';

Future<String> riskyNetworkCall() async {
  await Future.delayed(const Duration(milliseconds: 300));
  throw TimeoutException('Server took too long to respond');
}

Future<void> main() async {
  try {
    final response = await riskyNetworkCall();
    print('Response: $response');
  } on TimeoutException catch (e, stackTrace) {
    print('Timeout: ${e.message}');
    print('Stack trace snippet: ${stackTrace.toString().split('\n').first}');
  } catch (e) {
    print('Unexpected async error: $e');
    rethrow; // Bubble up if you cannot handle it here
  } finally {
    print('Cleanup async resources');
  }
}
```

### **Module 11: The Dart Ecosystem**

[⬅ Previous](topic-10-1-records-patterns.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-11-1-libraries-and-packages.md)

# Topic 10.2: Error Handling

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

### **Module 11: The Dart Ecosystem**

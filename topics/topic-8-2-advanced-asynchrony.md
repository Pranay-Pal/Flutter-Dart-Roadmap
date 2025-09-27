# Topic 8.2: Advanced Asynchrony

    * [ ] Handling data sequences with `Stream`
    * [ ] True parallelism with `Isolates`
    * [ ] Lazy value generation with `sync*`/`async*` (Generators)

#### Streams and Generators

```dart
import 'dart:async';

// Stream generator
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(milliseconds: 500));
    yield i;
  }
}

// Sync generator
Iterable<int> fibonacci(int n) sync* {
  int a = 0, b = 1;
  for (int i = 0; i < n; i++) {
    yield a;
    int temp = a + b;
    a = b;
    b = temp;
  }
}

void main() async {
  print('Stream example:');
  await for (int value in countStream(5)) {
    print('Received: $value');
  }
  
  print('\nFibonacci sequence:');
  for (int value in fibonacci(10)) {
    print(value);
  }
}
```

### **Module 9: Null Safety**

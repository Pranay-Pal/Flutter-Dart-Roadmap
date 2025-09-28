# Topic 5.2: Parameter Types

[⬅ Previous](topic-5-1-function-basics.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-5-3-advanced-function-concepts.md)

    * [ ] Positional, named, optional, and required parameters

#### Parameter Types in Dart

Dart provides flexible ways to define function parameters to make your functions more versatile and easier to use.

**Positional parameters:**
```dart
void main() {
  // Required positional parameters - must be provided in order
  String greeting = createGreeting('Hello', 'Alice');
  print(greeting);
  
  // Order matters with positional parameters
  int result1 = subtract(10, 3); // 10 - 3 = 7
  int result2 = subtract(3, 10); // 3 - 10 = -7
  print('10 - 3 = $result1');
  print('3 - 10 = $result2');
  
  // Function with multiple positional parameters
  displayPersonInfo('John', 25, 'Engineer');
}

String createGreeting(String greeting, String name) {
  return '$greeting, $name!';
}

int subtract(int a, int b) {
  return a - b;
}

void displayPersonInfo(String name, int age, String profession) {
  print('Name: $name, Age: $age, Profession: $profession');
}
```

**Optional positional parameters:**
```dart
void main() {
  // Optional positional parameters are enclosed in square brackets
  printUserInfo('Alice'); // Only required parameter
  printUserInfo('Bob', 30); // Required + one optional
  printUserInfo('Charlie', 25, 'Engineer'); // All parameters
  
  // Default values for optional parameters
  String message1 = formatMessage('Hello');
  String message2 = formatMessage('Hello', 'World');
  String message3 = formatMessage('Hello', 'World', '!');
  
  print(message1); // "Hello there"
  print(message2); // "Hello World"
  print(message3); // "Hello World!"
}

void printUserInfo(String name, [int? age, String? profession]) {
  print('Name: $name');
  if (age != null) {
    print('Age: $age');
  }
  if (profession != null) {
    print('Profession: $profession');
  }
  print('---');
}

String formatMessage(String greeting, [String target = 'there', String punctuation = '']) {
  return '$greeting $target$punctuation';
}
```

**Named parameters:**
```dart
void main() {
  // Named parameters make function calls more readable
  createUser(name: 'Alice', email: 'alice@example.com');
  
  // Order doesn't matter with named parameters
  createUser(email: 'bob@example.com', name: 'Bob');
  
  // Optional named parameters
  createUser(name: 'Charlie', email: 'charlie@example.com', age: 25);
  
  // With default values
  displayMessage(text: 'Hello World');
  displayMessage(text: 'Important!', isUrgent: true);
  displayMessage(text: 'Debug info', isUrgent: false, includeTimestamp: true);
}

void createUser({required String name, required String email, int? age, String role = 'user'}) {
  print('Creating user:');
  print('  Name: $name');
  print('  Email: $email');
  if (age != null) {
    print('  Age: $age');
  }
  print('  Role: $role');
  print('---');
}

void displayMessage({
  required String text,
  bool isUrgent = false,
  bool includeTimestamp = false,
}) {
  String message = text;
  
  if (isUrgent) {
    message = '🚨 URGENT: $message';
  }
  
  if (includeTimestamp) {
    message = '${DateTime.now()}: $message';
  }
  
  print(message);
}
```

**Mixing positional and named parameters:**
```dart
void main() {
  // Positional parameters come first, then named parameters
  processOrder('12345', 'laptop', quantity: 2);
  processOrder('12346', 'phone', quantity: 1, priority: 'high');
  
  // With optional positional and named parameters
  analyzeData([1, 2, 3, 4, 5]);
  analyzeData([1, 2, 3, 4, 5], 'mean');
  analyzeData([1, 2, 3, 4, 5], 'median', includeDetails: true);
}

void processOrder(String orderId, String item, {required int quantity, String priority = 'normal'}) {
  print('Processing Order:');
  print('  ID: $orderId');
  print('  Item: $item');
  print('  Quantity: $quantity');
  print('  Priority: $priority');
  print('---');
}

void analyzeData(List<int> data, [String method = 'sum'], {bool includeDetails = false}) {
  print('Analyzing data: $data');
  print('Method: $method');
  
  switch (method) {
    case 'sum':
      int sum = data.reduce((a, b) => a + b);
      print('Sum: $sum');
      break;
    case 'mean':
      double mean = data.reduce((a, b) => a + b) / data.length;
      print('Mean: ${mean.toStringAsFixed(2)}');
      break;
    case 'median':
      List<int> sorted = [...data]..sort();
      double median = sorted.length % 2 == 0
          ? (sorted[sorted.length ~/ 2 - 1] + sorted[sorted.length ~/ 2]) / 2
          : sorted[sorted.length ~/ 2].toDouble();
      print('Median: $median');
      break;
  }
  
  if (includeDetails) {
    print('Data length: ${data.length}');
    print('Min: ${data.reduce((a, b) => a < b ? a : b)}');
    print('Max: ${data.reduce((a, b) => a > b ? a : b)}');
  }
  print('---');
}
```

**Real-world parameter examples:**
```dart
void main() {
  // API function with various parameter types
  String response1 = makeApiCall('/users');
  String response2 = makeApiCall('/users', method: 'POST');
  String response3 = makeApiCall(
    '/users/123',
    method: 'PUT',
    headers: {'Authorization': 'Bearer token123'},
    timeout: 5000,
  );
  
  print('Response 1: $response1');
  print('Response 2: $response2');
  print('Response 3: $response3');
  
  // Configuration function
  startServer();
  startServer(8080);
  startServer(3000, host: '0.0.0.0', enableLogging: true);
}

String makeApiCall(
  String endpoint, {
  String method = 'GET',
  Map<String, String>? headers,
  int timeout = 3000,
  Map<String, dynamic>? body,
}) {
  // Simulate API call
  String result = 'API Call:\n';
  result += '  Endpoint: $endpoint\n';
  result += '  Method: $method\n';
  result += '  Timeout: ${timeout}ms\n';
  
  if (headers != null) {
    result += '  Headers: $headers\n';
  }
  
  if (body != null) {
    result += '  Body: $body\n';
  }
  
  return result;
}

void startServer([int port = 8080], {String host = 'localhost', bool enableLogging = false}) {
  print('Starting server...');
  print('  Host: $host');
  print('  Port: $port');
  print('  Logging: ${enableLogging ? 'enabled' : 'disabled'}');
  print('---');
}
```

### **Module 5: Functions (Reusable Code)**

[⬅ Previous](topic-5-1-function-basics.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-5-3-advanced-function-concepts.md)

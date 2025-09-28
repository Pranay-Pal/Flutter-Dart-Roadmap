# Topic 5.3: Advanced Function Concepts

    * [ ] Arrow syntax (`=>`), anonymous functions, and lexical scope
    * [ ] Using `typedef` for function aliases

#### Arrow Syntax (`=>`)

Arrow functions provide a concise way to write simple functions that contain only one expression.

```dart
void main() {
  // Traditional function vs arrow function
  print('Traditional: ${addTraditional(5, 3)}');
  print('Arrow: ${addArrow(5, 3)}');
  
  // Arrow functions with different operations
  print('Square of 7: ${square(7)}');
  print('Is even 8: ${isEven(8)}');
  print('Is even 7: ${isEven(7)}');
  
  // Arrow functions with string operations
  print('Greeting: ${greet('Alice')}');
  print('Uppercase: ${toUpper('hello world')}');
  
  // Arrow functions with collections
  List<int> numbers = [1, 2, 3, 4, 5];
  List<int> doubled = numbers.map(doubleValue).toList();
  List<int> evens = numbers.where(isEven).toList();
  
  print('Original: $numbers');
  print('Doubled: $doubled');
  print('Evens: $evens');
}

// Traditional function
int addTraditional(int a, int b) {
  return a + b;
}

// Arrow function - equivalent to above
int addArrow(int a, int b) => a + b;

// More arrow function examples
int square(int n) => n * n;
bool isEven(int n) => n % 2 == 0;
String greet(String name) => 'Hello, $name!';
String toUpper(String text) => text.toUpperCase();
int doubleValue(int n) => n * 2;

// Arrow functions with more complex expressions
String formatPrice(double price) => '\$${price.toStringAsFixed(2)}';
bool isValidEmail(String email) => email.contains('@') && email.contains('.');
int getMax(int a, int b) => a > b ? a : b;
```

#### Anonymous Functions (Lambda Functions)

Anonymous functions are functions without a name, often used as arguments to other functions.

```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  
  // Anonymous function with traditional syntax
  List<int> squares = numbers.map((int n) {
    return n * n;
  }).toList();
  print('Squares: $squares');
  
  // Anonymous function with arrow syntax
  List<int> cubes = numbers.map((n) => n * n * n).toList();
  print('Cubes: $cubes');
  
  // Filtering with anonymous functions
  List<int> evens = numbers.where((n) => n % 2 == 0).toList();
  List<int> largeNumbers = numbers.where((n) => n > 5).toList();
  
  print('Even numbers: $evens');
  print('Numbers > 5: $largeNumbers');
  
  // forEach with anonymous function
  print('Printing with forEach:');
  numbers.where((n) => n <= 5).forEach((n) {
    print('  Number: $n, Square: ${n * n}');
  });
  
  // Sorting with anonymous functions
  List<String> names = ['Charlie', 'Alice', 'Bob', 'Diana'];
  
  // Sort by length
  List<String> sortedByLength = [...names];
  sortedByLength.sort((a, b) => a.length.compareTo(b.length));
  print('Sorted by length: $sortedByLength');
  
  // Sort by last letter
  List<String> sortedByLastLetter = [...names];
  sortedByLastLetter.sort((a, b) => a[a.length - 1].compareTo(b[b.length - 1]));
  print('Sorted by last letter: $sortedByLastLetter');
}
```

**Anonymous functions in practice:**
```dart
void main() {
  // Using anonymous functions for event handling simulation
  List<String> events = ['click', 'hover', 'focus', 'blur'];
  
  // Map events to handlers
  Map<String, Function> eventHandlers = {
    for (String event in events)
      event: () => print('Handling $event event')
  };
  
  // Execute handlers
  eventHandlers.forEach((event, handler) {
    print('Event: $event');
    handler();
  });
  
  // Complex data processing with anonymous functions
  List<Map<String, dynamic>> products = [
    {'name': 'Laptop', 'price': 999.99, 'category': 'Electronics'},
    {'name': 'Book', 'price': 15.99, 'category': 'Education'},
    {'name': 'Phone', 'price': 599.99, 'category': 'Electronics'},
    {'name': 'Desk', 'price': 299.99, 'category': 'Furniture'},
  ];
  
  // Filter expensive electronics
  List<Map<String, dynamic>> expensiveElectronics = products
      .where((product) => 
          product['category'] == 'Electronics' && product['price'] > 500)
      .toList();
  
  print('Expensive electronics:');
  expensiveElectronics.forEach((product) => 
      print('  ${product['name']}: \$${product['price']}'));
  
  // Calculate total by category
  Map<String, double> categoryTotals = {};
  products.forEach((product) {
    String category = product['category'];
    double price = product['price'];
    categoryTotals[category] = (categoryTotals[category] ?? 0) + price;
  });
  
  print('Category totals: $categoryTotals');
}
```

#### Lexical Scope

Lexical scope means that functions have access to variables in their defining scope.

```dart
// Global variable
String globalMessage = 'I am global';

void main() {
  String mainMessage = 'I am in main';
  
  // Function defined in main scope
  void innerFunction() {
    String innerMessage = 'I am inner';
    print('From inner function:');
    print('  $innerMessage');
    print('  $mainMessage'); // Can access main's variable
    print('  $globalMessage'); // Can access global variable
  }
  
  innerFunction();
  
  // Closures - functions that capture variables from outer scope
  Function makeMultiplier(int multiplier) {
    // This function "closes over" the multiplier parameter
    return (int value) => value * multiplier;
  }
  
  Function multiplyByTwo = makeMultiplier(2);
  Function multiplyByTen = makeMultiplier(10);
  
  print('5 * 2 = ${multiplyByTwo(5)}');
  print('5 * 10 = ${multiplyByTen(5)}');
  
  // Practical closure example - counter
  Function createCounter() {
    int count = 0; // This variable is captured by the closure
    
    return () {
      count++;
      return count;
    };
  }
  
  Function counter1 = createCounter();
  Function counter2 = createCounter();
  
  print('Counter 1: ${counter1()}'); // 1
  print('Counter 1: ${counter1()}'); // 2
  print('Counter 2: ${counter2()}'); // 1 (separate instance)
  print('Counter 1: ${counter1()}'); // 3
}
```

**Advanced closure examples:**
```dart
void main() {
  // Configuration closure
  Function createApiClient(String baseUrl, String apiKey) {
    return (String endpoint) {
      return 'Calling $baseUrl$endpoint with API key: ${apiKey.substring(0, 3)}...';
    };
  }
  
  Function userApi = createApiClient('https://api.example.com', 'key123456');
  print(userApi('/users'));
  print(userApi('/posts'));
  
  // Accumulator closure
  Function createAccumulator(int initialValue) {
    int sum = initialValue;
    
    return (int value) {
      sum += value;
      return sum;
    };
  }
  
  Function accumulator = createAccumulator(100);
  print('Accumulator: ${accumulator(10)}'); // 110
  print('Accumulator: ${accumulator(5)}');  // 115
  print('Accumulator: ${accumulator(-3)}'); // 112
  
  // Memoization closure
  Function memoize(Function fn) {
    Map<String, dynamic> cache = {};
    
    return (dynamic arg) {
      String key = arg.toString();
      if (cache.containsKey(key)) {
        print('Cache hit for $key');
        return cache[key];
      }
      
      dynamic result = fn(arg);
      cache[key] = result;
      print('Computed and cached result for $key');
      return result;
    };
  }
  
  Function expensiveCalculation = (int n) {
    // Simulate expensive operation
    int result = 1;
    for (int i = 1; i <= n; i++) {
      result *= i;
    }
    return result;
  };
  
  Function memoizedFactorial = memoize(expensiveCalculation);
  
  print('Factorial 5: ${memoizedFactorial(5)}');
  print('Factorial 5: ${memoizedFactorial(5)}'); // Should use cache
  print('Factorial 6: ${memoizedFactorial(6)}');
}
```

#### Using `typedef` for Function Aliases

`typedef` allows you to create type aliases for functions, making code more readable and maintainable.

```dart
// Define function type aliases
typedef Calculator = int Function(int a, int b);
typedef StringProcessor = String Function(String input);
typedef BoolValidator = bool Function(String value);
typedef EventHandler = void Function(String eventType, Map<String, dynamic> data);

void main() {
  // Using typedef with different function implementations
  Calculator add = (a, b) => a + b;
  Calculator multiply = (a, b) => a * b;
  Calculator subtract = (a, b) => a - b;
  
  print('Add: ${performCalculation(add, 5, 3)}');
  print('Multiply: ${performCalculation(multiply, 5, 3)}');
  print('Subtract: ${performCalculation(subtract, 5, 3)}');
  
  // String processors
  StringProcessor upperCase = (s) => s.toUpperCase();
  StringProcessor addPrefix = (s) => 'PREFIX: $s';
  StringProcessor reverse = (s) => s.split('').reversed.join('');
  
  String text = 'hello world';
  print('Original: $text');
  print('Upper: ${processString(upperCase, text)}');
  print('Prefixed: ${processString(addPrefix, text)}');
  print('Reversed: ${processString(reverse, text)}');
  
  // Validators
  BoolValidator emailValidator = (email) => email.contains('@') && email.contains('.');
  BoolValidator phoneValidator = (phone) => RegExp(r'^\d{10}$').hasMatch(phone);
  BoolValidator notEmptyValidator = (value) => value.trim().isNotEmpty;
  
  print('Email validation: ${validateInput(emailValidator, 'user@example.com')}');
  print('Phone validation: ${validateInput(phoneValidator, '1234567890')}');
  print('Not empty validation: ${validateInput(notEmptyValidator, 'hello')}');
  
  // Event handling system
  EventHandler logHandler = (type, data) => print('LOG: $type - $data');
  EventHandler analyticsHandler = (type, data) => print('ANALYTICS: Tracking $type');
  
  handleEvent(logHandler, 'user_click', {'button': 'submit', 'page': 'login'});
  handleEvent(analyticsHandler, 'page_view', {'page': 'dashboard'});
}

int performCalculation(Calculator calc, int a, int b) {
  return calc(a, b);
}

String processString(StringProcessor processor, String input) {
  return processor(input);
}

bool validateInput(BoolValidator validator, String input) {
  return validator(input);
}

void handleEvent(EventHandler handler, String eventType, Map<String, dynamic> data) {
  handler(eventType, data);
}
```

**Advanced typedef usage:**
```dart
// Complex typedef examples
typedef AsyncDataProcessor<T> = Future<T> Function(T data);
typedef ListTransformer<T, R> = List<R> Function(List<T> items);
typedef ConditionalAction = void Function() Function(bool condition);

// Generic typedef
typedef Mapper<T, R> = R Function(T input);
typedef Predicate<T> = bool Function(T input);
typedef Consumer<T> = void Function(T input);

void main() {
  // Generic mappers
  Mapper<int, String> intToString = (n) => 'Number: $n';
  Mapper<String, int> stringLength = (s) => s.length;
  Mapper<double, int> doubleToInt = (d) => d.round();
  
  print('Int to string: ${intToString(42)}');
  print('String length: ${stringLength('Hello')}');
  print('Double to int: ${doubleToInt(3.7)}');
  
  // List transformers
  ListTransformer<int, String> numbersToStrings = (numbers) =>
      numbers.map((n) => 'Item $n').toList();
  
  ListTransformer<String, int> stringsToLengths = (strings) =>
      strings.map((s) => s.length).toList();
  
  List<int> numbers = [1, 2, 3, 4, 5];
  List<String> words = ['hello', 'world', 'dart'];
  
  print('Numbers to strings: ${numbersToStrings(numbers)}');
  print('Strings to lengths: ${stringsToLengths(words)}');
  
  // Conditional actions
  ConditionalAction createConditionalGreeting(String name) {
    return (bool shouldGreet) {
      return shouldGreet ? () => print('Hello, $name!') : () => print('...');
    };
  }
  
  ConditionalAction greetAlice = createConditionalGreeting('Alice');
  greetAlice(true)();  // Hello, Alice!
  greetAlice(false)(); // ...
  
  // Practical application: Plugin system
  demonstratePluginSystem();
}

// Plugin system demonstration using typedef
typedef PluginAction = void Function(Map<String, dynamic> context);
typedef PluginFilter = bool Function(String pluginName, Map<String, dynamic> context);

void demonstratePluginSystem() {
  // Define plugins
  Map<String, PluginAction> plugins = {
    'logger': (context) => print('LOG: ${context['message']}'),
    'validator': (context) => print('VALIDATE: ${context['data']}'),
    'formatter': (context) => print('FORMAT: ${context['text']?.toUpperCase()}'),
  };
  
  // Plugin filters
  PluginFilter devOnlyFilter = (name, context) => 
      name.startsWith('dev_') || context['environment'] == 'development';
  
  PluginFilter userRoleFilter = (name, context) =>
      context['userRole'] == 'admin' || !name.contains('admin');
  
  // Execute plugins with filtering
  Map<String, dynamic> context = {
    'message': 'User logged in',
    'data': {'user': 'john@example.com'},
    'text': 'hello world',
    'environment': 'production',
    'userRole': 'user',
  };
  
  executePlugins(plugins, context, [userRoleFilter]);
}

void executePlugins(
  Map<String, PluginAction> plugins,
  Map<String, dynamic> context,
  List<PluginFilter> filters,
) {
  plugins.forEach((name, action) {
    bool shouldExecute = filters.every((filter) => filter(name, context));
    
    if (shouldExecute) {
      print('Executing plugin: $name');
      action(context);
    } else {
      print('Skipping plugin: $name (filtered out)');
    }
  });
}
```

### **Module 6: Object-Oriented Programming (OOP) - Part 1**

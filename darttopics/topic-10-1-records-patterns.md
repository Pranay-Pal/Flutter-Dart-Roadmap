# Topic 10.1: Records & Patterns

[⬅ Previous](topic-9-2-practical-application.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-10-2-error-handling.md)

    * [ ] Returning multiple values with Records
    * [ ] Destructuring data with patterns
    * [ ] Advanced logic with pattern matching (`switch`, `if-case`)

#### Returning Multiple Values with Records

```dart
// Function returning multiple values using a positional record
(String name, int age, bool isActive) getUserInfo(String userId) {
  // Simulate database lookup
  return ('John Doe', 25, true);
}

// Named field records make call sites self-documenting
({String id, String role}) assignRole(String username) {
  return (id: 'user_${username.toLowerCase()}', role: 'admin');
}

void main() {
  final (name, age, active) = getUserInfo('123');
  print('User: $name, Age: $age, Active: $active');

  final userRole = assignRole('Alice');
  print('Assigned ${userRole.role} to ${userRole.id}');
}
```

Records let you return multiple strongly-typed values without creating a custom class or tuple-like helper.

#### Destructuring Data with Patterns

```dart
void logCoordinate(Object input) {
  if (input case (int x, int y)) {
    print('Integer point at ($x, $y)');
  } else if (input case (double x, double y)) {
    print('Double precision point at ($x, $y)');
  } else if (input case {'lat': double lat, 'lng': double lng}) {
    print('Map-based coordinate → ($lat, $lng)');
  } else {
    print('Unknown coordinate format: $input');
  }
}

void main() {
  logCoordinate((10, 20));
  logCoordinate((3.14, 2.71));
  logCoordinate({'lat': 51.5074, 'lng': -0.1278});

  final users = [
    ('Alice', 28, isAdmin: true),
    ('Bob', 16, isAdmin: false),
    ('Charlie', 35, isAdmin: false),
  ];

  for (final (name, age, :isAdmin) in users) {
    final label = isAdmin ? 'Admin' : 'User';
    print('$label $name → age $age');
  }
}
```

Patterns can destructure positional records, named records, maps, and even nested structures right inside control flow statements.

#### Advanced Logic with Pattern Matching (`switch`, `if-case`)

```dart
String categorizeAge(int age) => switch (age) {
  < 0 => throw ArgumentError('Age cannot be negative'),
  < 13 => 'Child',
  >= 13 && < 20 => 'Teenager',
  >= 20 && < 60 => 'Adult',
  _ => 'Senior',
};

void handleMessage(Map<String, Object?> message) {
  switch (message) {
    case {'type': 'login', 'user': String user}:
      print('User $user logged in');
    case {'type': 'logout', 'user': String user}:
      print('User $user logged out');
    case {'type': 'error', 'code': int code, 'message': String text}:
      print('Error $code → $text');
    default:
      print('Unhandled message: $message');
  }
}

void main() {
  print('Age 42 category: ${categorizeAge(42)}');

  const messages = [
    {'type': 'login', 'user': 'alice'},
    {'type': 'error', 'code': 500, 'message': 'Server down'},
    {'type': 'logout', 'user': 'alice'},
  ];

  for (final message in messages) {
    handleMessage(message);
  }

  // if-case combines conditionals with pattern matching
  final response = ('ok', 200);
  if (response case ('ok', int status) when status == 200) {
    print('Request succeeded with status $status');
  }
}
```

### **Module 10: Modern Dart Features**

[⬅ Previous](topic-9-2-practical-application.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-10-2-error-handling.md)

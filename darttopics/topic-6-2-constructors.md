# Topic 6.2: Constructors

[⬅ Previous](topic-6-1-classes-and-objects.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-6-3-inheritance.md)

    * [ ] Default, named, constant, and factory constructors

#### Constructors in Dart

Constructors are special methods used to create and initialize objects. Dart provides several types of constructors.

**Default and basic constructors:**
```dart
class Rectangle {
  double width;
  double height;
  
  // Default constructor
  Rectangle(this.width, this.height);
  
  // Constructor with parameter validation
  Rectangle.withValidation(double width, double height) {
    if (width <= 0 || height <= 0) {
      throw ArgumentError('Width and height must be positive');
    }
    this.width = width;
    this.height = height;
  }
  
  double get area => width * height;
  double get perimeter => 2 * (width + height);
  
  void display() {
    print('Rectangle: ${width}x$height, Area: $area, Perimeter: $perimeter');
  }
}

void main() {
  // Using default constructor
  Rectangle rect1 = Rectangle(5.0, 3.0);
  rect1.display();
  
  // Using constructor with validation
  try {
    Rectangle rect2 = Rectangle.withValidation(4.0, 6.0);
    rect2.display();
    
    Rectangle rect3 = Rectangle.withValidation(-2.0, 3.0); // This will throw an error
  } catch (e) {
    print('Error: $e');
  }
}
```

**Named constructors:**
```dart
import 'dart:math';

class Point {
  double x;
  double y;
  
  // Default constructor
  Point(this.x, this.y);
  
  // Named constructor for origin point
  Point.origin() {
    x = 0;
    y = 0;
  }
  
  // Named constructor from another point
  Point.fromPoint(Point other) {
    x = other.x;
    y = other.y;
  }
  
  // Named constructor from coordinates
  Point.fromCoordinates(double x, double y) : this(x, y);
  
  // Named constructor for point on x-axis
  Point.onXAxis(double x) : this(x, 0);
  
  // Named constructor for point on y-axis  
  Point.onYAxis(double y) : this(0, y);
  
  double distanceFromOrigin() {
    return sqrt(x * x + y * y);
  }
  
  void display() {
    print('Point($x, $y)');
  }
}

void main() {
  // Using different constructors
  Point p1 = Point(3.0, 4.0);           // Default constructor
  Point p2 = Point.origin();            // Named constructor
  Point p3 = Point.fromPoint(p1);       // Copy constructor
  Point p4 = Point.onXAxis(5.0);        // Point on x-axis
  Point p5 = Point.onYAxis(-3.0);       // Point on y-axis
  
  print('Points created:');
  p1.display();
  p2.display(); 
  p3.display();
  p4.display();
  p5.display();
  
  print('Distance from origin: ${p1.distanceFromOrigin().toStringAsFixed(2)}');
}
```

**Constant constructors:**
```dart
class ImmutablePoint {
  final double x;
  final double y;
  
  // Constant constructor - all fields must be final
  const ImmutablePoint(this.x, this.y);
  
  // Named constant constructor
  const ImmutablePoint.origin() : x = 0, y = 0;
  
  double get distanceFromOrigin => x * x + y * y;
  
  @override
  String toString() => 'ImmutablePoint($x, $y)';
}

class Color {
  final int red;
  final int green;
  final int blue;
  
  const Color(this.red, this.green, this.blue);
  
  // Named constant constructors for common colors
  const Color.black() : red = 0, green = 0, blue = 0;
  const Color.white() : red = 255, green = 255, blue = 255;
  const Color.red() : red = 255, green = 0, blue = 0;
  const Color.green() : red = 0, green = 255, blue = 0;
  const Color.blue() : red = 0, green = 0, blue = 255;
  
  @override
  String toString() => 'Color(r:$red, g:$green, b:$blue)';
}

void main() {
  // Constant objects - created at compile time
  const ImmutablePoint origin = ImmutablePoint.origin();
  const ImmutablePoint point = ImmutablePoint(3, 4);
  
  // Constant colors
  const Color redColor = Color.red();
  const Color customColor = Color(128, 64, 192);
  
  print('Origin: $origin');
  print('Point: $point');
  print('Red color: $redColor');
  print('Custom color: $customColor');
  
  // Comparing constant objects
  const ImmutablePoint p1 = ImmutablePoint(1, 2);
  const ImmutablePoint p2 = ImmutablePoint(1, 2);
  
  print('p1 == p2: ${p1 == p2}'); // true for const objects with same values
  print('identical(p1, p2): ${identical(p1, p2)}'); // true - same object in memory
}
```

**Factory constructors:**
```dart
class Logger {
  String name;
  static final Map<String, Logger> _loggers = {};
  
  // Private constructor
  Logger._internal(this.name);
  
  // Factory constructor - can return existing instance
  factory Logger(String name) {
    if (_loggers.containsKey(name)) {
      return _loggers[name]!;
    } else {
      final logger = Logger._internal(name);
      _loggers[name] = logger;
      return logger;
    }
  }
  
  void log(String message) {
    print('[$name] $message');
  }
}

class DatabaseConnection {
  String host;
  int port;
  bool isConnected = false;
  
  DatabaseConnection._internal(this.host, this.port);
  
  // Factory constructor with connection logic
  factory DatabaseConnection.connect(String host, int port) {
    print('Connecting to $host:$port...');
    
    // Simulate connection validation
    if (host.isEmpty || port <= 0) {
      throw ArgumentError('Invalid host or port');
    }
    
    DatabaseConnection connection = DatabaseConnection._internal(host, port);
    connection.isConnected = true;
    print('Connected successfully!');
    return connection;
  }
  
  void query(String sql) {
    if (!isConnected) {
      print('Error: Not connected to database');
      return;
    }
    print('Executing query: $sql');
  }
  
  void disconnect() {
    isConnected = false;
    print('Disconnected from $host:$port');
  }
}

void main() {
  // Factory constructor - singleton pattern
  Logger logger1 = Logger('App');
  Logger logger2 = Logger('App');
  Logger logger3 = Logger('Database');
  
  print('logger1 == logger2: ${identical(logger1, logger2)}'); // true - same instance
  print('logger1 == logger3: ${identical(logger1, logger3)}'); // false - different instances
  
  logger1.log('Application started');
  logger2.log('User logged in'); // Same logger instance
  logger3.log('Database query executed');
  
  // Factory constructor with validation
  try {
    DatabaseConnection db = DatabaseConnection.connect('localhost', 5432);
    db.query('SELECT * FROM users');
    db.disconnect();
  } catch (e) {
    print('Database error: $e');
  }
}
```

### **Module 6: Object-Oriented Programming (OOP) - Part 1**

[⬅ Previous](topic-6-1-classes-and-objects.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-6-3-inheritance.md)

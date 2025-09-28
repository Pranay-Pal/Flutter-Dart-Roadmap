# Topic 13.2: Advanced Math and Data (`dart:math` & `dart:typed_data`)

[⬅ Previous](topic-13-1-core-essentials.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-3-specialized-collections.md)

Beyond basic arithmetic, Dart provides powerful libraries for specialized numerical and data-handling tasks. `dart:math` offers a suite of advanced mathematical functions, while `dart:typed_data` provides tools for handling fixed-size, efficient lists of numerical data, crucial for performance-sensitive applications and native code interoperability.

- [x] Advanced Math: `min`, `max`, `pow`, `sqrt`, trigonometric functions, `Random`, `Point`, `Rectangle`.
- [x] Typed Data: Understanding `ByteBuffer`, `TypedData`, `Uint8List`, and `ByteData`.

---

### 1. Advanced Math (`dart:math`)

The `dart:math` library contains mathematical constants, functions for geometry and trigonometry, and a random number generator.

- **Constants**: `pi`, `e`, `sqrt2`, etc.
- **Core Functions**: `min()`, `max()`, `pow()`, `sqrt()`.
- **Trigonometry**: `sin()`, `cos()`, `tan()`, `asin()`, etc.
- **Random Number Generation**: The `Random` class.
- **Geometric Types**: `Point` and `Rectangle`.

```dart
import 'dart:math';

void main() {
  // 1. Constants and Core Functions
  print('Value of pi: $pi');
  print('Square root of 2: $sqrt2');
  
  num larger = max(10, 20); // 20
  num smaller = min(10.5, 5.5); // 5.5
  num power = pow(2, 5); // 32 (2 to the power of 5)
  num squareRoot = sqrt(49); // 7
  
  print('\nMax of 10 and 20 is $larger');
  print('2 to the power of 5 is $power');

  // 2. Trigonometric Functions (angles are in radians)
  double angleDegrees = 90.0;
  double angleRadians = angleDegrees * (pi / 180);
  
  print('\nSine of 90 degrees: ${sin(angleRadians)}'); // ~1.0
  print('Cosine of 90 degrees: ${cos(angleRadians)}'); // ~0.0

  // 3. Generating Random Numbers
  final random = Random();
  int randomInt = random.nextInt(100); // A random integer between 0 and 99.
  double randomDouble = random.nextDouble(); // A random double between 0.0 and 1.0.
  bool randomBool = random.nextBool();
  
  print('\nRandom int (0-99): $randomInt');
  print('Random double (0-1): $randomDouble');
  print('Random bool: $randomBool');

  // 4. Using Point for 2D coordinates
  final pointA = Point(5, 10);
  final pointB = Point(15, 25);
  
  double distance = pointA.distanceTo(pointB);
  print('\nDistance between $pointA and $pointB is ${distance.toStringAsFixed(2)}');
  
  Point midpoint = pointA + (pointB - pointA) * 0.5;
  print('Midpoint is $midpoint');

  // 5. Using Rectangle for geometry
  final rect = Rectangle(pointA.x, pointA.y, 100, 50); // x, y, width, height
  print('\nRectangle: $rect');
  print('  - Contains pointB? ${rect.containsPoint(pointB)}'); // false
  
  Rectangle biggerRect = rect.boundingBox(Rectangle(10, 20, 20, 20));
  print('Bounding box: $biggerRect');
}
```

---

### 2. Typed Data (`dart:typed_data`)

While a standard `List<int>` is flexible, it can be inefficient for large amounts of raw numerical data (e.g., image data, audio streams, native C data). Typed data lists are fixed-size and backed by a contiguous block of memory (`ByteBuffer`), making them highly memory-efficient and fast.

- **`ByteBuffer`**: An opaque, fixed-size region of raw bytes. You rarely use it directly.
- **`TypedData`**: An interface for lists that are views on a `ByteBuffer`.
- **`Uint8List`, `Int32List`, `Float64List`, etc.**: Concrete `TypedData` implementations. `Uint8List` (a list of 8-bit unsigned integers) is the most common, representing raw byte data.
- **`ByteData`**: A special view that lets you read/write numbers of different types and endianness at any byte offset, which is critical for parsing binary file formats or network protocols.

```dart
import 'dart:typed_data';

void main() {
  // --- Part 1: Using Uint8List ---

  // 1. Create a typed data list of 16 bytes, all initialized to zero.
  final bytes = Uint8List(16);
  print('Initial Uint8List: $bytes');

  // 2. Manipulate data like a regular list.
  for (int i = 0; i < bytes.length; i++) {
    bytes[i] = i * 10;
  }
  print('Modified Uint8List: $bytes');

  // 3. Create a "view" on part of the list's buffer.
  // This is a memory-efficient operation; it doesn't copy the data.
  // The view shares the same underlying ByteBuffer.
  // This view covers 4 bytes starting from the 4th byte (index 4).
  final view = Uint8List.view(bytes.buffer, 4, 4);
  print('\nA view of the buffer: $view');

  // 4. Modifying the view affects the original list.
  view[0] = 255;
  print('Modified view: $view');
  print('Original list after modifying view: $bytes'); // Note the change at index 4.

  // --- Part 2: Using ByteData for fine-grained control ---

  // Imagine a binary format: 16-bit ID, 32-bit value, 16-bit checksum.
  final binaryData = ByteData(8); // 2 + 4 + 2 = 8 bytes

  // 1. Write data with specific types and endianness.
  // Endianness refers to the byte order (Big or Little). It's crucial
  // when interoperating with systems that have a different native order.
  binaryData.setUint16(0, 101, Endian.big);    // Write ID (101) at offset 0
  binaryData.setFloat32(2, 3.14, Endian.big);  // Write value (pi) at offset 2
  binaryData.setInt16(6, -50, Endian.big);     // Write checksum at offset 6

  print('\n--- ByteData Example ---');
  print('Raw bytes in ByteData: ${binaryData.buffer.asUint8List()}');

  // 2. Read the data back.
  final id = binaryData.getUint16(0, Endian.big);
  final value = binaryData.getFloat32(2, Endian.big);
  final checksum = binaryData.getInt16(6, Endian.big);

  print('Read ID: $id');
  print('Read Value: ${value.toStringAsFixed(2)}');
  print('Read Checksum: $checksum');
}
```

#### When to Use Typed Data vs. `List<int>`:
- Use `List<int>` for general-purpose collections where size changes dynamically and you need methods like `.add()` and `.remove()`.
- Use `Uint8List` (or other typed lists) when:
  - Handling binary file data, image pixels, or network packets.
  - Interfacing with C libraries via `dart:ffi`.
  - You need a predictable, compact memory layout for performance-critical code.
- Use `ByteData` when you need to read or write a mix of different numerical types (e.g., `int16`, `float32`) from the same block of bytes.

[⬅ Previous](topic-13-1-core-essentials.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-3-specialized-collections.md)

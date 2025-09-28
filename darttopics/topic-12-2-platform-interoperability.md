# Topic 12.2: Platform Interoperability

[⬅ Previous](topic-12-1-the-professional-toolchain.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-1-core-essentials.md)

Dart is a versatile language that can run on many platforms. Its interoperability features allow you to leverage existing code written in other languages, giving you access to mature libraries, performance-critical code, and platform-specific APIs.

This topic explores the two primary ways Dart interoperates with other languages: through the Foreign Function Interface (FFI) for C and through platform channels for mobile development with Flutter.

- [x] Calling C code with Foreign Function Interface (FFI)
- [x] Interacting with Kotlin/Java and Swift/Objective-C code in Flutter

---

### 1. Calling C Code with Dart FFI

**Dart FFI (Foreign Function Interface)** allows Dart code to call functions in C-style native libraries (e.g., `.dll` on Windows, `.so` on Linux, `.dylib` on macOS). This is incredibly powerful for performance-critical tasks, interacting with low-level OS or hardware APIs, or reusing existing C/C++ codebases without rewriting them in Dart.

**How It Works**:
1.  **Provide a Native Library**: You need a compiled dynamic library (`.so`, `.dll`, `.dylib`) that exports the functions you want to call.
2.  **Load the Library**: In Dart, use `DynamicLibrary.open()` to load your compiled native library into memory.
3.  **Define Function Signatures**: You must provide two `typedef`s for the function signature: one describing the C function's signature and one for the Dart version. This tells Dart how to manage the data types and calling conventions.
4.  **Look Up the Function**: Use `lookupFunction()` to find the C function by its name and cast it to your Dart `typedef`.
5.  **Call the Function**: Call the resulting function just like any other Dart function. Dart FFI handles the translation.

**Example: Calling a Simple C `add` function**

First, you need a C source file and to compile it into a dynamic library.

```c
// In a file named math_lib.c

// On Windows, you may need to export the function explicitly.
// #define DART_EXPORT __declspec(dllexport)
// DART_EXPORT int add(int a, int b) { ... }

int add(int a, int b) {
    return a + b;
}
```
**Compilation Commands**:
- **Linux/macOS**: `gcc -shared -o math_lib.so -fPIC math_lib.c`
- **Windows**: `gcc -shared -o math_lib.dll math_lib.c`

Then, you can call it from Dart:

```dart
// This code must be run in a non-web environment.
import 'dart:ffi';
import 'dart:io' show Platform;

// Define the C function's signature. Note the use of FFI types like Int32.
typedef AddC = Int32 Function(Int32 a, Int32 b);
// Define the Dart function's signature. Note the use of Dart types like int.
typedef AddDart = int Function(int a, int b);

void main() {
  // Determine the correct library file name based on the OS.
  String libraryPath = 'path/to/your/lib/math_lib.so'; // Default to .so
  if (Platform.isMacOS) {
    libraryPath = 'path/to/your/lib/math_lib.dylib';
  } else if (Platform.isWindows) {
    libraryPath = 'path/to/your/lib/math_lib.dll';
  }

  // Load the dynamic library.
  final dylib = DynamicLibrary.open(libraryPath);

  // Look up the function 'add' and cast it to the Dart signature.
  final add = dylib.lookupFunction<AddC, AddDart>('add');

  // Call the C function from Dart!
  final result = add(10, 5);
  print('Result from C: $result'); // Output: Result from C: 15
}
```

---

### 2. Interacting with Native Code in Flutter

When building mobile apps with Flutter, you often need to access platform-specific APIs that are not available in Dart, such as fetching battery level, using a specific sensor, or integrating with a third-party native SDK.

Flutter provides **platform channels** for this purpose. The most common type is `MethodChannel`, which allows for asynchronous method calls between Dart and the host platform (Android or iOS).

**How `MethodChannel` Works**:
1.  **Create a Channel**: Create a `MethodChannel` in your Dart code with a unique name. This same name is used on the native side to identify the channel.
2.  **Invoke a Method**: In Dart, call `channel.invokeMethod()` with a method name (as a string) and optional arguments. This sends a message to the host platform.
3.  **Receive and Handle the Call**: On the native side (Kotlin/Java for Android, Swift/Objective-C for iOS), you set up a `MethodCallHandler`. This handler listens for method calls from Dart, identifies them by name, and executes native code.
4.  **Send a Result Back**: The native code performs the requested action and sends a result back to Dart (either a success with data or an error), which completes the `Future` in your Dart code.

**Example: Getting the Battery Level**

**Dart (Flutter) Code:**

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

class BatteryPage extends StatefulWidget {
  @override
  _BatteryPageState createState() => _BatteryPageState();
}

class _BatteryPageState extends State<BatteryPage> {
  // Define the method channel.
  static const _channel = MethodChannel('com.example.myapp/battery');
  String _batteryLevel = 'Waiting for battery level...';

  Future<void> _getBatteryLevel() async {
    try {
      // Invoke the method on the native side.
      final int? level = await _channel.invokeMethod('getBatteryLevel');
      setState(() {
        _batteryLevel = 'Battery Level: $level%';
      });
    } on PlatformException catch (e) {
      setState(() {
        _batteryLevel = "Failed to get battery level: '${e.message}'.";
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Battery Level')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(_batteryLevel),
            ElevatedButton(
              onPressed: _getBatteryLevel,
              child: Text('Get Battery Level'),
            ),
          ],
        ),
      ),
    );
  }
}
```

**Android (Kotlin) Code:**

```kotlin
// In MainActivity.kt
package com.example.my_flutter_app // Your package name

import io.flutter.embedding.android.FlutterActivity
import io.flutter.embedding.engine.FlutterEngine
import io.flutter.plugin.common.MethodChannel
import android.os.BatteryManager
import android.content.Context
import android.content.ContextWrapper
import android.content.Intent
import android.content.IntentFilter

class MainActivity: FlutterActivity() {
    private val CHANNEL = "com.example.myapp/battery"

    override fun configureFlutterEngine(flutterEngine: FlutterEngine) {
        super.configureFlutterEngine(flutterEngine)
        MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler { call, result ->
            if (call.method == "getBatteryLevel") {
                val batteryLevel = getBatteryLevel()
                if (batteryLevel != -1) {
                    result.success(batteryLevel)
                } else {
                    result.error("UNAVAILABLE", "Battery level not available.", null)
                }
            } else {
                result.notImplemented()
            }
        }
    }

    private fun getBatteryLevel(): Int {
        val batteryManager = getSystemService(Context.BATTERY_SERVICE) as BatteryManager
        return batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY)
    }
}
```

This mechanism allows your Dart code to seamlessly request information or trigger actions from the underlying native platform, enabling full access to the device's capabilities.

---

### 3. The `external` Keyword: Declaring Unimplemented Functions

The `external` keyword is used to declare a function, method, or getter/setter that is implemented elsewhere. Its body is omitted and replaced with a semicolon (`;`).

This is a signal that the implementation will be provided by an external source, often in a different language or environment. You will encounter it in a few key scenarios:

1.  **Dart FFI**: When defining the Dart signature for a C function, you can use `external` to mark it as being implemented in the native library.
2.  **Platform-Specific Code**: When using `dart:io` vs. `dart:html`, you might define a function as `external` and provide different implementations for each platform.
3.  **Patching the SDK**: The Dart SDK itself uses `external` to define APIs in public libraries, with the implementations "patched in" from private, platform-specific files.

**Example: Using `external` with Dart FFI**

This is an alternative way to structure the FFI code from the first section. Instead of using `lookupFunction`, you can use `@Native` and `external`.

```dart
import 'dart:ffi';
import 'dart:io' show Platform;

// 1. Load the library.
final dylib = DynamicLibrary.open('path/to/your/lib/math_lib.dll');

// 2. Use @Native to link the external Dart function to the C function.
// The C function signature is provided as a type argument.
@Native<Int32 Function(Int32, Int32)>(symbol: 'add')
// 3. Declare the Dart function as external.
external int add(int a, int b);

void main() {
  // 4. Call the function directly.
  // Dart's compiler connects the call to the C implementation.
  final result = add(20, 22);
  print('Result from C (using external): $result'); // Output: 42
}
```
Using `external` with `@Native` can lead to cleaner and more declarative FFI code, as the boilerplate of `lookupFunction` is handled for you.

[⬅ Previous](topic-12-1-the-professional-toolchain.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-1-core-essentials.md)

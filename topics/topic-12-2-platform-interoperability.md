# Topic 12.2: Platform Interoperability

    * [ ] Calling C code with Foreign Function Interface (FFI)
    * [ ] Interacting with Kotlin/Java and Swift/Objective-C code

#### FFI Example

```dart
import 'dart:ffi';
import 'dart:io';

// C function signature: int add(int a, int b)
typedef AddC = Int32 Function(Int32 a, Int32 b);
typedef AddDart = int Function(int a, int b);

void main() {
  // Load the dynamic library
  final dylib = Platform.isWindows 
      ? DynamicLibrary.open('math_lib.dll')
      : DynamicLibrary.open('libmath_lib.so');
  
  // Look up the function
  final add = dylib.lookupFunction<AddC, AddDart>('add');
  
  // Call the C function
  final result = add(5, 3);
  print('5 + 3 = $result');
}
```

### **Module 13: Best Practices & Next Steps**

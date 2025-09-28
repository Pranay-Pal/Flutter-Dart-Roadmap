# Topic 13.9: Developer and Debugging Tools (`dart:developer` & `dart:mirrors`)

[⬅ Previous](topic-13-8-isolates.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-10-third-party-packages.md)

Beyond the core language, Dart provides libraries specifically designed to aid in debugging, profiling, and dynamic analysis. The `dart:developer` library is the modern tool for interacting with debuggers and profilers, while `dart:mirrors` offers powerful reflection capabilities, albeit with significant limitations.

- [x] Enhanced Logging and Inspection: `log`, `inspect`, and `debugger`.
- [x] Performance Profiling: `postEvent` and `Timeline`.
- [x] Introspection and Reflection with `dart:mirrors` (and its caveats).

---

### 1. Enhanced Logging and Inspection with `dart:developer`

The `dart:developer` library is your bridge to debugging and profiling tools like Dart DevTools. It provides a much richer experience than `print()`.

- **`log()`**: A structured logging function. It allows you to provide a name, error object, severity level, and a longer message, making logs filterable and more informative in the developer console.
- **`debugger()`**: Programmatically inserts a breakpoint. If a debugger is attached, execution will pause at this line. You can add a conditional `when` clause.
- **`inspect()`**: Sends an object to the debugger's inspector, allowing you to explore its properties and state in detail at a specific point in time.

```dart
import 'dart:developer';

void processUserData(Map<String, dynamic> data) {
  log('Starting user data processing', name: 'user.service');

  if (!data.containsKey('id')) {
    // Log an error with a specific level (1000 for SEVERE).
    log(
      'User data is missing an ID.',
      name: 'user.service',
      level: 1000, // SEVERE
      error: 'ID field not found in provided data.',
    );
    
    // Pause execution only if the data is from an external source.
    debugger(when: data['source'] == 'external', message: 'No user ID found. Please inspect the data object.');
    return;
  }

  final user = {'id': data['id'], 'name': 'Computed Name'};
  
  // Send the 'user' object to the inspector in Dart DevTools.
  inspect(user);

  log('Finished processing for user ${user['id']}', name: 'user.service');
}

void main() {
  // This call will trigger the debugger because when is true.
  processUserData({'name': 'Alice', 'source': 'external'}); 
  
  // This call will log an error but not trigger the debugger.
  processUserData({'name': 'Bob', 'source': 'internal'});
}
```
When you run this code with a debugger attached (e.g., from VS Code or by running `dart run --observe`), you can connect Dart DevTools to see the structured logs, and the program will pause on the `debugger()` line for the first call.

---

### 2. Performance Profiling with `Timeline`

The `dart:developer` library also allows you to add custom events to Dart's performance timeline. This is incredibly useful for measuring the duration of specific operations in your application and visualizing them in the DevTools "Performance" or "CPU Profiler" views.

- **`Timeline.startSync()` / `Timeline.finishSync()`**: Used to trace a synchronous operation.
- **`postEvent()`**: Emits a single, instantaneous event to the timeline.

```dart
import 'dart:developer';
import 'dart:io';

void performComplexOperation() {
  // Start a timeline task. The name and arguments are arbitrary.
  Timeline.startSync('Complex Operation', arguments: {'step': 'parsing'});
  
  // Simulate some work
  sleep(Duration(milliseconds: 50));
  
  // Post an event to mark a milestone within the task.
  postEvent('Parsing Complete', {'type': 'milestone'});

  // Finish the task. The name must match the startSync name.
  Timeline.finishSync();
}

void main() {
  print('Running complex operation...');
  performComplexOperation();
  print('Operation finished. Check Dart DevTools Performance tab for "Complex Operation".');
}
```
When you profile this application, you will see a "Complex Operation" bar in the timeline, giving you a precise measurement of how long it took to execute.

---

### 3. Introspection and Reflection with `dart:mirrors`

**Important**: `dart:mirrors` is a powerful but highly specialized library. It is **disabled by default in AOT-compiled code (Flutter release builds, compiled executables)** because it prevents "tree shaking" (the removal of unused code), which leads to much larger application sizes. It is primarily useful for server-side JIT-mode applications, code generators, or debug tools.

Mirrors allow a program to inspect and manipulate itself at runtime. You can get information about classes, methods, and properties, and even invoke them dynamically.

- **`reflect()`**: Get an `InstanceMirror` for an object instance.
- **`reflectClass()`**: Get a `ClassMirror` for a `Type`.

```dart
// IMPORTANT: This code will not run in Flutter or a web browser.
// It requires a command-line Dart environment running in JIT mode.
import 'dart:mirrors';

class MyReflectableClass {
  final String name;
  int _value = 0;

  MyReflectableClass(this.name);

  void myMethod(int newValue) {
    _value = newValue;
    print('myMethod called with $newValue. New private value is $_value.');
  }
}

void main() {
  final myObject = MyReflectableClass('Test');

  // 1. Get an InstanceMirror for the object.
  InstanceMirror instanceMirror = reflect(myObject);
  ClassMirror classMirror = instanceMirror.type;

  // 2. List all declarations (methods, variables) in the class.
  print('Declarations in ${classMirror.simpleName}:');
  for (var decl in classMirror.declarations.values) {
    final name = MirrorSystem.getName(decl.simpleName);
    if (!decl.isPrivate) {
      print('- Public: $name');
    }
  }

  // 3. Dynamically invoke a method using its Symbol.
  print('\nDynamically invoking myMethod...');
  instanceMirror.invoke(#myMethod, [42]); // The '#' creates a Symbol.

  // 4. Dynamically get and set a private field's value.
  // This demonstrates the power and potential danger of mirrors.
  print('\nDynamically accessing private field _value...');
  var currentValue = instanceMirror.getField(#_value).reflectee;
  print('Current private value: $currentValue'); // 42

  instanceMirror.setField(#_value, 100);
  currentValue = instanceMirror.getField(#_value).reflectee;
  print('New private value: $currentValue'); // 100
}
```
While powerful for frameworks and tools that need to be highly dynamic, the performance and size costs of mirrors mean they should be avoided in typical application code. **Prefer code generation** when you need to solve similar problems in a Flutter/web environment.

[⬅ Previous](topic-13-8-isolates.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-10-third-party-packages.md)

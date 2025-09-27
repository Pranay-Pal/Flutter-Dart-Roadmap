# Topic 13.2: Compilation and Deployment

    * [ ] Understanding JIT (development) vs. AOT (production) compilation

#### Compilation Types

```dart
// Development (JIT - Just In Time)
// - Fast startup
// - Hot reload support
// - Larger memory usage
// Command: dart run main.dart

// Production (AOT - Ahead Of Time)  
// - Optimized performance
// - Smaller runtime footprint
// - No hot reload
// Commands:
// dart compile exe main.dart        # Native executable
// dart compile js main.dart         # JavaScript (web)
// dart compile aot-snapshot main.dart # AOT snapshot

void main() {
  print('Dart compilation demonstration');
  print('JIT: Great for development');
  print('AOT: Perfect for production');
}
```

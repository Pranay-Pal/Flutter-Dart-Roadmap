# Topic 13.2: Compilation and Deployment

[⬅ Previous](topic-13-1-writing-quality-code.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-13-3-where-to-go-next.md)

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

#### Packaging with Docker

```dockerfile
# Dockerfile
FROM dart:stable AS build

WORKDIR /app
COPY pubspec.* ./
RUN dart pub get

COPY . .
RUN dart compile exe bin/server.dart -o bin/server

FROM gcr.io/distroless/cc-debian12 AS runtime
WORKDIR /app
COPY --from=build /runtime/ /runtime/
COPY --from=build /app/bin/server ./server

EXPOSE 8080
CMD ["/app/server"]
```

### **Module 13: Best Practices & Next Steps**

[⬅ Previous](topic-13-1-writing-quality-code.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-13-3-where-to-go-next.md)

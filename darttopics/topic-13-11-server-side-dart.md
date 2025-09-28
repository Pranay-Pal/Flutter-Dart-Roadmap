# Topic 13.11: Server-Side Dart with `package:shelf`

[⬅ Previous](topic-13-10-third-party-packages.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-14-1-writing-quality-code.md)

While often associated with Flutter, Dart is a powerful and capable language for building fast, scalable server-side applications. The `package:shelf` provides a minimal, middleware-based web server framework that is easy to understand and extend. It serves as the foundation for building everything from simple APIs to complex web applications.

This topic covers the `shelf` ecosystem:
- [ ] Creating a Basic Web Server with `package:shelf`.
- [ ] Routing Requests with `package:shelf_router`.
- [ ] Handling WebSockets with `package:shelf_web_socket`.

---

### 1. Creating a Basic Web Server with `package:shelf`

`package:shelf` is a web server framework for Dart. It allows you to compose a request handler by combining smaller, focused handlers (middleware). Each piece of middleware can inspect or modify the request before passing it to the next handler, or it can create a response and stop the chain.

**`pubspec.yaml`**:
```yaml
dependencies:
  shelf: ^1.4.1
  shelf_router: ^1.1.4
  shelf_web_socket: ^1.0.4
  web_socket_channel: ^2.4.0
```

A `shelf` handler is simply a function that takes a `Request` and returns a `Future<Response>`.

```dart
import 'package:shelf/shelf.dart';
import 'package:shelf/shelf_io.dart' as io;

void main() async {
  // A simple handler that responds with "Hello, World!".
  Future<Response> _echoRequest(Request request) async {
    return Response.ok('Hello, World!\n');
  }

  // A pipeline allows you to chain middleware and a final handler.
  final handler = const Pipeline()
      .addMiddleware(logRequests()) // Log all incoming requests.
      .addHandler(_echoRequest);

  final server = await io.serve(handler, 'localhost', 8080);
  print('Serving at http://${server.address.host}:${server.port}');
}
```
To run this, save it as a file (e.g., `server.dart`) and execute `dart run server.dart`. You can then visit `http://localhost:8080` in your browser.

#### Understanding Middleware

Middleware are the building blocks of a `shelf` application. They are handlers that can perform actions before or after a request is processed by the main handler. The `Pipeline` class chains them together.

A middleware is a function that takes a handler and returns a new handler. It can:
1.  **Inspect the request:** Read headers, check the path, etc.
2.  **Modify the request:** Add headers or context for downstream handlers.
3.  **Short-circuit:** Return a `Response` immediately (e.g., for authentication).
4.  **Modify the response:** Add headers or log the status code of the response from a downstream handler.

**Example: A custom middleware for adding a header**
```dart
// This middleware adds a custom header to every response.
Middleware _addCustomHeader() {
  return (Handler innerHandler) {
    return (Request request) async {
      // Wait for the inner handler to produce a response.
      final response = await innerHandler(request);
      // Add a custom header to the response.
      return response.change(headers: {'X-Custom-Header': 'Powered by Shelf'});
    };
  };
}

// Usage in a pipeline:
final handlerWithCustomHeader = const Pipeline()
    .addMiddleware(_addCustomHeader())
    .addHandler((Request request) => Response.ok('Check your headers!'));
```

---

### 2. Combining HTTP Routing and WebSockets

In a real-world application, you'll want to handle both standard HTTP requests (like for a REST API) and WebSocket connections on the same server, but on different paths. `shelf_router` makes this easy to organize.

This example creates a server that:
-   Serves a REST API at `/api/...`.
-   Handles WebSocket connections at `/ws`.

```dart
import 'dart:io';
import 'package:shelf/shelf.dart';
import 'package:shelf/shelf_io.dart' as io;
import 'package:shelf_router/shelf_router.dart';
import 'package:shelf_web_socket/shelf_web_socket.dart';
import 'package.web_socket_channel/web_socket_channel.dart';

void main() async {
  final app = Router();

  // API Route: Returns a simple JSON message.
  app.get('/api/greet', (Request request) {
    return Response.ok(
      '{"message": "Hello from the API!"}',
      headers: {'Content-Type': 'application/json'},
    );
  });

  // WebSocket Route: Upgrades the connection to a WebSocket.
  final webSocket = webSocketHandler((WebSocketChannel webSocket) {
    webSocket.stream.listen((message) {
      print('Received WebSocket message: $message');
      webSocket.sink.add('Server echoes: $message');
    });
  });
  app.get('/ws', webSocket);

  // Add a fallback for any routes that don't match.
  app.all('/<ignored|.*>', (Request request) {
    return Response.notFound('Page not found.');
  });

  final handler = const Pipeline()
      .addMiddleware(logRequests())
      .addHandler(app);

  final server = await io.serve(handler, InternetAddress.anyIPv4, 8080);
  print('Server running on http://${server.address.host}:${server.port}');
  print('API available at /api/greet');
  print('WebSocket available at /ws');
}
```

**How to test it:**
1.  **Run the server:** `dart run server.dart`
2.  **Test the API:** Open `http://localhost:8080/api/greet` in your browser or use a tool like `curl`.
3.  **Test the WebSocket:** Use a WebSocket client (like a simple Dart client or a browser tool) to connect to `ws://localhost:8080/ws`. Send a message, and the server will echo it back.

This combined approach provides a solid foundation for building complex web services and applications with Dart.

[⬅ Previous](topic-13-10-third-party-packages.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-14-1-writing-quality-code.md)

# Topic 13.7: Input/Output (`dart:io`) - Part 2: Networking

[⬅ Previous](topic-13-6-io-files-processes.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-8-isolates.md)

The `dart:io` library provides a robust set of tools for network programming, enabling you to build everything from simple HTTP clients to complex, real-time WebSocket servers and custom TCP/UDP protocols.

**Note**: `dart:io` is not available in web projects. For web networking, use `package:http` or `dart:html`.

- [x] Low-Level HTTP: `HttpClient` for clients and `HttpServer` for servers.
- [x] Real-Time Communication: `WebSocket` clients and servers.
- [x] Low-Level Networking: `Socket` (TCP) and `RawDatagramSocket` (UDP).

---

### 1. HTTP Client and Server

While `package:http` is simpler for client requests, `dart:io` provides the building blocks for both clients and servers, offering maximum control.

#### `HttpClient`
`HttpClient` is ideal for when you need to manage cookies, set custom headers, or handle authentication manually.

```dart
import 'dart:io';
import 'dart:convert';

void main() async {
  final client = HttpClient();
  final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');

  try {
    print('Making GET request to $url...');
    // 1. Open the request.
    final request = await client.getUrl(url);
    
    // 2. (Optional) Customize headers.
    request.headers.set(HttpHeaders.contentTypeHeader, 'application/json');
    request.headers.set('X-My-Header', 'dart-is-awesome');

    // 3. Send the request and get the response.
    final response = await request.close();

    print('Status code: ${response.statusCode}');

    // 4. Read the response body by joining the stream of chunks.
    final responseBody = await response.transform(utf8.decoder).join();
    
    if (response.statusCode == HttpStatus.ok) {
      final data = jsonDecode(responseBody);
      print('Post Title: ${data['title']}');
    } else {
      print('Error response:\n$responseBody');
    }

  } finally {
    // 5. IMPORTANT: Always close the client to release resources.
    client.close();
  }
}
```

#### `HttpServer`
You can build a fully-featured web server directly in Dart.

```dart
import 'dart:io';

Future<void> runHttpServer() async {
  final server = await HttpServer.bind(InternetAddress.loopbackIPv4, 8080);
  print('HTTP server listening on http://${server.address.host}:${server.port}');

  await for (final request in server) {
    print('Received ${request.method} request for ${request.uri.path}');
    if (request.uri.path == '/hello') {
      request.response
        ..statusCode = HttpStatus.ok
        ..headers.contentType = ContentType.text
        ..write('Hello, World!')
        ..close();
    } else {
      request.response
        ..statusCode = HttpStatus.notFound
        ..write('Not Found')
        ..close();
    }
  }
}
```

---

### 2. Real-Time Communication with `WebSocket`

WebSockets provide a two-way, real-time communication channel over a single TCP connection, perfect for chat apps, live dashboards, and multiplayer games.

#### WebSocket Server
A WebSocket server is typically created by "upgrading" a standard HTTP request.

```dart
import 'dart:io';

Future<void> runWebSocketServer() async {
  final server = await HttpServer.bind(InternetAddress.loopbackIPv4, 4040);
  print('WebSocket server running on ws://${server.address.host}:${server.port}');

  await for (final request in server) {
    if (WebSocketTransformer.isUpgradeRequest(request)) {
      // Upgrade the HTTP request to a WebSocket connection.
      final socket = await WebSocketTransformer.upgrade(request);
      print('Client connected!');
      
      socket.listen(
        (message) {
          print('Received: $message');
          socket.add('Echo: You sent "$message"');
        },
        onDone: () => print('Client disconnected.'),
      );
    }
  }
}
```

#### WebSocket Client
The client connects directly to the WebSocket URL. For a simpler client API, consider the [`package:web_socket_channel`](topic-13-10-third-party-packages.md), which provides a platform-independent `StreamChannel` wrapper.

```dart
import 'dart:io';

Future<void> runWebSocketClient() async {
  try {
    final socket = await WebSocket.connect('ws://localhost:4040');
    print('Connected to WebSocket server.');

    socket.add('Hello, Server!');
    
    socket.listen(
      (message) => print('Server says: $message'),
      onDone: () => print('Connection closed.'),
      onError: (error) => print('Error: $error'),
    );
    
    await Future.delayed(Duration(seconds: 2));
    socket.add('This is another message.');
    await Future.delayed(Duration(seconds: 2));
    socket.close();
  } catch (e) {
    print("Couldn't connect to WebSocket server: $e");
  }
}

// To run this example:
// void main() async {
//   runWebSocketServer();
//   await Future.delayed(Duration(seconds: 1));
//   await runWebSocketClient();
// }
```

---

### 3. Low-Level Networking with Sockets

For maximum control, you can work directly with TCP and UDP sockets.

#### TCP `ServerSocket` and `Socket`
This is useful for building custom, stateful network protocols. Here's a simple "echo" server.

```dart
import 'dart:io';
import 'dart:convert';

Future<void> runTcpServer() async {
  final serverSocket = await ServerSocket.bind(InternetAddress.loopbackIPv4, 4567);
  print('TCP Echo Server on ${serverSocket.address.host}:${serverSocket.port}');

  serverSocket.listen((Socket client) {
    print('Client connected from ${client.remoteAddress.address}:${client.remotePort}');
    client.listen(
      (data) {
        final message = utf8.decode(data).trim();
        print('Received: "$message"');
        client.write('Echo: $message\n'); // Echo back
      },
      onDone: () {
        print('Client disconnected.');
        client.destroy();
      },
    );
  });
}
```

#### UDP `RawDatagramSocket`
UDP is a connectionless protocol, ideal for fire-and-forget messages where delivery isn't guaranteed (e.g., game state updates, logging).

```dart
import 'dart:io';
import 'dart:convert';

Future<void> runUdpExample() async {
  final targetPort = 6000;
  final receiver = await RawDatagramSocket.bind(InternetAddress.loopbackIPv4, targetPort);
  print('UDP receiver listening on port $targetPort');

  receiver.listen((RawSocketEvent event) {
    if (event == RawSocketEvent.read) {
      final datagram = receiver.receive();
      if (datagram != null) {
        final message = utf8.decode(datagram.data);
        print('Received UDP message: "$message" from ${datagram.address.address}:${datagram.port}');
      }
    }
  });

  // Send a message to ourself
  final sender = await RawDatagramSocket.bind(InternetAddress.loopbackIPv4, 0);
  sender.send(utf8.encode('Hello, UDP!'), InternetAddress.loopbackIPv4, targetPort);
  print('Sent UDP message.');
  
  await Future.delayed(Duration(seconds: 1));
  receiver.close();
  sender.close();
}
```

[⬅ Previous](topic-13-6-io-files-processes.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-8-isolates.md)

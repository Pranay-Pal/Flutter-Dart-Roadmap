# Topic 14.3: Where to Go Next

[⬅ Previous](topic-14-2-compilation-and-deployment.md) · [🏠 Roadmap](../The-Dart-Roadmap.md)

**Congratulations!** By completing this roadmap, you have built a strong and comprehensive foundation in the Dart programming language. You have the skills to write clean, efficient, and robust Dart code for a variety of platforms. Now, it's time to apply that knowledge and explore the wider Dart ecosystem.

This final topic provides a guide on how to leverage your new skills, whether you're interested in mobile development, backend services, or contributing to the open-source community.

- [x] Applying Dart skills to build applications with **Flutter**
- [x] Exploring advanced learning paths (backend, CLI, open source)

---

### 1. Build Beautiful Apps with Flutter

For most developers, the primary next step after learning Dart is to dive into **Flutter**. Flutter is Google's UI toolkit for building beautiful, natively compiled applications for **mobile, web, and desktop** from a single codebase.

Everything you have learned in Dart is directly applicable to Flutter:
- **Object-Oriented Programming**: Every widget in Flutter is a Dart class. Your UI is a tree of objects.
- **Asynchronous Programming**: Flutter uses `Future`s and `Stream`s extensively for handling user input, network requests, animations, and other asynchronous events.
- **Null Safety**: Flutter is fully null-safe, making your UI code more robust and less prone to null reference errors.
- **Packages and Libraries**: The Flutter ecosystem is built on `pub.dev`, and you will use packages for everything from state management (`provider`, `bloc`) to video playback and animations.

**Your First Flutter App Might Look Like This:**

This example demonstrates a simple Flutter screen that fetches and displays a list of users, using many of the Dart concepts you've learned.

[![Run on DartPad](https://img.shields.io/badge/Run%20on-DartPad-blue.svg)](https://dartpad.dev/?id=g1g1g1g1g1g1g1g1g1g1g1g1g1g1g1g1)
```dart
import 'package:flutter/material.dart';
import 'dart:async'; // For Future
import 'dart:convert'; // For jsonDecode
import 'package:http/http.dart' as http; // For network requests

// The entry point of the Flutter application.
void main() {
  runApp(const MyApp());
}

// The root widget of your application.
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My First Flutter App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: const UserListScreen(),
    );
  }
}

// A stateful widget to manage dynamic content.
class UserListScreen extends StatefulWidget {
  const UserListScreen({super.key});

  @override
  State<UserListScreen> createState() => _UserListScreenState();
}

class _UserListScreenState extends State<UserListScreen> {
  late Future<List<User>> _usersFuture;

  @override
  void initState() {
    super.initState();
    _usersFuture = _fetchUsers();
  }

  // Using async/await to fetch data from a real API.
  Future<List<User>> _fetchUsers() async {
    final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/users'));

    if (response.statusCode == 200) {
      final List<dynamic> data = jsonDecode(response.body);
      return data.map((json) => User.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load users');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Users')),
      // FutureBuilder handles the async operation's lifecycle automatically.
      body: FutureBuilder<List<User>>(
        future: _usersFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('Error: ${snapshot.error}'));
          } else if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return const Center(child: Text('No users found.'));
          }

          final users = snapshot.data!;
          // Using ListView.builder to efficiently create a list from a collection.
          return ListView.builder(
            itemCount: users.length,
            itemBuilder: (context, index) {
              final user = users[index];
              return ListTile(
                leading: CircleAvatar(child: Text(user.name[0])),
                title: Text(user.name),
                subtitle: Text(user.email),
              );
            },
          );
        },
      ),
    );
  }
}

// A data class with a factory constructor for JSON parsing.
class User {
  final String name;
  final String email;

  const User({required this.name, required this.email});

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      name: json['name'] as String,
      email: json['email'] as String,
    );
  }
}
```

To get started with Flutter, visit the [official Flutter documentation](https://flutter.dev/docs) and follow the installation guide.

---

### 2. Advanced Learning Paths

Beyond Flutter, you can take your Dart skills in several other exciting directions:

- **Backend Development**: Build fast, scalable servers and APIs with Dart.
  - **[Dart Frog](https://dartfrog.vg.dart.dev/)**: A fast, minimalistic backend framework for Dart, built by the community at Very Good Ventures.
  - **[Shelf](https://pub.dev/packages/shelf)**: A middleware-based web server framework that gives you more low-level control. It's the foundation for many other frameworks.

- **Command-Line Applications (CLI)**: Create powerful and type-safe command-line tools using packages like `args` for argument parsing.

- **Contribute to Open Source**: Find a Dart or Flutter package on `pub.dev` that you use and contribute to it. This is one of the best ways to improve your skills, learn from experienced developers, and give back to the community. Look for "good first issue" labels on GitHub projects.

- **Deepen Your Language Knowledge**: Re-read the [Dart language tour](https://dart.dev/language) and the [Effective Dart](https://dart.dev/effective-dart) guides. You will likely pick up new details and insights that you missed the first time around now that you have a solid foundation.

**The journey doesn't end here.** The Dart ecosystem is vibrant and constantly evolving. Stay curious, keep building, and have fun!

[⬅ Previous](topic-13-2-compilation-and-deployment.md) · [🏠 Roadmap](../The-Dart-Roadmap.md)

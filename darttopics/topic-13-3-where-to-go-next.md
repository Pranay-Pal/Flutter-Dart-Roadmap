# Topic 13.3: Where to Go Next

[⬅ Previous](topic-13-2-compilation-and-deployment.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](../The Definitive Dart Learning Roadmap.md)

    * [ ] Applying Dart skills to build applications with **Flutter**

#### Next Steps with Flutter

```dart
// Example Flutter app structure using Dart concepts learned

import 'package:flutter/material.dart';

// Using classes and inheritance
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Dart to Flutter',
      home: UserListScreen(),
    );
  }
}

// Using async programming
class UserListScreen extends StatefulWidget {
  @override
  _UserListScreenState createState() => _UserListScreenState();
}

class _UserListScreenState extends State<UserListScreen> {
  List<User> users = [];
  bool isLoading = true;
  
  @override
  void initState() {
    super.initState();
    loadUsers();
  }
  
  Future<void> loadUsers() async {
    try {
      // Using the concepts learned in previous modules
      final repository = UserRepository();
      final fetchedUsers = await repository.fetchAllUsers();
      
      setState(() {
        users = fetchedUsers;
        isLoading = false;
      });
    } catch (e) {
      setState(() {
        isLoading = false;
      });
      // Handle error
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Users')),
      body: isLoading
          ? Center(child: CircularProgressIndicator())
          : ListView.builder(
              itemCount: users.length,
              itemBuilder: (context, index) {
                final user = users[index];
                return ListTile(
                  title: Text(user.name),
                  subtitle: Text(user.email),
                );
              },
            ),
    );
  }
}

class UserRepository {
  Future<List<User>> fetchAllUsers() async {
    await Future.delayed(const Duration(seconds: 1));
    return const [
      User(name: 'Alice', email: 'alice@example.com'),
      User(name: 'Bob', email: 'bob@example.com'),
      User(name: 'Charlie', email: 'charlie@example.com'),
    ];
  }
}

class User {
  final String name;
  final String email;

  const User({required this.name, required this.email});
}

void main() {
  runApp(MyApp());
}
```

#### Continue Your Journey

- Explore the Flutter roadmap to master widgets, state management, and platform integration.
- Contribute to open-source Dart packages to sharpen collaboration and code quality habits.
- Experiment with backend frameworks like Dart Frog or Shelf to expand into server-side development.

### **You're Ready for What's Next!**

[⬅ Previous](topic-13-2-compilation-and-deployment.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](../The Definitive Dart Learning Roadmap.md)

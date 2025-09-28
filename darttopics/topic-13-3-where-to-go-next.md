# Topic 13.3: Where to Go Next

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
      final repository = UserRepository(HttpClient());
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

void main() {
  runApp(MyApp());
}
```

---

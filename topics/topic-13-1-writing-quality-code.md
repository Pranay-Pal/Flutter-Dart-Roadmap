# Topic 13.1: Writing Quality Code

    * [ ] Following the "Effective Dart" style and usage guides

#### Effective Dart Guidelines

```dart
// Good: Use clear, descriptive names
class UserRepository {
  final HttpClient _httpClient;
  
  UserRepository(this._httpClient);
  
  // Good: Use async/await for readability
  Future<User> fetchUser(String id) async {
    try {
      final response = await _httpClient.get('/users/$id');
      return User.fromJson(response.data);
    } on HttpException catch (e) {
      throw UserNotFoundException('User $id not found: ${e.message}');
    }
  }
  
  // Good: Use meaningful parameter names
  Future<List<User>> searchUsers({
    required String query,
    int limit = 10,
    int offset = 0,
  }) async {
    // Implementation
    return [];
  }
}

// Good: Document public APIs
/// Represents a user in the system.
/// 
/// Contains basic information like name, email, and creation date.
class User {
  final String id;
  final String name;
  final String email;
  final DateTime createdAt;
  
  const User({
    required this.id,
    required this.name,
    required this.email,
    required this.createdAt,
  });
  
  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] as String,
      name: json['name'] as String,
      email: json['email'] as String,
      createdAt: DateTime.parse(json['created_at'] as String),
    );
  }
}
```

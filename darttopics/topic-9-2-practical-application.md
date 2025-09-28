# Topic 9.2: Practical Application

[⬅ Previous](topic-9-1-understanding-the-system.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-10-1-records-patterns.md)

    * [ ] Type promotion and flow analysis
    * [ ] Using the assertion (`!`) and `late` keywords safely

#### Null Safety in Practice

```dart
class User {
  String name;
  String? email; // Nullable
  late String id; // Late initialization
  
  User(this.name, {this.email});
  
  void initializeId() {
    id = 'user_${name.toLowerCase().replaceAll(' ', '_')}';
  }
  
  String getDisplayName() {
    // Type promotion - Dart knows email is not null inside this block
    if (email != null) {
      return '$name (${email!.split('@')[0]})';
    }
    return name;
  }
  
  void sendEmail(String message) {
    // Null-aware operator
    email?.isNotEmpty == true 
        ? print('Sending email to $email: $message')
        : print('No email available for $name');
  }
}

void main() {
  User user1 = User('John Doe', email: 'john@example.com');
  User user2 = User('Jane Smith'); // No email
  
  user1.initializeId();
  user2.initializeId();
  
  print(user1.getDisplayName());
  print(user2.getDisplayName());
  
  user1.sendEmail('Welcome!');
  user2.sendEmail('Welcome!');
}
```

### **Module 10: Modern Dart Features**

[⬅ Previous](topic-9-1-understanding-the-system.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-10-1-records-patterns.md)

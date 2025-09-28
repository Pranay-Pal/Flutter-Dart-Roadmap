# Topic 4.4: Collection Tools

[⬅ Previous](topic-4-3-maps.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-5-1-function-basics.md)

    * [ ] The spread operator (`...`) and collection `if`/`for`

#### The Spread Operator and Collection If/For

Dart provides powerful tools to work with collections more expressively and concisely.

**Spread operator (`...`):**
```dart
void main() {
  // Basic spread operator usage
  List<int> numbers1 = [1, 2, 3];
  List<int> numbers2 = [4, 5, 6];
  List<int> combined = [...numbers1, ...numbers2];
  
  print('Numbers 1: $numbers1');
  print('Numbers 2: $numbers2');
  print('Combined: $combined');
  
  // Adding individual elements with spread
  List<String> fruits = ['apple', 'banana'];
  List<String> moreFruits = ['orange', ...fruits, 'grape', 'kiwi'];
  print('More fruits: $moreFruits');
  
  // Null-aware spread operator (...?)
  List<int>? nullableList = null;
  List<int> safeList = [1, 2, ...?nullableList, 3, 4];
  print('Safe list: $safeList');
  
  // Spread with sets
  Set<String> colors1 = {'red', 'green'};
  Set<String> colors2 = {'blue', 'yellow'};
  Set<String> allColors = {...colors1, ...colors2, 'purple'};
  print('All colors: $allColors');
  
  // Spread with maps
  Map<String, int> map1 = {'a': 1, 'b': 2};
  Map<String, int> map2 = {'c': 3, 'd': 4};
  Map<String, int> combinedMap = {...map1, ...map2, 'e': 5};
  print('Combined map: $combinedMap');
}
```

**Collection if (conditional elements):**
```dart
void main() {
  bool includeBonusItems = true;
  bool isVip = false;
  int userLevel = 5;
  
  // List with conditional elements
  List<String> items = [
    'basic_item',
    'standard_item',
    if (includeBonusItems) 'bonus_item',
    if (isVip) 'vip_item',
    if (userLevel >= 5) 'advanced_item',
    if (userLevel >= 10) 'expert_item',
  ];
  
  print('Items: $items');
  
  // Set with conditional elements
  Set<String> permissions = {
    'read',
    if (userLevel >= 3) 'write',
    if (userLevel >= 5) 'modify',
    if (userLevel >= 8) 'delete',
    if (isVip) 'admin',
  };
  
  print('Permissions: $permissions');
  
  // Map with conditional entries
  Map<String, dynamic> userProfile = {
    'name': 'John Doe',
    'level': userLevel,
    if (isVip) 'membership': 'VIP',
    if (userLevel >= 5) 'badge': 'Advanced User',
    if (includeBonusItems) 'bonusPoints': 100,
  };
  
  print('User profile: $userProfile');
}
```

**Collection for (loops in collections):**
```dart
void main() {
  // Generate list using for loop
  List<int> squares = [
    for (int i = 1; i <= 5; i++) i * i
  ];
  print('Squares: $squares');
  
  // Generate list from another collection
  List<String> names = ['alice', 'bob', 'charlie'];
  List<String> upperNames = [
    for (String name in names) name.toUpperCase()
  ];
  print('Upper names: $upperNames');
  
  // Conditional generation
  List<int> evenNumbers = [
    for (int i = 1; i <= 10; i++)
      if (i % 2 == 0) i
  ];
  print('Even numbers: $evenNumbers');
  
  // Complex generation with multiple conditions
  List<String> products = ['laptop', 'phone', 'tablet', 'watch'];
  List<double> prices = [999.99, 599.99, 399.99, 299.99];
  
  List<String> expensiveProducts = [
    for (int i = 0; i < products.length; i++)
      if (prices[i] > 500) '${products[i]} (\$${prices[i]})'
  ];
  print('Expensive products: $expensiveProducts');
  
  // Generate set with for loop
  Set<String> uniqueFirstLetters = {
    for (String name in names) name[0].toUpperCase()
  };
  print('Unique first letters: $uniqueFirstLetters');
  
  // Generate map with for loop
  Map<String, int> nameLengths = {
    for (String name in names) name: name.length
  };
  print('Name lengths: $nameLengths');
}
```

**Combining spread, if, and for:**
```dart
void main() {
  List<String> basicFeatures = ['login', 'profile'];
  List<String> premiumFeatures = ['analytics', 'export'];
  List<String> adminFeatures = ['user_management', 'system_config'];
  
  bool isPremium = true;
  bool isAdmin = false;
  List<String> extraFeatures = ['notifications', 'themes'];
  
  // Complex collection construction
  List<String> availableFeatures = [
    ...basicFeatures,
    if (isPremium) ...premiumFeatures,
    if (isAdmin) ...adminFeatures,
    ...extraFeatures,
    for (int i = 1; i <= 3; i++) 'feature_$i',
    if (isPremium)
      for (String feature in ['advanced_search', 'priority_support'])
        feature,
  ];
  
  print('Available features: $availableFeatures');
  
  // Building user interface elements
  bool showHeader = true;
  bool showFooter = true;
  List<String> menuItems = ['home', 'about', 'contact'];
  
  List<String> pageElements = [
    if (showHeader) 'header',
    'main_content',
    for (String item in menuItems) 'menu_$item',
    if (showFooter) 'footer',
    for (int i = 1; i <= 2; i++)
      if (isPremium) 'premium_widget_$i',
  ];
  
  print('Page elements: $pageElements');
}
```

**Practical examples combining all collection tools:**
```dart
void main() {
  // E-commerce cart system
  Map<String, double> inventory = {
    'laptop': 999.99,
    'mouse': 29.99,
    'keyboard': 79.99,
    'monitor': 299.99,
    'webcam': 89.99,
  };
  
  List<String> cartItems = ['laptop', 'mouse'];
  bool hasDiscount = true;
  bool isMember = true;
  double discountRate = 0.1;
  double membershipDiscount = 0.05;
  
  // Calculate order summary
  List<Map<String, dynamic>> orderItems = [
    for (String item in cartItems)
      if (inventory.containsKey(item))
        {
          'name': item,
          'price': inventory[item]!,
          'discountedPrice': inventory[item]! * 
            (1 - (hasDiscount ? discountRate : 0) - 
             (isMember ? membershipDiscount : 0))
        }
  ];
  
  print('Order items:');
  for (var item in orderItems) {
    print('${item['name']}: \$${item['price']} -> \$${item['discountedPrice']?.toStringAsFixed(2)}');
  }
  
  // Generate report
  double totalOriginal = orderItems.fold(0, (sum, item) => sum + item['price']);
  double totalDiscounted = orderItems.fold(0, (sum, item) => sum + item['discountedPrice']);
  double savings = totalOriginal - totalDiscounted;
  
  Map<String, dynamic> orderSummary = {
    'items': orderItems.length,
    'originalTotal': totalOriginal,
    'finalTotal': totalDiscounted,
    'savings': savings,
    if (hasDiscount) 'discount': '${(discountRate * 100).toInt()}%',
    if (isMember) 'membershipDiscount': '${(membershipDiscount * 100).toInt()}%',
    ...if (savings > 50) {'specialOffer': 'Free shipping!'},
  };
  
  print('\nOrder Summary: $orderSummary');
}
```

### **Module 5: Functions (Reusable Code)**

[⬅ Previous](topic-4-3-maps.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-5-1-function-basics.md)

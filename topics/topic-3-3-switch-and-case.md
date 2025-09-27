# Topic 3.3: Switch and Case

    * [ ] Traditional `switch` statements for flow control

#### Traditional Switch Statements

Switch statements provide a clean way to handle multiple conditions based on a single value.

**Basic switch statement:**
```dart
void main() {
  int dayOfWeek = 3;
  
  switch (dayOfWeek) {
    case 1:
      print('Monday - Start of the work week');
      break;
    case 2:
      print('Tuesday - Getting into the groove');
      break;
    case 3:
      print('Wednesday - Hump day!');
      break;
    case 4:
      print('Thursday - Almost there');
      break;
    case 5:
      print('Friday - TGIF!');
      break;
    case 6:
      print('Saturday - Weekend time');
      break;
    case 7:
      print('Sunday - Rest day');
      break;
    default:
      print('Invalid day number');
  }
}
```

**Switch with strings:**
```dart
void main() {
  String grade = 'B';
  
  switch (grade.toUpperCase()) {
    case 'A':
      print('Excellent! Keep up the great work!');
      break;
    case 'B':
      print('Good job! You\'re doing well.');
      break;
    case 'C':
      print('Average. There\'s room for improvement.');
      break;
    case 'D':
      print('Below average. Please study harder.');
      break;
    case 'F':
      print('Failed. You need to retake the exam.');
      break;
    default:
      print('Invalid grade entered.');
  }
}
```

**Multiple cases with same action:**
```dart
void main() {
  String month = 'December';
  
  switch (month.toLowerCase()) {
    case 'december':
    case 'january':
    case 'february':
      print('$month is a winter month');
      break;
    case 'march':
    case 'april':
    case 'may':
      print('$month is a spring month');
      break;
    case 'june':
    case 'july':
    case 'august':
      print('$month is a summer month');
      break;
    case 'september':
    case 'october':
    case 'november':
      print('$month is a fall/autumn month');
      break;
    default:
      print('Invalid month name');
  }
}
```

**Switch in functions:**
```dart
String getOperationResult(String operation, double a, double b) {
  switch (operation) {
    case '+':
      return '${a + b}';
    case '-':
      return '${a - b}';
    case '*':
      return '${a * b}';
    case '/':
      if (b != 0) {
        return '${a / b}';
      } else {
        return 'Error: Division by zero';
      }
    default:
      return 'Error: Unknown operation';
  }
}

void main() {
  print('Calculator:');
  print('5 + 3 = ${getOperationResult('+', 5, 3)}');
  print('10 - 4 = ${getOperationResult('-', 10, 4)}');
  print('6 * 7 = ${getOperationResult('*', 6, 7)}');
  print('15 / 3 = ${getOperationResult('/', 15, 3)}');
  print('10 / 0 = ${getOperationResult('/', 10, 0)}');
  print('5 % 2 = ${getOperationResult('%', 5, 2)}');
}
```

**Advanced switch usage:**
```dart
enum Priority { low, medium, high, urgent }

void main() {
  Priority taskPriority = Priority.high;
  
  switch (taskPriority) {
    case Priority.low:
      print('📝 Low priority - Handle when convenient');
      break;
    case Priority.medium:
      print('📋 Medium priority - Handle within a day');
      break;
    case Priority.high:
      print('⚡ High priority - Handle within hours');
      break;
    case Priority.urgent:
      print('🚨 Urgent - Handle immediately!');
      break;
  }
  
  // Switch with complex conditions
  processHttpStatusCode(200);
  processHttpStatusCode(404);
  processHttpStatusCode(500);
}

void processHttpStatusCode(int statusCode) {
  switch (statusCode) {
    case 200:
    case 201:
    case 202:
      print('✅ Success: Request completed successfully');
      break;
    case 400:
    case 401:
    case 403:
    case 404:
      print('❌ Client Error: Status code $statusCode');
      break;
    case 500:
    case 502:
    case 503:
      print('🔥 Server Error: Status code $statusCode');
      break;
    default:
      if (statusCode >= 100 && statusCode < 200) {
        print('ℹ️ Informational: Status code $statusCode');
      } else if (statusCode >= 300 && statusCode < 400) {
        print('↩️ Redirection: Status code $statusCode');
      } else {
        print('❓ Unknown status code: $statusCode');
      }
  }
}
```

### **Module 4: Collections (Grouping Data)**

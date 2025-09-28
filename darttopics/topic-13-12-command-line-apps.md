# Topic 13.12: Building Command-Line Applications

[⬅ Previous](topic-13-11-server-side-dart.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-14-1-writing-quality-code.md)

Dart is an excellent choice for building fast, native command-line interface (CLI) applications. The core libraries and a rich ecosystem of packages provide all the tools you need to create powerful and user-friendly utilities.

This topic covers the essential packages for CLI development:
- [ ] Parsing Command-Line Arguments with `package:args`.
- [ ] Automatically Generating Documentation with `package:dartdoc`.
- [ ] Reading Configuration Files with `package:cli_config`.
- [ ] Common CLI Utilities with `package:cli_util`.

---

### 1. Parsing Command-Line Arguments with `package:args`

Nearly every CLI application needs to accept arguments and flags from the user. `package:args` provides a robust `ArgParser` class to define the arguments you expect and parse them from the command line.

**`pubspec.yaml`**:
```yaml
dependencies:
  args: ^2.4.2
```

**Example: A CLI with options, flags, and multiple values**
```dart
import 'package:args/args.dart';
import 'dart:io';

void main(List<String> arguments) {
  final parser = ArgParser()
    ..addOption('mode', abbr: 'm', defaultsTo: 'dev', help: 'The mode to run in.')
    ..addMultiOption('include', abbr: 'i', help: 'Directories to include in processing.')
    ..addFlag('verbose', abbr: 'v', negatable: false, help: 'Enable verbose logging.')
    ..addFlag('help', abbr: 'h', negatable: false, help: 'Show this help message.');

  try {
    final results = parser.parse(arguments);

    if (results['help']) {
      print('A simple example CLI.');
      print(parser.usage);
      exit(0);
    }

    final mode = results['mode'];
    final verbose = results['verbose'];
    final includes = results['include'] as List<String>;

    print('Running in mode: $mode');
    if (verbose) {
      print('Verbose logging is enabled.');
    }

    if (includes.isNotEmpty) {
      print('Included directories:');
      for (final dir in includes) {
        print('- $dir');
      }
    }

  } on FormatException catch (e) {
    print(e.message);
    print('');
    print(parser.usage);
    exit(1);
  }
}
```
**How to run it:**
- `dart run your_app.dart -m prod -v --include=./lib --include=./test`
- `dart run your_app.dart --help`

---

### 2. Generating API Documentation with `package:dartdoc`

Good documentation is crucial for any project. `package:dartdoc` is the official tool for generating API reference documentation from your Dart source code. It parses your `///` doc comments and generates a beautiful, searchable HTML website.

You don't need to add it to `pubspec.yaml`. You can run it directly from your terminal.

**How to use it:**
1. **Write doc comments:** Add `///` comments to your public classes, methods, and functions.
2. **Run the tool:** In your project's root directory, run the command:
   ```bash
   dart doc .
   ```
3. **View the output:** This will generate a `doc/api` directory containing the HTML documentation. Open `doc/api/index.html` in your browser.

---

### 3. Reading Configuration Files with `package:cli_config`

For complex applications, you may want to allow users to specify configuration in a file (e.g., `config.yaml`) instead of passing dozens of command-line arguments. `package:cli_config` intelligently merges settings from a configuration file with arguments passed on the command line, with command-line arguments taking precedence.

**`pubspec.yaml`**:
```yaml
dependencies:
  cli_config: ^0.1.1
```

**Example:**
Imagine a `config.yaml` file in your project's root directory:
```yaml
# config.yaml
# Default configuration for our application.
log-level: info
output-dir: ./build
```

Your Dart application can then read these values, but allow users to override them from the command line.

```dart
import 'package:cli_config/cli_config.dart';

void main(List<String> arguments) async {
  // Suppose the user runs the app with an overriding argument:
  // dart run your_app.dart --log-level=debug

  final config = await Config.fromArgs(
    args: arguments,
    // The tool automatically finds and parses 'config.yaml'.
    // It also maps kebab-case keys from the file to camelCase variables.
  );

  // The value from the command-line (--log-level=debug) overrides the file's value.
  final logLevel = config.getString('log-level');
  print('Log Level: $logLevel'); // Prints: debug

  // The value from the config file is used as a fallback since it wasn't passed via CLI.
  final outputDir = config.getString('output-dir');
  print('Output Directory: $outputDir'); // Prints: ./build
}
```
This powerful feature allows you to provide sensible defaults in a file while giving users the flexibility to override them for specific tasks.

---

### 4. Common CLI Utilities with `package:cli_util`

`package:cli_util` provides a set of common utilities for CLI applications, most notably for finding the Dart SDK directory.

**`pubspec.yaml`**:
```yaml
dependencies:
  cli_util: ^0.4.0
```

**Example: Finding the Dart SDK**
```dart
import 'package:cli_util/cli_util.dart';

void main() {
  // getSdkPath() finds the currently running Dart SDK.
  // This is useful if your tool needs to invoke other Dart tools like 'dart format' or 'dart analyze'.
  final sdkPath = getSdkPath();
  
  print('Dart SDK is located at: $sdkPath');
}
```

[⬅ Previous](topic-13-11-server-side-dart.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-14-1-writing-quality-code.md)

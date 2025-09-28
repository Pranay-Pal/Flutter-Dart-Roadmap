# Topic 13.6: Input/Output (`dart:io`) - Part 1: Files, Processes, and Platform

[⬅ Previous](topic-13-5-advanced-async.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-7-io-networking.md)

The `dart:io` library is your gateway to interacting with the underlying operating system from non-web applications (like command-line tools, servers, or Flutter apps). This section covers file system access, running external programs, and accessing platform-specific information.

**Note**: `dart:io` is not available in web projects, as browsers sandbox execution and prevent direct OS access.

- [x] File System Access: Reading and writing files with `File`.
- [x] Directory and Link Management: `Directory` and `Link`.
- [x] Running External Processes: `Process.run` and `Process.start`.
- [x] Accessing Platform Information: `Platform` class.

---

### 1. File System Access with `File`

The `File` class provides an interface to files. Most of its methods have both synchronous and asynchronous versions (e.g., `writeAsStringSync` vs. `writeAsString`). **The async versions are almost always preferred** to avoid blocking your program's execution.

```dart
import 'dart:io';
import 'dart:convert';

void main() async {
  final myFile = File('hello.txt');

  // 1. Write to a file (asynchronously).
  // This creates the file if it doesn't exist or overwrites it if it does.
  await myFile.writeAsString('Hello from Dart!\n');
  print('Wrote to ${myFile.path}');

  // 2. Append to a file.
  await myFile.writeAsString('Dart is a client-optimized language.\n', mode: FileMode.append);
  print('Appended to file.');

  // 3. Read the entire file content as a string.
  final contents = await myFile.readAsString();
  print('\nRead from file:\n$contents');

  // 4. For large files, read as a stream to avoid high memory usage.
  print('Reading file as a stream of lines:');
  final stream = myFile.openRead().transform(utf8.decoder).transform(LineSplitter());
  await for (final line in stream) {
    print('  - Line: $line');
  }
  
  // 5. Get file metadata.
  if (await myFile.exists()) {
    print('\nFile exists. Size: ${await myFile.length()} bytes.');
    print('Last modified: ${await myFile.lastModified()}');
  }

  // 6. Delete the file.
  await myFile.delete();
  print('\nDeleted the file.');
}
```

---

### 2. Directory and Link Management

- **`Directory`**: Represents a directory/folder. Use it to create, list, rename, and delete directories.
- **`Link`**: Represents a symbolic link (symlink).

```dart
import 'dart:io';

void main() async {
  final tempDir = Directory('my_temp_directory');

  try {
    // 1. Create a directory. The `recursive: true` flag creates parent directories if needed.
    if (!await tempDir.exists()) {
      await tempDir.create(recursive: true);
      print('Created directory: ${tempDir.path}');
    }

    // 2. List contents of a directory.
    print('\nListing contents of current directory:');
    final currentDir = Directory.current;
    // `listSync` is acceptable in simple scripts, but `list` (async) is better for apps.
    for (final entity in currentDir.listSync()) {
      final type = entity is File ? 'File' : (entity is Directory ? 'Dir' : 'Link');
      print('  - ${entity.path} ($type)');
    }

  } finally {
    // 3. Clean up by deleting the directory.
    if (await tempDir.exists()) {
      // 'recursive: true' is required to delete a non-empty directory.
      await tempDir.delete(recursive: true);
      print('\nDeleted directory: ${tempDir.path}');
    }
  }
}
```

---

### 3. Running External Processes

Dart can execute shell commands and other programs using the `Process` class.

- **`Process.run()`**: Executes a command, waits for it to finish, and returns a `ProcessResult` containing the exit code, `stdout`, and `stderr`. It's simple and best for quick, non-interactive commands.
- **`Process.start()`**: Starts a process and returns a `Future<Process>`. This is more powerful as it allows you to interact with the process's I/O streams (`stdin`, `stdout`, `stderr`) while it's running.

```dart
import 'dart:io';
import 'dart:convert';

void main() async {
  // --- Example 1: Process.run() ---
  print('--- Running "git --version" with Process.run() ---');
  // The command and its arguments are passed as separate strings to avoid shell injection vulnerabilities.
  final runResult = await Process.run('git', ['--version']);
  
  if (runResult.exitCode == 0) {
    print('Success! Git version: ${runResult.stdout.trim()}');
  } else {
    print('Error! Exit code: ${runResult.exitCode}');
    print('STDERR: ${runResult.stderr}');
  }

  // --- Example 2: Process.start() for interactive I/O ---
  print('\n--- Running "ping" with Process.start() ---');
  final process = await Process.start('ping', ['-c', '4', 'google.com']);

  // Listen to the process's standard output and error streams simultaneously.
  process.stdout.transform(utf8.decoder).listen((line) => print('STDOUT: ${line.trim()}'));
  process.stderr.transform(utf8.decoder).listen((line) => print('STDERR: ${line.trim()}'));

  // Wait for the process to exit and get its exit code.
  final exitCode = await process.exitCode;
  print('\nPing process exited with code: $exitCode');
}
```

---

### 4. Accessing Platform Information

The `Platform` class provides static properties to identify the operating system and access environment variables. This is essential for writing cross-platform code.

```dart
import 'dart:io';

void main() {
  print('--- Platform Information ---');
  
  // 1. Get the operating system.
  print('Operating System: ${Platform.operatingSystem}'); // e.g., 'windows', 'linux', 'macos'
  
  // 2. Write platform-specific code.
  if (Platform.isWindows) {
    print('This is a Windows machine.');
  } else if (Platform.isMacOS) {
    print('This is a macOS machine.');
  } else if (Platform.isLinux) {
    print('This is a Linux machine.');
  }

  // 3. Get the path to the Dart executable.
  print('Dart executable: ${Platform.executable}');

  // 4. Access environment variables.
  // This is a Map of all environment variables.
  Map<String, String> envVars = Platform.environment;
  print('\n--- Environment Variables ---');
  print('PATH: ${envVars['PATH']?.substring(0, 50)}...');
  print('HOME or USERPROFILE: ${envVars['HOME'] ?? envVars['USERPROFILE']}');
}
```

[⬅ Previous](topic-13-5-advanced-async.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-7-io-networking.md)

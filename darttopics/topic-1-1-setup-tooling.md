# Topic 1.1: Setup & Tooling

    * [ ] Installing the Dart SDK
    * [ ] Configuring a code editor (e.g., VS Code with Dart extension)

#### Installing the Dart SDK

Dart is a programming language developed by Google that can be used for web, mobile, desktop, and server applications. To get started, you need to install the Dart SDK.

**Option 1: Using the Dart installer (Recommended for beginners)**
1. Visit [dart.dev/get-dart](https://dart.dev/get-dart)
2. Download the installer for your operating system
3. Run the installer and follow the instructions

**Option 2: Using package managers**

**Windows (using Chocolatey):**
```bash
choco install dart-sdk
```

**macOS (using Homebrew):**
```bash
brew tap dart-lang/dart
brew install dart
```

**Linux (using apt):**
```bash
sudo apt update
sudo apt install apt-transport-https
wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /usr/share/keyrings/dart.gpg
echo 'deb [signed-by=/usr/share/keyrings/dart.gpg arch=amd64] https://storage.googleapis.com/download.dartlang.org/linux/debian stable main' | sudo tee /etc/apt/sources.list.d/dart_stable.list
sudo apt update
sudo apt install dart
```

**Verify Installation:**
After installation, verify Dart is installed correctly:
```bash
dart --version
```
You should see output like: `Dart SDK version: 3.2.0 (stable)`

#### Configuring a Code Editor

**Visual Studio Code (Recommended)**
1. Download and install [VS Code](https://code.visualstudio.com/)
2. Install the "Dart" extension by Dart Code
3. The extension provides:
   - Syntax highlighting
   - Code completion
   - Debugging support
   - Linting and error detection

**Other Editor Options:**
- **IntelliJ IDEA/Android Studio**: Install the Dart plugin
- **Vim/Neovim**: Use the coc-dart plugin
- **Emacs**: Use dart-mode

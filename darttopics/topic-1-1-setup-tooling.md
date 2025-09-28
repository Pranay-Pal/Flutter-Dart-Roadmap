# Topic 1.1: Setup & Tooling

[⬅ Previous](topic-13-3-where-to-go-next.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-1-2-your-first-program.md)

Welcome to your Dart journey! The first step is to set up your development environment. This involves two key components: the **Dart SDK (Software Development Kit)** and a **code editor**.

- **The Dart SDK** is a collection of tools you'll need to write, compile, and run Dart code. It includes the Dart virtual machine (VM), libraries, and a command-line tool (`dart`) for managing your projects.
- **A Code Editor** is where you'll write your code. A good editor provides syntax highlighting, code completion, and debugging features to make your life easier.

---

### 1. Installing the Dart SDK

The official Dart website provides the easiest way to get the SDK.

**Option 1: Using the Dart Installer (Recommended)**
1.  Visit the official guide at [dart.dev/get-dart](https://dart.dev/get-dart).
2.  Download the installer for your operating system (Windows, macOS, or Linux).
3.  Run the installer and follow the on-screen instructions.

**Option 2: Using a Package Manager (for Command-Line Users)**

If you're comfortable with the command line, you can use a package manager.

**Windows (with Chocolatey):**
```bash
# This command installs the latest stable version of the Dart SDK.
choco install dart-sdk
```

**macOS (with Homebrew):**
```bash
# First, tap the Dart language repository.
brew tap dart-lang/dart
# Then, install Dart.
brew install dart
```

**Linux (with apt-get):**
```bash
# These commands add the Google signing key and Dart repository, then install the SDK.
sudo apt-get update
sudo apt-get install apt-transport-https
wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /usr/share/keyrings/dart.gpg
echo 'deb [signed-by=/usr/share/keyrings/dart.gpg arch=amd64] https://storage.googleapis.com/download.dartlang.org/linux/debian stable main' | sudo tee /etc/apt/sources.list.d/dart_stable.list
sudo apt-get update
sudo apt-get install dart
```

#### Verify the Installation
Once installed, open a new terminal or command prompt and run:
```bash
dart --version
```
You should see output confirming the SDK version, like `Dart SDK version: 3.4.3 (stable) ...`.

> **Note:** If the `dart` command is not found, you may need to add the Dart SDK's `bin` directory to your system's `PATH` environment variable. The installer usually handles this, but manual configuration is sometimes necessary.

---

### 2. Configuring a Code Editor

While you can write Dart in any text editor, using an editor with dedicated Dart support is highly recommended.

**Visual Studio Code (Recommended)**
1.  Download and install [VS Code](https://code.visualstudio.com/)—it's free and powerful.
2.  Open VS Code and go to the **Extensions** view (click the icon on the left sidebar).
3.  Search for `Dart` and install the official extension published by **Dart Code**.

This extension gives you powerful features like:
-   **Syntax Highlighting**: Makes your code readable with colors.
-   **Code Completion (IntelliSense)**: Suggests code as you type.
-   **Error Highlighting**: Shows you mistakes before you even run the code.
-   **Debugging Tools**: Helps you find and fix bugs step-by-step.

**Other Great Options:**
-   **IntelliJ IDEA** or **Android Studio**: Install the `Dart` plugin from the JetBrains marketplace.
-   **Vim/Neovim**: Use a plugin like `coc-dart` for a modern development experience.

---

### 3. Create and Run Your First Dart Project

With the SDK and editor ready, let's create a simple "Hello, World!" application to ensure everything works.

1.  **Create a new project:** Open your terminal and run the following command. The `dart create` command scaffolds a simple project structure for you.
    ```bash
    dart create hello_dart
    ```

2.  **Navigate into the project directory:**
    ```bash
    cd hello_dart
    ```

3.  **Run the application:** The `bin/` directory contains the main entry point of your app. Use `dart run` to execute it.
    ```bash
    dart run
    ```

You should see the following output in your console:
```
Hello world: 42!
```

Congratulations! Your Dart development environment is now fully configured.

[⬅ Previous](topic-13-3-where-to-go-next.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-1-2-your-first-program.md)

# Topic 14.2: Compilation and Deployment

[⬅ Previous](topic-14-1-writing-quality-code.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-14-3-where-to-go-next.md)

Once you've written and tested your application, the next step is to compile and deploy it. Dart is a uniquely flexible language that supports two different compilation strategies: **Just-In-Time (JIT)** and **Ahead-Of-Time (AOT)**. Understanding the difference is key to optimizing your development workflow and achieving high performance in production.

This topic covers Dart's compilation models and how to package a server-side application for deployment using Docker.

- [x] Understanding JIT (development) vs. AOT (production) compilation
- [x] Compiling for different targets (native vs. web)
- [x] Packaging a Dart server application with Docker

---

### 1. JIT vs. AOT Compilation

Dart can be compiled in two different ways, each suited for a different phase of the development lifecycle.

#### Just-In-Time (JIT) Compilation

- **How it works**: The Dart VM (Virtual Machine) runs your source code directly. It starts executing the code right away and compiles functions to native machine code *just in time* as they are called. The VM can also perform optimizations at runtime based on how the code is actually used.
- **When to use it**: During **development**.
- **Key Feature**: **Stateful Hot Reload**. The JIT compiler can inject new code into a running application, allowing you to see your changes instantly without restarting the app or losing your current state. This is a cornerstone of the fast and iterative Flutter development experience.
- **Command**: `dart run <file.dart>`

#### Ahead-Of-Time (AOT) Compilation

- **How it works**: Your Dart code is fully compiled to optimized, native machine code *before* the application is executed. The output is a self-contained executable file that can be run without the Dart SDK.
- **When to use it**: For **production** releases.
- **Key Features**:
  - **Fast Startup**: The app starts almost instantly because the code is already compiled.
  - **Predictable Performance**: All code is optimized at compile time, leading to consistently fast and smooth execution.
  - **Smaller Footprint**: The final executable does not need to include the Dart compiler or VM analysis tools, resulting in a smaller deployment size.
- **Command**: `dart compile exe <file.dart>`

**In Summary**:

| Feature             | JIT (Development)          | AOT (Production)           |
| ------------------- | -------------------------- | -------------------------- |
| **Speed**           | Fast development cycles    | Fast app startup & runtime |
| **Key Advantage**   | Stateful Hot Reload        | Optimized performance      |
| **Use Case**        | `dart run`                 | `dart compile`             |

---

### 2. Compiling for Different Targets

The `dart compile` command can produce different outputs depending on your target platform.

- **Native Executable**: Compiles your Dart code into a standalone executable for a specific operating system. This is ideal for command-line tools and backend servers.

  ```bash
  # Compile for the current operating system
  dart compile exe bin/my_app.dart -o my_app
  ```

- **JavaScript**: Compiles your Dart code to highly optimized JavaScript, allowing it to run in any modern web browser. This is how Dart web apps (including those built with Flutter) are deployed.

  ```bash
  # Compile for the web (output goes to a specified directory)
  dart compile js -o build/web/main.js web/main.dart
  ```

---

### 3. Packaging for Deployment with Docker

When you're ready to deploy a server-side Dart application, packaging it in a **Docker container** is a standard and effective approach. A container bundles your application and all its dependencies into a single, isolated, and portable unit.

Using a **multi-stage `Dockerfile`** is a best practice. This allows you to build the application in one stage (the `build` stage), which contains the full Dart SDK, and then copy only the compiled executable into a smaller, more secure final image (the `runtime` stage).

**Example: Dockerizing a Dart Server**

Here is a `Dockerfile` for a simple Dart server application.

```dockerfile
# Dockerfile

# --- Stage 1: Build the application ---
# Use the official Dart SDK image as the builder. This image contains all the
# tools needed to compile the application.
FROM dart:stable AS build

WORKDIR /app

# Copy in the pubspec files and get dependencies. This is done as a separate
# step to leverage Docker's layer caching. Dependencies are only re-fetched
# if pubspec.yaml or pubspec.lock changes.
COPY pubspec.* ./
RUN dart pub get

# Copy the rest of the application source code.
COPY . .

# Ensure the code is formatted and passes analysis. This is a good CI practice.
RUN dart format --output=none --set-exit-if-changed .
RUN dart analyze

# Compile the application to a native executable.
RUN dart compile exe bin/server.dart -o bin/server

# --- Stage 2: Create the final, small runtime image ---
# Use a minimal, "distroless" image that contains only the bare minimum
# OS libraries needed to run a native executable. This improves security
# by reducing the attack surface.
FROM gcr.io/distroless/cc-debian12 AS runtime

WORKDIR /app

# Copy the compiled executable from the build stage.
COPY --from=build /app/bin/server .

# Expose the port the server will listen on (update if your server uses a different port).
EXPOSE 8080

# The command to run when the container starts.
CMD ["./server"]
```

**To build and run this Docker container**:

```bash
# 1. Build the Docker image and tag it as 'my-dart-server'.
docker build -t my-dart-server .

# 2. Run the container, mapping port 8080 on your machine to port 8080 in the container.
docker run -p 8080:8080 my-dart-server
```

This process creates a highly optimized and secure container, ready for deployment to any cloud provider or server that supports Docker.

[⬅ Previous](topic-13-1-writing-quality-code.md) · [🏠 Roadmap](../The-Dart-Roadmap.md) · [Next ➡](topic-13-3-where-to-go-next.md)

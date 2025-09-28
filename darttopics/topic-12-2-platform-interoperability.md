# Topic 12.2: Platform Interoperability

[⬅ Previous](topic-12-1-the-professional-toolchain.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-13-1-writing-quality-code.md)

    * [ ] Calling C code with Foreign Function Interface (FFI)
    * [ ] Interacting with Kotlin/Java and Swift/Objective-C code

#### FFI Example

```dart
import 'dart:ffi';
import 'dart:io';

// C function signature: int add(int a, int b)
typedef AddC = Int32 Function(Int32 a, Int32 b);
typedef AddDart = int Function(int a, int b);

void main() {
  // Load the dynamic library
  final dylib = Platform.isWindows 
      ? DynamicLibrary.open('math_lib.dll')
      : DynamicLibrary.open('libmath_lib.so');
  
  // Look up the function
  final add = dylib.lookupFunction<AddC, AddDart>('add');
  
  // Call the C function
  final result = add(5, 3);
  print('5 + 3 = $result');
}
```

#### Interacting with Kotlin/Java and Swift/Objective-C (Flutter `MethodChannel`)

```dart
// lib/platform_channels/device_info.dart
import 'package:flutter/services.dart';

class NativeDeviceInfo {
  static const _channel = MethodChannel('app.device/info');

  static Future<String> getBatteryLevel() async {
    try {
      final level = await _channel.invokeMethod<int>('getBatteryLevel');
      return level == null ? 'Unavailable' : '$level%';
    } on PlatformException catch (e) {
      return 'Battery info error: ${e.message}';
    }
  }
}

// Usage inside a widget
// final battery = await NativeDeviceInfo.getBatteryLevel();
```

```kotlin
// android/app/src/main/kotlin/com/example/app/MainActivity.kt
import io.flutter.embedding.android.FlutterActivity
import io.flutter.embedding.engine.FlutterEngine
import io.flutter.plugin.common.MethodChannel

class MainActivity : FlutterActivity() {
  private val CHANNEL = "app.device/info"

  override fun configureFlutterEngine(flutterEngine: FlutterEngine) {
    super.configureFlutterEngine(flutterEngine)

    MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL)
      .setMethodCallHandler { call, result ->
        when (call.method) {
          "getBatteryLevel" -> {
            val battery = queryBatteryPercentage()
            if (battery != null) {
              result.success(battery)
            } else {
              result.error("UNAVAILABLE", "Battery level not available", null)
            }
          }
          else -> result.notImplemented()
        }
      }
  }

  private fun queryBatteryPercentage(): Int? {
    // Platform-specific API usage here
    return 87
  }
}
```

```swift
// ios/Runner/AppDelegate.swift
import Flutter
import UIKit

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {
    let controller = window?.rootViewController as! FlutterViewController
    let channel = FlutterMethodChannel(name: "app.device/info", binaryMessenger: controller.binaryMessenger)

    channel.setMethodCallHandler { call, result in
      switch call.method {
      case "getBatteryLevel":
        UIDevice.current.isBatteryMonitoringEnabled = true
        let level = Int(UIDevice.current.batteryLevel * 100)
        result(level)
      default:
        result(FlutterMethodNotImplemented)
      }
    }

    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

`MethodChannel` bridges Dart code to platform-specific implementations, making it easy to call existing Android/iOS APIs without leaving the Dart ecosystem.

### **Module 13: Best Practices & Next Steps**

[⬅ Previous](topic-12-1-the-professional-toolchain.md) · [🏠 Roadmap](../The Definitive Dart Learning Roadmap.md) · [Next ➡](topic-13-1-writing-quality-code.md)

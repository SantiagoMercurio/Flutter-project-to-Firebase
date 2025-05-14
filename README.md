## 🚀 How to Connect Flutter to Firebase

This guide will help you set up Firebase in your Flutter project for both Android and iOS.

### 1. Prerequisites

- Flutter installed ([Install guide](https://docs.flutter.dev/get-started/install))
- Dart SDK installed
- A Firebase account ([Sign up here](https://firebase.google.com/))
- The Firebase CLI and FlutterFire CLI installed

### 2. Add Firebase Dependencies

Add these to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.27.1
  firebase_auth: ^4.17.9
  cloud_firestore: ^4.15.9
```

Then run:

```sh
flutter pub get
```

### 3. Install the Firebase CLI and FlutterFire CLI

```sh
npm install -g firebase-tools
dart pub global activate flutterfire_cli
```

Add FlutterFire CLI to your PATH (if not already):

```sh
export PATH="$PATH":"$HOME/.pub-cache/bin"
```

### 4. Login to Firebase

```sh
firebase login
```

### 5. Configure Firebase for Your Project

From your project root, run:

```sh
flutterfire configure
```

- Select your Firebase project (or create a new one).
- Select the platforms you want to configure (android, ios, etc.).
- This will generate `lib/firebase_options.dart`.

### 6. Initialize Firebase in Your App

In your `lib/main.dart`:

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Firebase Demo',
      home: Scaffold(
        appBar: AppBar(title: const Text('Firebase Connected!')),
        body: const Center(child: Text('Hello Firebase!')),
      ),
    );
  }
}
```

### 7. Platform-Specific Setup

#### Android

- The `flutterfire configure` command will set your Android package name in `android/app/build.gradle.kts` (`applicationId` and `namespace`).
- Make sure your `google-services.json` is in `android/app/` (handled by FlutterFire CLI).

#### iOS

- The `flutterfire configure` command will set your iOS bundle identifier in `ios/Runner.xcodeproj/project.pbxproj`.
- Make sure your `GoogleService-Info.plist` is in `ios/Runner/` (handled by FlutterFire CLI).
- Open `ios/Runner.xcworkspace` in Xcode, and ensure you have the correct signing and capabilities.

### 8. Test Your Setup

Run your app:

```sh
flutter run
```

If everything is set up correctly, your app will initialize Firebase on startup.

---

## 🔗 Useful Links

- [FlutterFire Documentation](https://firebase.flutter.dev/docs/overview)
- [Flutter Documentation](https://docs.flutter.dev/)

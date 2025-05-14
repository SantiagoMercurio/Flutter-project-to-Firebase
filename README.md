# 🔥 Connecting Flutter to Firebase

This guide explains how to set up Firebase in your Flutter project using FlutterFire.

---

## ✅ Prerequisites

- Flutter SDK installed
- Firebase CLI installed: `npm install -g firebase-tools`
- Firebase account
- Dart SDK (`flutter` already includes it)
- FlutterFire CLI: `dart pub global activate flutterfire_cli`

---

## 🔧 Step-by-Step Setup

### 1. Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create a new project
3. Register your app (Android/iOS) with:
   - Android: `package name` (e.g., `com.example.myapp`)
   - iOS: `Bundle ID` (e.g., `com.example.myapp`)

---

### 2. Initialize FlutterFire

From your project root:

```bash
flutterfire configure
```

- Select your Firebase project
- Select platforms (Android/iOS)
- This generates `lib/firebase_options.dart`

---

### 3. Add Firebase Packages

In your `pubspec.yaml`:

```yaml
dependencies:
  firebase_core: ^2.27.0
  # Add additional packages as needed:
  firebase_auth: ^4.17.4
  cloud_firestore: ^4.14.0
```

Then run:

```bash
flutter pub get
```

---

### 4. Initialize Firebase in `main.dart`

```dart
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  runApp(MyApp());
}
```

---

## 📱 Android Configuration

1. In `android/build.gradle`:

```gradle
classpath 'com.google.gms:google-services:4.3.15'
```

2. In `android/app/build.gradle`:

```gradle
apply plugin: 'com.google.gms.google-services'
```

3. Place your `google-services.json` file inside `android/app/`.

---

## 🍎 iOS Configuration

1. Open `ios/Runner.xcworkspace` in Xcode.
2. Set your **Team** and **Bundle Identifier**.
3. In `ios/Podfile`:

```ruby
platform :ios, '11.0'
```

4. Place `GoogleService-Info.plist` inside `ios/Runner/`.

---

## ▶️ Run the App

```bash
flutter run
```

If everything is configured correctly, your Flutter app is now connected to Firebase!

---

## 🔌 Optional Packages

- **Authentication**: `firebase_auth`
- **Firestore**: `cloud_firestore`
- **Storage**: `firebase_storage`
- **Crashlytics**: `firebase_crashlytics`
- **Analytics**: `firebase_analytics`

---

# 🔥 Connect Flutter to Firebase (Complete Guide)
---

## ✅ Prerequisites

- Flutter installed → [Flutter Install Guide](https://docs.flutter.dev/get-started/install)
- Dart SDK (included with Flutter)
- Firebase account → [Sign up here](https://firebase.google.com/)
- Firebase CLI and FlutterFire CLI
- Android Studio and/or Xcode for platform builds

---

##  Step-by-Step Setup

### 1. Create a Firebase Project

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **"Add project"**
3. Enter a project name → Click **Continue**
4. Skip Google Analytics (optional)
5. Click **Create project**

![image](https://github.com/user-attachments/assets/fd3aa043-e94c-4429-901f-9717bab65e52)

---

### 2. Register Your Flutter App

#### Android

1. Click the **Android** icon in Firebase Console
2. Use your package name from `android/app/build.gradle` (e.g. `com.example.myapp`)
3. Download `google-services.json` and place it in `android/app/`

#### iOS

1. Click the **iOS** icon in Firebase Console
2. Use your bundle ID from `ios/Runner.xcodeproj/project.pbxproj`
3. Download `GoogleService-Info.plist` and place it in `ios/Runner/`

![image](https://github.com/user-attachments/assets/4e8f78d5-8922-4822-b8c0-22df65ad3003)

---

## 🔧 Install Required Tools

Install Firebase and FlutterFire CLI:

```bash
npm install -g firebase-tools
dart pub global activate flutterfire_cli
```

Then add FlutterFire CLI to your path:

```bash
export PATH="$PATH":"$HOME/.pub-cache/bin"
```

---

## 🔐 Login and Configure

```bash
firebase login
flutterfire configure
```

- Select your Firebase project
- Select platforms (Android/iOS)
- This will generate `lib/firebase_options.dart`

---

## 📦 Add Dependencies

In `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.27.1
  cloud_firestore: ^4.15.9
```

Install them:

```bash
flutter pub get
```

---

## 🚀 Initialize Firebase in `main.dart`

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
      title: 'Firebase Example',
      home: UsersPage(),
    );
  }
}
```

## 🛠 Platform-Specific Setup

### Android

Ensure:

- `google-services.json` is in `android/app/`
- `android/app/build.gradle` includes:

```gradle
defaultConfig {
  multiDexEnabled true
}

dependencies {
  implementation 'androidx.multidex:multidex:2.0.1'
}

apply plugin: 'com.google.gms.google-services'
```
---

### iOS

Ensure:

- `GoogleService-Info.plist` is in `ios/Runner/`
- Open with `ios/Runner.xcworkspace` in Xcode
- In `ios/Podfile`:

```ruby
platform :ios, '12.0'
```

Then run:

```bash
cd ios
pod install
```
---

## 🔥 Create a Firestore Collection

1. In Firebase Console, go to **Build → Firestore Database**
2. Click **Create database** (use test mode)
3. Click **Start collection**
   - Collection ID: `users`
4. Create a document with fields:
   - `name`: `"Alice"`
   - `email`: `"alice@example.com"`

![image](https://github.com/user-attachments/assets/b55c0e17-841f-43b5-99e9-c02b8350f4c3)
![image](https://github.com/user-attachments/assets/aec3ed32-6c8b-4f75-a3ab-9f070706cbd2)
![image](https://github.com/user-attachments/assets/bdc95a26-8367-404c-9cef-064e43bae922)


---

## 📄 Fetch Firestore Collection in Flutter

Create a file like `users_page.dart`:

```dart
import 'package:flutter/material.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

class UsersPage extends StatelessWidget {
  const UsersPage({super.key});

  @override
  Widget build(BuildContext context) {
    final usersRef = FirebaseFirestore.instance.collection('users');

    return Scaffold(
      appBar: AppBar(title: const Text('Users')),
      body: StreamBuilder<QuerySnapshot>(
        stream: usersRef.snapshots(),
        builder: (context, snapshot) {
          if (snapshot.hasError) return const Text('Error loading users');
          if (!snapshot.hasData) return const CircularProgressIndicator();

          final users = snapshot.data!.docs;

          return ListView.builder(
            itemCount: users.length,
            itemBuilder: (context, index) {
              final user = users[index].data() as Map<String, dynamic>;
              return ListTile(
                title: Text(user['name'] ?? 'No name'),
                subtitle: Text(user['email'] ?? 'No email'),
              );
            },
          );
        },
      ),
    );
  }
}
```

Then update your `home:` in `main.dart`:

```dart
home: UsersPage(),
```
---

## 🧪 Run the App

```bash
flutter run
```

If everything is set up correctly, you’ll see your Firestore users displayed in the app.

---

## 🔗 Useful Links

- [FlutterFire Docs](https://firebase.flutter.dev/)
- [Flutter Documentation](https://docs.flutter.dev/)

# Ajus Tacts — Flutter starter app

A cross-platform football learning app starter for Android and iOS, using Flutter and Firebase Authentication. It includes email/password registration and sign-in, a dashboard, introductory tactical lessons, football-law explainers and coach biographies.

## Requirements
- Flutter SDK (stable)
- A Firebase project
- Android Studio / Xcode for platform builds (iOS builds require macOS)

## Setup
1. Install Flutter and verify it with `flutter doctor`.
2. Create a Firebase project at https://console.firebase.google.com/.
3. Install the FlutterFire CLI and run `dart pub global activate flutterfire_cli`.
4. From this project folder run `flutterfire configure` and select your Firebase project and Android/iOS platforms. This generates `lib/firebase_options.dart`.
5. In `lib/main.dart`, uncomment `import 'firebase_options.dart';` and change `Firebase.initializeApp()` to `Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform)`.
6. In Firebase Console, enable Authentication → Sign-in method → Email/Password.
7. Create a Cloud Firestore database. Start in production mode and apply the rules below.
8. Run `flutter pub get`, then `flutter run`.

## Suggested Firestore security rules
These rules let signed-in users read learning content and only read/write their own user profile. Add admin-only content publishing before creating an editor/admin interface.

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow create: if request.auth != null && request.auth.uid == userId;
      allow read, update, delete: if request.auth != null && request.auth.uid == userId;
    }
    match /lessons/{lessonId} {
      allow read: if request.auth != null;
      allow write: if false;
    }
  }
}
```

## Current scope
The learning material is currently bundled in the app as starter content. Authentication is connected to Firebase once configured. The lesson library is not yet remotely managed, and quizzes, admin publishing, profile editing, password reset, analytics and app-store release setup are future development tasks.

## Important
Do not commit Firebase service credentials, private keys or admin SDK files to a public repository. The Firebase client configuration is not a substitute for secure Firestore rules.

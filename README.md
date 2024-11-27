## Configuration

Before using Firebase Simplifier, ensure that your Firebase project is properly set up and configured in your Flutter application.

### 1. Firebase Project Setup

#### Create a Firebase Project:

- Navigate to the [Firebase Console](https://console.firebase.google.com/).
- Click on "Add project" and follow the prompts to create a new project.

#### Enable Firebase Services:

- **Authentication**: Go to **Build > Authentication** and enable the desired sign-in methods (e.g., Email/Password, Google).
- **Firestore**: Navigate to **Build > Firestore Database** and create a database.
- **Realtime Database**: Go to **Build > Realtime Database** and set up your database.
- **Storage**: Navigate to **Build > Storage** and configure your storage bucket.

### 2. Add Firebase Configuration Files

#### Android:

- In the Firebase Console, go to **Project Settings > General > Your Apps > Add App > Android**.
- Register your app and download the `google-services.json` file.
- Place the `google-services.json` file in the `android/app/` directory of your Flutter project.

#### iOS:

- In the Firebase Console, go to **Project Settings > General > Your Apps > Add App > iOS**.
- Register your app and download the `GoogleService-Info.plist` file.
- Place the `GoogleService-Info.plist` file in the `ios/Runner/` directory of your Flutter project.

#### Web:

- If targeting web platforms, add the Firebase configuration object in your `web/index.html`:

```html
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
<script>
  const firebaseConfig = {
    apiKey: "your-api-key",
    authDomain: "your-auth-domain",
    databaseURL: "https://your-database-url.firebaseio.com",
    projectId: "your-project-id",
    storageBucket: "your-storage-bucket",
    messagingSenderId: "your-sender-id",
    appId: "your-app-id"
  };
  firebase.initializeApp(firebaseConfig);
</script>
```

### 3. Initialize Firebase in Your App

In your `main.dart`, initialize Firebase before running the app:

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_simplifier/firebase_simplifier.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Firebase Simplifier Example',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomePage(),
    );
  }
}
```

## Usage

Firebase Simplifier provides various utilities to manage Firebase services seamlessly. Below are detailed usage instructions for each service.

### Authentication

#### Email & Password

```dart
import 'package:firebase_simplifier/firebase_simplifier.dart';
import 'package:firebase_auth/firebase_auth.dart';

// Initialize Auth helper
final Auth auth = Auth();

// Sign in with email and password
Future<void> signIn(String email, String password) async {
  User? user = await auth.signInWithEmail(email, password);
  if (user != null) {
    print('Signed in as ${user.email}');
  } else {
    print('Sign-in failed');
  }
}

// Register a new user
Future<void> register(String email, String password) async {
  User? user = await auth.registerWithEmail(email, password);
  if (user != null) {
    print('Registered user: ${user.email}');
  } else {
    print('Registration failed');
  }
}

// Sign out
Future<void> signOut() async {
  await auth.signOut();
  print('User signed out');
}
```

#### Google Sign-In

```dart
import 'package:firebase_simplifier/firebase_simplifier.dart';
import 'package:firebase_auth/firebase_auth.dart';

// Initialize Auth helper
final Auth auth = Auth();

// Sign in with Google
Future<void> signInWithGoogle() async {
  User? user = await auth.signInWithGoogle();
  if (user != null) {
    print('Signed in with Google as ${user.email}');
  } else {
    print('Google sign-in canceled or failed');
  }
}
```

### Firestore

```dart
import 'package:firebase_simplifier/firebase_simplifier.dart';

// Initialize Firestore helper
final SimpleFirestore firestore = SimpleFirestore();

// Write data to Firestore
Future<void> writeFirestoreData(String collection, String docId, Map<String, dynamic> data) async {
  bool success = await firestore.writeData('$collection/$docId', data);
  if (success) {
    print('Data written to Firestore successfully.');
  } else {
    print('Failed to write data to Firestore.');
  }
}

// Read data from Firestore
Future<Map<String, dynamic>?> readFirestoreData(String collection, String docId) async {
  Map<String, dynamic>? data = await firestore.getData('$collection/$docId');
  if (data != null) {
    print('Data retrieved from Firestore: $data');
  } else {
    print('No data found at the specified path.');
  }
  return data;
}

// Delete data from Firestore
Future<void> deleteFirestoreData(String collection, String docId) async {
  bool success = await firestore.deleteData('$collection/$docId');
  if (success) {
    print('Data deleted from Firestore successfully.');
  } else {
    print('Failed to delete data from Firestore.');
  }
}

// Create a unique document
Future<void> createUniqueFirestoreDoc(String collection, Map<String, dynamic> data) async {
  String? docId = await firestore.createUniqueDocument(collection, data);
  if (docId != null) {
    print('Document created with ID: $docId');
  } else {
    print('Failed to create document.');
  }
}
```

### Realtime Database (RTDB)

```dart
import 'package:firebase_simplifier/firebase_simplifier.dart';
import 'package:firebase_auth/firebase_auth.dart';

// Initialize RTDB helper
final RTDB rtdb = RTDB(currentUser, 'https://your-database-url.firebaseio.com/');

// Write data to RTDB
Future<void> writeRTDBData(String path, Map<String, dynamic> data) async {
  bool success = await rtdb.writeData(path, data);
  if (success) {
    print('Data written to RTDB successfully.');
  } else {
    print('Failed to write data to RTDB.');
  }
}

// Read data from RTDB
Future<dynamic> readRTDBData(String path) async {
  dynamic data = await rtdb.getData(path);
  if (data != null) {
    print('Data retrieved from RTDB: $data');
  } else {
    print('No data found at the specified path.');
  }
  return data;
}

// Update data in RTDB
Future<void> updateRTDBData(String path, Map<String, dynamic> data) async {
  bool success = await rtdb.updateData(path, data);
  if (success) {
    print('Data updated in RTDB successfully.');
  } else {
    print('Failed to update data in RTDB.');
  }
}

// Delete data from RTDB
Future<void> deleteRTDBData(String path) async {
  bool success = await rtdb.deleteData(path);
  if (success) {
    print('Data deleted from RTDB successfully.');
  } else {
    print('Failed to delete data from RTDB.');
  }
}

// Observe real-time data changes
Future<void> observeRTDBValue(String path) async {
  ValueNotifier<dynamic> notifier = await rtdb.observeRealtimeDBValue<dynamic>(path);
  notifier.addListener(() {
    print('Real-time data at $path updated: ${notifier.value}');
  });
}
```

### Firebase Storage

```dart
import 'package:firebase_simplifier/firebase_simplifier.dart';
import 'dart:io';

// Initialize Storage helper
final BucketStorage storage = BucketStorage();

// Upload a single image
Future<void> uploadSingleImage(File imageFile, String userId) async {
  String? imageUrl = await storage.uploadImage(imageFile, userId);
  if (imageUrl != null) {
    print('Image uploaded successfully: $imageUrl');
  } else {
    print('Failed to upload image.');
  }
}

// Upload multiple images
Future<void> uploadMultipleImages(List<File> imageFiles, String userId) async {
  List<String> imageUrls = await storage.uploadMultipleImages(imageFiles, userId);
  print('Uploaded image URLs: $imageUrls');
}

// Get download URL for an image
Future<void> getImageDownloadUrl(String filePath) async {
  String? url = await storage.getImageUrl(filePath);
  if (url != null) {
    print('Download URL: $url');
  } else {
    print('Failed to retrieve download URL.');
  }
}

// Check if a file exists
Future<void> checkFileExists(String filePath) async {
  bool exists = await storage.checkIfFileExists(filePath);
  print(exists ? 'File exists.' : 'File does not exist.');
}

// Pick images using ImagePicker
Future<void> pickAndUploadImages(String userId) async {
  List<File> images = await storage.pickImages();
  if (images.isNotEmpty) {
    await uploadMultipleImages(images, userId);
  } else {
    print('No images selected.');
  }
}
```

## Examples

Below are example implementations demonstrating how to use Firebase Simplifier in your Flutter applications.

### Example 1: Authentication Flow

```dart
import 'package:flutter/material.dart';
import 'package:firebase_simplifier/firebase_simplifier.dart';
import 'package:firebase_auth/firebase_auth.dart';

class AuthExamplePage extends StatefulWidget {
  const AuthExamplePage({Key? key}) : super(key: key);

  @override
  _AuthExamplePageState createState() => _AuthExamplePageState();
}

class _AuthExamplePageState extends State<AuthExamplePage> {
  final Auth _auth = Auth();
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();

  User? _user;

  @override
  void initState() {
    super.initState();
    _user = _auth.getCurrentUser();
  }

  Future<void> _signIn() async {
    User? user = await _auth.signInWithEmail(
      _emailController.text,
      _passwordController.text,
    );
    setState(() {
      _user = user;
    });
  }

  Future<void> _register() async {
    User? user = await _auth.registerWithEmail(
      _emailController.text,
      _passwordController.text,
    );
    setState(() {
      _user = user;
    });
  }

  Future<void> _signOut() async {
    await _auth.signOut();
    setState(() {
      _user = null;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Auth Example'),
        ),
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: _user == null
              ? Column(
                  children: [
                    TextField(
                      controller: _emailController,
                      decoration: InputDecoration(labelText: 'Email'),
                    ),
                    TextField(
                      controller: _passwordController,
                      decoration: InputDecoration(labelText: 'Password'),
                      obscureText: true,
                    ),
                    ElevatedButton(
                      onPressed: _signIn,
                      child: Text('Sign In'),
                    ),
                    ElevatedButton(
                      onPressed: _register,
                      child: Text('Register'),
                    ),
                  ],
                )
              : Column(
                  children: [
                    Text('Signed in as ${_user!.email}'),
                    ElevatedButton(
                      onPressed: _signOut,
                      child: Text('Sign Out'),
                    ),
                  ],
                ),
        ));
  }
}
```

### Example 2: Firestore CRUD Operations

```dart
import 'package:flutter/material.dart';
import 'package:firebase_simplifier/firebase_simplifier.dart';

class FirestoreExamplePage extends StatefulWidget {
  const FirestoreExamplePage({Key? key}) : super(key: key);

  @override
  _FirestoreExamplePageState createState() => _FirestoreExamplePageState();
}

class _FirestoreExamplePageState extends State<FirestoreExamplePage> {
  final SimpleFirestore _firestore = SimpleFirestore();
  final String _collection = 'users';
  final String _docId = 'user123';
  Map<String, dynamic>? _userData;

  @override
  void initState() {
    super.initState();
    _fetchUserData();
  }

  Future<void> _fetchUserData() async {
    Map<String, dynamic>? data = await _firestore.getData('$_collection/$_docId');
    setState(() {
      _userData = data;
    });
  }

  Future<void> _createOrUpdateUser() async {
    Map<String, dynamic> data = {
      'name': 'John Doe',
      'age': 30,
      'email': 'john.doe@example.com',
    };
    bool success = await _firestore.writeData('$_collection/$_docId', data);
    if (success) {
      print('User data written successfully.');
      _fetchUserData();
    } else {
      print('Failed to write user data.');
    }
  }

  Future<void> _deleteUser() async {
    bool success = await _firestore.deleteData('$_collection/$_docId');
    if (success) {
      print('User data deleted successfully.');
      setState(() {
        _userData = null;
      });
    } else {
      print('Failed to delete user data.');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Firestore Example'),
        ),
        body: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              _userData != null
                  ? Text('User Data: $_userData')
                  : Text('No user data available.'),
              ElevatedButton(
                onPressed: _createOrUpdateUser,
                child: Text('Create/Update User'),
              ),
              ElevatedButton(
                onPressed: _deleteUser,
                child: Text('Delete User'),
              ),
            ],
          ),
        ));
  }
}
```

### Example 3: Realtime Database Operations

Refer to the [Usage](#usage) section for detailed examples on using the RTDB class.

### Example 4: Firebase Storage Operations

Refer to the [Usage](#usage) section for detailed examples on using the Firebase Storage class.

## Documentation

Comprehensive documentation is provided within the package to assist you in understanding and utilizing its full potential. Each class and method is thoroughly documented with explanations of parameters, return values, and usage examples.

### Classes and Their Responsibilities

- **Auth**: Handles Firebase Authentication tasks including email/password and Google sign-in, registration, sign-out, and password reset.
- **SimpleFirestore**: Simplifies Firestore operations such as reading, writing, updating, deleting documents, and observing real-time changes.
- **RTDB**: Manages Realtime Database operations with rate limiting, CRUD functionalities, and real-time data observation.
- **BucketStorage**: Facilitates Firebase Storage operations including uploading single/multiple images, retrieving download URLs, and file management.

### Getting Started

1. **Initialize Firebase**: Ensure Firebase is initialized in your `main.dart`.
2. **Configure Firebase**: Add the necessary configuration files (`google-services.json`, `GoogleService-Info.plist`).
3. **Use Helper Classes**: Instantiate and utilize the helper classes (`Auth`, `SimpleFirestore`, `RTDB`, `BucketStorage`) as demonstrated in the usage examples.

For detailed API references, refer to the inline documentation within each class file.

## Contributing

Contributions are welcome! If you find a bug or have a feature request, please open an issue or submit a pull request.

### Steps to Contribute

1. Fork the repository.
2. Create a new branch:

```bash
git checkout -b feature/YourFeatureName
```

3. Make your changes and commit them:

```bash
git commit -m "Add your message here"
```

4. Push to the branch:

```bash
git push origin feature/YourFeatureName
```

5. Open a pull request.

### Guidelines

- Follow the existing code style and structure.
- Ensure that all new features are well-documented.
- Write unit tests for new functionalities.

## License

This project is licensed under the MIT License.

---

## Contact

For any questions or support, please open an issue on the GitHub repository or contact youremail@example.com.

---

## Acknowledgements

- [Firebase](https://firebase.google.com/)
- [Flutter](https://flutter.dev/)
- [Dart](https://dart.dev/)
- [Image Picker](https://pub.dev/packages/image_picker)

---

## Example Project

Check out the example project demonstrating how to integrate Firebase Simplifier into a Flutter application.

---

**Note**: Replace placeholders like `yourusername`, `firebase_simplifier`, `youremail@example.com`, and repository links with your actual information.
""";
```
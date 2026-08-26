# Firebase setup for Mauricio's Arcade

The site code is static, but chat/admin data must be protected by Firebase.

## 1. Create/configure Firebase

Create a Firebase project and enable:

- Authentication → Email/Password
- Realtime Database
- Storage

The HTML pages currently contain placeholder Firebase web config values. Replace those placeholders with the Web App config from your Firebase project. Do **not** put service-account private keys in this repository.

## 2. Deploy the rules

Install Firebase CLI and log in, then from this repository run:

```bash
firebase use YOUR_PROJECT_ID
firebase deploy --only database,storage
```

The repository's `firebase.json` points the CLI at `database.rules.json` and `storage.rules`.

## 3. Create the first admin

After your account signs in once, find its Firebase Authentication UID. In Realtime Database, create:

```text
admins
  YOUR_FIREBASE_UID: true
```

This is the bootstrap step. Do it manually in the Firebase console. Once that admin exists, the security rules prevent ordinary users from assigning themselves admin access.

## 4. Important

The `/apple/` path and any client-side password are not security boundaries. Real authorization comes from Firebase Authentication plus the `admins/{uid}` database rule.

Do not make the database public just to get the chat working. If a rule rejects a request, fix the rule/auth flow instead.

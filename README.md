# Quest Question Quench (q3) — Android build project

This is your React prototype, already bundled into a standalone web app and
wrapped in a native Android project (via Capacitor). Everything is done
**except the final compile step**, which needs Android Studio / the Android
SDK — something that isn't available in the environment that prepared this
package, so it has to be run on your own computer.

## What's inside
- `src/App.jsx` — your app's source code (edit this if you want to keep changing it)
- `dist/` — the bundled, standalone web build (HTML + JS) that runs in any browser
- `android/` — a full native Android Studio project pointing at that build
- `package.json` — the build scripts (`npm run build`, `npm run sync`)

## What you need on your computer first
1. **Node.js** (v18+) — https://nodejs.org
2. **Android Studio** — https://developer.android.com/studio (this installs the Android SDK for you)

## Steps to get an installable file

1. Unzip this project anywhere on your computer.
2. **Paste your Firebase config.** Open `src/App.jsx`, find the block near
   the top marked `PASTE YOUR FIREBASE CONFIG HERE`, and replace the
   placeholder values with the real ones from your Firebase Console
   (Project settings → Your apps → the `firebaseConfig` snippet).
3. Open a terminal in that folder and run:
   ```
   npm install
   ```
4. Rebuild the bundle and sync it into the Android project:
   ```
   npm run sync
   ```
   (Do this every time you edit `src/App.jsx` — including after step 2.)
5. Open the `android` folder as a project in Android Studio
   (File → Open → select the `android` folder).
5. Let Android Studio finish indexing and downloading Gradle dependencies
   (first time only — needs internet access, can take several minutes).
6. To just try it on your own phone/emulator: click the green ▶ Run button.
7. To get a file for the Play Store:
   - Go to **Build → Generate Signed Bundle / APK**
   - Choose **Android App Bundle** (this is what Play Store wants — an `.aab` file)
   - Create a new signing key if you don't have one yet (keep this key file
     and its password safe forever — you need the exact same one for every
     future update of this app)
   - Build it. The signed `.aab` will appear under `android/app/release/`

That `.aab` file is what you upload to Google Play Console.

## Firestore security rules

While testing, "Test mode" rules (open to anyone) are fine. Before any real
student uses this app, go to Firebase Console → Firestore Database → Rules
and use something like this instead — it still has no per-user auth (this
prototype doesn't have real login yet), but at least scopes access to only
this app's one document:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /q3/{document=**} {
      allow read, write: if true; // TODO: replace with real auth checks
    }
  }
}
```

A production version should add Firebase Authentication (e.g. real phone
OTP via Firebase Auth) and rewrite these rules to check `request.auth`,
so random strangers can't read or overwrite your students' data just by
knowing your Firestore project exists.

## Important — read before publishing

This is still a **prototype**, not production software. Cross-device sync
now works for real (via Firestore) — but:
- OTP login still accepts the demo code `123456` for anyone
- Payments are simulated — tapping "I've completed the payment" always marks it paid, with no real verification
- Teacher passcode (`partharoy69`) and phone number are hard-coded in the
  app's source code, not stored securely on a server
- Firestore rules are wide open (see above) until you lock them down

Publishing this as-is means real students would be entering real phone
numbers and making real payment decisions against a system that isn't
actually verifying either. At minimum, before real users install this,
replace the OTP and payment flows with real providers (e.g. Firebase Auth
Phone or MSG91/Twilio for OTP, Razorpay/Cashfree for payments) and lock
down the Firestore rules above.

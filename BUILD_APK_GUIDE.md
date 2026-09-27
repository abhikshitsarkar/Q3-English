# How to get your downloadable APK (no coding needed)

Your project is a Capacitor Android app. It needs to be compiled once — the easiest
free way is to let GitHub build it for you in the cloud.

## Step 1: Put this folder on GitHub
1. Go to https://github.com and create a free account (if you don't have one).
2. Click **New repository** → name it e.g. `questionquench` → keep it **Public** → Create.
3. On the repo page click **"uploading an existing file"** and drag in everything from
   this folder (or use "Add file → Upload files"). Make sure the `.github/workflows`
   folder is included too.
4. Commit the files.

## Step 2: Let GitHub build the APK
- The build starts automatically on every push to `main`.
- Or go to the **Actions** tab → "Build APK" → **Run workflow** to start it manually.
- Wait ~5–10 minutes (green checkmark = success).

## Step 3: Share the APK with anyone
- Go to the **Releases** section on the right side of your repo → **v1.0**.
- The file `app-debug.apk` in **Assets** has a public link — anyone can download and
  install it on Android (they may need to allow "Install from unknown sources").

Note: a *debug* APK is fine for sharing with friends/testing. To publish on the
Google Play Store later, you'd need a *signed release* build, which can be added
to the same workflow when you're ready.

## Alternative: build on your own computer
1. Install Android Studio (https://developer.android.com/studio).
2. Open the `android` folder in Android Studio.
3. Build → Build Bundle(s)/APK(s) → Build APK(s).
4. The APK appears at `android/app/build/outputs/apk/debug/app-debug.apk`.

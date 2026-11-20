# IMBHU_APP (modernized)

WebView wrapper for https://www.imbhu.com/ with a 4-second logo splash.

Stack: AGP 8.13.0, Gradle 8.13, JDK 17, AndroidX AppCompat 1.7.1,
compileSdk/targetSdk 36 (Google Play's requirement since 2026-08-31), minSdk 24.

## 1. Make a signing key (once; back it up, never share it)
    keytool -genkeypair -v -keystore imbhu.jks -alias imbhu \
      -keyalg RSA -keysize 2048 -validity 10000

Create `keystore.properties` in the project root:
    storeFile=imbhu.jks
    storePassword=YOUR_STORE_PASSWORD
    keyAlias=imbhu
    keyPassword=YOUR_KEY_PASSWORD

## 2. Build
Open the folder in Android Studio (it creates the Gradle wrapper), or with Gradle 8.13 installed:
    gradle wrapper --gradle-version 8.13
    ./gradlew assembleRelease bundleRelease

Outputs:
- app/build/outputs/apk/release/app-release.apk    -> rename to imbhu.apk
- app/build/outputs/bundle/release/app-release.aab -> rename to imbhu.aab (upload to Play Console)

## 3. Or build on GitHub (no local setup)
Push this folder to a GitHub repo and run the "Build IMBHU" workflow. Add secrets
KEYSTORE_BASE64 (`base64 -w0 imbhu.jks`), KEYSTORE_PASSWORD, KEY_ALIAS, KEY_PASSWORD
for signed files. The run uploads imbhu.apk and imbhu.aab as an artifact.

## Before publishing on Google Play
- applicationId is still `com.example.imbhu_app.imbhu_app`. Play rejects `com.example.*`.
  Change `applicationId` in app/build.gradle (e.g. `com.imbhu.app`). Keep it as-is only if
  this exact id is already published and you are updating that listing.
- Upload with Play App Signing; the .aab must be signed with your upload key.

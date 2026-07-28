# Zotero Android

## Prerequisites & Setup

### 1. Requirements
- **JDK**: Java 17 or higher
- **Android SDK**: API Level 35 (Build Tools 35.0.0)

### 2. Environment Configuration
Ensure `local.properties` exists in the project root pointing to your Android SDK location:
```properties
sdk.dir=/home/dhp/android-sdk
```

---

## Building the App

### Build Debug APK via Command Line
To compile the debug APK (`app-dev-debug.apk`), run:

```bash
JAVA_HOME=/home/dhp/jdk17 ./gradlew :app:assembleDevDebug
```

The generated APK will be located at:
```text
app/build/outputs/apk/dev/debug/app-dev-debug.apk
```

### Build via Android Studio
1. Open the project folder in Android Studio.
2. Select **`devDebug`** in the **Build Variants** panel.
3. Select **Build** > **Build Bundle(s) / APK(s)** > **Build APK(s)**.
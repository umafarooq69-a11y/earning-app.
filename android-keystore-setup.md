
# Android Keystore & Release Signing

To publish or distribute a **signed release** APK/AAB, you need a keystore.

## Create a Keystore (locally)

```bash
keytool -genkey -v -keystore ~/my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
```
Remember the keystore path, alias, and passwords.

## Configure Flutter Android Signing

Create/modify `app/android/key.properties`:

```
storePassword=YOUR_STORE_PASSWORD
keyPassword=YOUR_KEY_PASSWORD
keyAlias=my-key-alias
storeFile=/absolute/path/to/my-release-key.jks
```

Edit `app/android/app/build.gradle` and include:

```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file("key.properties")
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    ...
    signingConfigs {
        release {
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            shrinkResources false
        }
    }
}
```

Then build:

```bash
cd app
flutter build apk --release
```

Signed APK path:
```
app/build/app/outputs/flutter-apk/app-release.apk
```

## CI Signing (optional)

- Add the keystore file as an **encrypted secret** in GitHub (e.g. Base64 encode it).
- Add secrets for passwords.
- Update the CI workflow to decode and place the keystore, then run `flutter build apk --release` with signing.

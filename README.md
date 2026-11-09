# Itihasa – Android app

Indian history, answered by voice, working fully offline: 1,053 quick facts and 169 topics,
with a built-in speech-recognition model so the microphone works without internet.

## How it is built

The app is the same web app as `itihasa-v2.html` (in `www/index.html`), wrapped with
[Capacitor](https://capacitorjs.com) so it runs as a normal Android app.

Inside the app:

| Feature | How it works on Android |
|---|---|
| Voice questions, online | The phone's own speech recogniser (`@capacitor-community/speech-recognition`) |
| Voice questions, offline | Built-in Whisper model in `www/itihasa-voice/`, running on the phone |
| Reading answers aloud | The phone's text-to-speech voices (`@capacitor-community/text-to-speech`) |
| Back button | Closes the open panel, then returns home, then exits (`@capacitor/app`) |
| Pictures | Downloaded once on Wi‑Fi and kept on the phone |

The link between the web app and these native features is `android_bridge.js` (inside `www/index.html`).

## Getting the APK

Every push to `main` builds the app on GitHub (see `.github/workflows/android.yml`).
The installable file appears under **Releases** as `Itihasa-1.0.<n>.apk`.

To install on a phone: open the release page on the phone, download the APK, and allow
"install unknown apps" for your browser when Android asks.

## Building on your own computer (optional)

Requirements: Node.js 22, JDK 21, Android Studio (SDK 36).

```
npm ci
npx cap sync android
cd android
./gradlew assembleRelease
```

The APK is written to `android/app/build/outputs/apk/release/`.

## Before publishing on Google Play

* **Signing key.** `android/app/itihasa-sideload.keystore` is a *test* key so that new builds install over
  old ones. For the Play Store, create your own private upload key, keep it secret (GitHub secrets,
  not the repository), and point `signingConfigs` at it.
* **App ID.** `com.itihasa.app` becomes permanent once published. Change it first if you want
  another (in `capacitor.config.json`, `android/app/build.gradle` and `strings.xml`).
* **Play format.** Build an App Bundle with `./gradlew bundleRelease`.
* **Policies.** You will need a privacy policy and the Data safety form (the app collects no data,
  and voice is processed on the phone when offline), plus the content rating and Families
  forms if children are a target audience.

## Licences of included components

* Whisper speech models (OpenAI) – MIT; ONNX conversions by Xenova
* Transformers.js – Apache-2.0; ONNX Runtime Web – MIT
* Capacitor and its plugins – MIT
* Fonts: Cormorant Garamond, Mukta – SIL Open Font License
* Pictures: Wikimedia Commons, each under its own free licence; the app shows the credits

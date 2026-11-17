# DashDrive

A dark, tablet-first dashcam app for any Android tablet (Android 5.0+). Records like Nexar:
loop recording, impact-locked clips, parking mode, local storage and cloud backup.

## Get the APK (pick one)

**A. No installs, via GitHub (about 5 minutes)**
1. Create a new empty GitHub repo and push this folder to it.
2. Open the repo's **Actions** tab, then **Build APK**, then the latest run.
3. Download the **DashDrive-debug-apk** artifact, unzip it, and copy `app-debug.apk` to the tablet.
4. On the tablet, open the file and allow "install unknown apps" when asked.

**B. Android Studio**
1. Open this folder in Android Studio (Koala or newer) and let Gradle sync.
2. Build > Build APK(s). Or plug the tablet in (USB debugging on) and press Run.

**C. Command line** (needs JDK 17 and the Android SDK): `./gradlew assembleDebug`
The APK lands in `app/build/outputs/apk/debug/app-debug.apk`.

## How it works

- **Driving detection** fuses three sources each second:
  1. GPS speed, when the tablet has GPS (authoritative).
  2. The tablet's motion sensor (road vibration vs. a calibrated idle baseline). Preferred on GPS-less tablets.
  3. Camera motion (frame differencing) as a tie-breaker, or the only source if there's no motion sensor.
  Start/stop delays prevent flapping at red lights.
- **Recording** uses CameraX, in segments (1-10 min). Oldest unlocked clips are deleted when the storage limit is reached.
- **Incidents**: a jolt over your threshold locks the current and previous clip. In Auto mode a bump while parked records a locked 1-minute clip.
- **Storage**: app storage by default (no permissions needed), optional SD card, optional mirror folder (USB/SD/any storage app).
- **Cloud**: (a) any cloud app folder via the system picker (Google Drive, Dropbox, OneDrive...), (b) WebDAV (Nextcloud, Synology, ownCloud). Wi-Fi-only, charging-only, locked-clips-only and delete-after-upload options.

## Known limits

- Android 11+ only lets a camera start while the app is on screen, so open the app (it arms automatically) and leave it mounted.
- No timestamp / speed burned into the video yet.
- WebDAV password is stored unencrypted in app preferences.
- Launcher icon is a simple placeholder.
- This project was written without being compiled (no Android SDK in the authoring environment). If the first build reports a small error, it should be a one-line fix.

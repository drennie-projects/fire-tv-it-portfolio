# Setup steps and commands I used

These notes document my own project. Replace `<TV_IP>` with the Fire TV's current local IP address when following them. I removed my local IP addresses from this public write-up.

## 1. Connect with ADB

On the TV, I enabled Developer Options and ADB Debugging. On Windows, I downloaded Google's Android SDK Platform-Tools and ran:

```powershell
.\adb.exe version
.\adb.exe connect <TV_IP>:5555
.\adb.exe devices
```

ADB lets me send Android commands from my PC. The `devices` command lists devices with an ADB connection. Because I also had BlueStacks installed, I used `-s <TV_IP>:5555` to make sure commands targeted the actual Fire TV.

## 2. Check the device

```powershell
.\adb.exe -s <TV_IP>:5555 shell getprop ro.product.model
.\adb.exe -s <TV_IP>:5555 shell getprop ro.build.version.sdk
.\adb.exe -s <TV_IP>:5555 shell pm list packages -3
```

The first two commands returned `AFTHY7ABBA` and API level `30` (Android 11). The third command listed third-party packages and later helped me identify the launcher as `dev.tvhome`.

## 3. Set up Java and Android Studio

The project needed Java 21. Initially, `java -version` failed even though Java was installed. I found the JDK in Eclipse Adoptium and set my environment for the current PowerShell session:

```powershell
$env:JAVA_HOME = 'C:\Program Files\Eclipse Adoptium\jdk-21.0.13.9-hotspot'
$env:Path = "$env:JAVA_HOME\bin;$env:Path"
java -version
```

The result was Eclipse Temurin Java 21.0.13. I also saved `JAVA_HOME` as a user environment variable; I didn't separately verify a permanent PATH change.

I installed Android Studio, Android SDK Platform 34, and build tools. I opened the inner project folder containing `settings.gradle` and selected Java 21 when Android Studio warned that Gradle 8.9 didn't support Java 25.

## 4. Customize the launcher

In `app/src/main/assets/home/config.js`, I pointed the background to my own image:

```javascript
background: ["chrome.jpg"],
backgroundMinutes: 30,
```

I saved the image in the same `home` assets folder. I also changed `style.css` to experiment with rounded app cards, translucent panels, and a blue-white focus outline. I backed up the working CSS before doing more design changes.

## 5. Build and install APK

In Android Studio, I used **Build APK(s)**. The file was created at `app/build/outputs/apk/debug/app-debug.apk`. From the APK folder, I installed it using:

```powershell
& "C:\path\to\platform-tools\adb.exe" -s <TV_IP>:5555 install -r ".\app-debug.apk"
```

The `-r` flag requests replacement of the existing app without removing its data when the signing key is compatible. ADB reported `Success`.

To open it manually:

```powershell
& "C:\path\to\platform-tools\adb.exe" -s <TV_IP>:5555 shell monkey -p dev.tvhome -c android.intent.category.LAUNCHER 1
```

I checked the current Android activity with:

```powershell
& "C:\path\to\platform-tools\adb.exe" -s <TV_IP>:5555 shell dumpsys activity activities
```

The output showed `dev.tvhome/.MainActivity` as resumed and visible.

## 6. Screenshot troubleshooting

Windows PowerShell's `>` redirection produced an unreadable screenshot file. I learned to save the image on the device and transfer it with ADB:

```powershell
& "C:\path\to\platform-tools\adb.exe" -s <TV_IP>:5555 shell screencap -p /sdcard/firetv-glass-test.png
& "C:\path\to\platform-tools\adb.exe" -s <TV_IP>:5555 pull /sdcard/firetv-glass-test.png "$env:USERPROFILE\Desktop\firetv-glass-test.png"
```

I didn't run any debloating commands or disable Amazon's original launcher.

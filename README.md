# Fire TV Custom Launcher Project

I wanted to see if I could customize my Fire TV instead of using Amazon's regular home screen. I found an open-source launcher on GitHub, got it running on my TV, and changed its background and some of its styling. I also used the project to practice ADB, PowerShell, Android Studio, and troubleshooting.

I didn't build the original launcher from scratch. My part was setting it up, changing the configuration and CSS, building the APK, installing it, and figuring out problems along the way.

## What I used

- **PC:** Windows 11, PowerShell, Android Studio, Eclipse Temurin Java 21
- **TV:** Fire OS 8.1.0.5, Android 11 (API 30), model code `AFTHY7ABBA`
- **Tools:** ADB 1.0.41, Android SDK, Gradle 8.9
- **Original launcher:** [Fire TV Web Launcher by pickdani](https://github.com/pickdani/fire-tv-web-launcher)
- **My changes:** custom `chrome.jpg` wallpaper, launcher configuration, glass-style CSS for cards, focus outline, clock, and weather

## What I did

1. Enabled Developer Options and ADB debugging on my Fire TV.
2. Connected it to my PC over my local network, then checked the model and Android version.
3. Downloaded the launcher from GitHub and looked through `config.js` and `style.css` to learn what they control.
4. Installed Java 21 and the Android SDK. Fixed Java PATH and a Gradle/JVM compatibility issue.
5. Added my own wallpaper and made some CSS changes to create a glass-style interface.
6. Built the Android APK in Android Studio and installed it using ADB.
7. Found the launcher package name (`dev.tvhome`) and opened it directly through ADB.
8. Tested the result on my TV and confirmed the launcher was active with Android diagnostics.

## Troubleshooting

This wasn't a straight install. At first, ADB only showed my BlueStacks emulator, and the Fire TV connection gave me Windows socket error 10013. I checked port 5555 using PowerShell and got the ADB connection working. I also found that a successful TCP test doesn't automatically mean ADB is connected.

Java was installed but PowerShell couldn't find `java` until I fixed the environment for that session. Android Studio initially tried Java 25 with Gradle 8.9, so I switched to Java 21.

After the APK installed, I couldn't find it in the Fire TV app list. I used `pm list packages -3` to identify `dev.tvhome`, then opened it with ADB. When ADB disconnected during an update, I reconnected before installing again.

I also learned that piping a screenshot into a file using Windows PowerShell can corrupt the PNG. A better method is taking the screenshot on the TV and transferring it with `adb pull`.

The commands and fixes are in my [troubleshooting log](docs/troubleshooting.md).

## Result

**The custom launcher works.** I built and installed the modified APK, opened it on the Fire TV, and saw my wallpaper, app row, clock, and weather. `dumpsys` confirmed that `dev.tvhome/.MainActivity` was visible and resumed.

I'm leaving this as a working prototype for now. The glass design isn't quite where I want it, and the Home button still returns to Amazon's original screen. I **did not** run the debloat script, disable Amazon system apps, or measure performance gains.

## What I learned

The biggest thing I learned was how to go from a GitHub project to an Android app running on an actual device. I got more comfortable with PowerShell, reading errors, checking network connections, selecting the right ADB device, and testing whether an app works after installing it.

## Project documentation

- [Setup and commands](docs/setup-guide.md)
- [Problems and fixes](docs/troubleshooting.md)
- [Tests and limitations](docs/testing.md)
- [Screenshots](screenshots/README.md)
- [Scripts](scripts/README.md)

## Credit

This project is based on [pickdani/fire-tv-web-launcher](https://github.com/pickdani/fire-tv-web-launcher), which is MIT-licensed. I customized and deployed it for a learning project. This repository contains my documentation, not a claim of ownership of the original code. My personal wallpaper and compiled APK are not included.

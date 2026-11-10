# Testing and results

| Test | Result |
| --- | --- |
| ADB installed on Windows | Passed — version 1.0.41 |
| Fire TV port 5555 reachable | Passed — `TcpTestSucceeded : True` |
| Fire TV identified | Passed — `AFTHY7ABBA`, Android API 30 |
| Java JDK available | Passed — Temurin 21.0.13 |
| Gradle project sync | Passed |
| APK compilation | Passed |
| Install and update through ADB | Passed — `Success` |
| Launch the app through ADB | Passed — `dev.tvhome` opened |
| Foreground activity check | Passed — `dev.tvhome/.MainActivity` resumed and visible |
| Custom wallpaper | Passed — seen on TV |
| Glass CSS changes | Partially passed — visible changes, design still unfinished |
| Home button replacement | Not working — returns to Amazon |
| Debloating | Not attempted |
| Performance comparison | Not measured |

## What's next

This project is a working prototype, not a fully replaced Fire TV system. Before calling a future launcher finished, I want to test all app shortcuts, Home button behavior, reboot behavior, navigation with the remote, and performance. I won't claim speed improvements without measuring them.

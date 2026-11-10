# Problems I ran into

These are the issues I encountered while getting the launcher working. I kept the original error details so I can show how I approached each problem.

| Problem | How I checked or fixed it | Result |
| --- | --- | --- |
| I typed the placeholder `YOUR_TV_IP` into an ADB command | Replaced it with the TV's actual IP and port `5555` | Syntax corrected |
| ADB connection gave Windows socket error `10013` | Used `Test-NetConnection -ComputerName <TV_IP> -Port 5555`; TCP succeeded. Closed BlueStacks, restarted ADB, and retried | ADB eventually connected; exact cause of `10013` wasn't confirmed |
| `adb devices` only showed `emulator-5554` | Recognized this was BlueStacks and used `-s <TV_IP>:5555` for commands | Sent commands to the Fire TV |
| `java` wasn't recognized | Located JDK 21 and updated `JAVA_HOME` and the current PowerShell PATH | `java -version` returned 21.0.13 |
| Android Studio opened the wrong project level | Opened the inner folder that contained `settings.gradle` | Project recognized |
| Gradle 8.9 wouldn't use Java 25 | Selected **Use JVM 21** | Sync completed |
| App installed but didn't show in the TV apps list | Used `pm list packages -3` to find `dev.tvhome`, then launched with ADB Monkey | Launcher opened |
| Pressing Home returned to Amazon | Confirmed custom launcher was not set up to intercept Home | Left original launcher unchanged |
| ADB said `device not found` during an update | Reconnected to the TV with `adb connect` | Updated install succeeded |
| PNG screenshot wouldn't open | Captured on TV with `screencap -p`, then transferred using `adb pull` | Documented safer transfer method |

## Main lesson

I learned that a successful TCP port test doesn't necessarily mean ADB is connected or authorized. Troubleshooting worked better when I checked the network connection, the ADB device list, and the actual Android app state separately.

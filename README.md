# CellBroadcast Trigger

[简体中文](README_zh.md) | [English](README.md)

This is a testing tool for simulating and triggering Wireless Emergency Alerts (WEA) and Earthquake & Tsunami Warning Systems (ETWS) on Android devices.

This application uses root privileges to call hidden APIs at the Android system level, directly sending fake cell broadcast messages to the system broadcast receiver. This is extremely useful for testing the phone's alarm reception capabilities, UI display, and for developing and debugging related applications.

## Real World Usage
It found a real [android design bugs](https://issuetracker.google.com/issues/472754120).


### Compilation Environment

* **JDK**: Java 11 or later (project configured with `sourceCompatibility = JavaVersion.VERSION_11`).

* **Android SDK**:

* `compileSdk`: 36

* `minSdk`: 21 (Android 5.0)

* `targetSdk`: 36

* **Gradle**: The project includes a Gradle Wrapper; it is recommended to use the `./gradlew` command directly.

### Compilation Steps

1. Clone the project to your local machine.

2. Open the project root directory using Android Studio.

3. Sync the Gradle project.

4. Connect your device or launch the emulator.

5. Run the following command to install the debug package:

```bash

./gradlew installDebug

```
Or build the release package:

```bash

./gradlew assembleRelease

```

## Usage Instructions

### Prerequisites

* Your Android device must **have root access** (Magisk, KernelSU, APatch, etc.).

* Grant the application root access (requested the first time you tap "TRIGGER ALERT").

### App Usage

1. Open the application.

2. Enter the alert message.

3. Select the alert level (e.g., "Presidential" or "ETWS: Earthquake").

4. (Optional) Set the delay time.

5. Click the **"TRIGGER ALERT (ROOT)"** button.

6. Observe whether the phone displays a system-level alert pop-up.

### ADB Command Line Usage

You can also directly call the underlying logic of this application through ADB, suitable for automated testing.

**Basic Format:**

```bash
adb shell "CLASSPATH=\$(pm path top.stevezmt.cellbroadcast.trigger | cut -d: -f2) app_process /system/bin top.stevezmt.cellbroadcast.trigger.RootMain '<Base64 encoded message content>' <type code> <delay in milliseconds> <whether ETWS>"

```

**Parameter Description:**

* `Base64 encoded message content`: Converts the string to a UTF-8 Base64 string (to prevent garbled characters).

* **Type Code:**

**CMAS (isEtws=false)**: 0=Presidential, 1=Extreme, 2=Severe, 3=Amber, 4=Test

**ETWS (isEtws=true)**: 0=Earthquake, 1=Tsunami, 2=Quake+Tsunami, 3=Test, 4=Other

* **Delay in milliseconds:** Integer, 0 indicates immediate transmission.

* **ETWS Status:** `true` or `false`.

**Example: Sending an ETWS alert with the Chinese characters "地震预警" (earthquake warning)**

(The Base64 encoding of "地震预警" is `5Zyw6ZyH6aKE6K2m`)

```bash
adb shell "CLASSPATH=\$(pm path top.stevezmt.cellbroadcast.trigger | cut -d: -f2) app_process /system/bin top.stevezmt.cellbroadcast.trigger.RootMain '5Zyw6ZyH6aKE6K2m' 0 0 true"

```

## Disclaimer

**Please use this tool responsibly.**

* This application is for **development, testing, and research** purposes only.

* **It is strictly forbidden to use this application to trigger alarms in public places** or in situations that may cause misunderstanding or panic among others.

* The simulated alarm sound and vibration are completely identical to a real alarm and may cause disturbance or fright to those around you.

* The developer is not responsible for any consequences resulting from the misuse of this tool.

## License

[GPL v3](LICENSE)
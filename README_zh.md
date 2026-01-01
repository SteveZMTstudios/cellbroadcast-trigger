# CellBroadcast Trigger

[简体中文](README_zh.md) | [English](README.md)

这是一个用于在 Android 设备上模拟和触发无线紧急警报 (Wireless Emergency Alerts, WEA) 和地震海啸预警系统 (ETWS) 的测试工具。

本应用通过 Root 权限调用 Android 系统底层的隐藏 API，直接向系统广播接收器发送伪造的小区广播消息。这对于测试手机的警报接收功能、UI 展示以及相关应用的开发调试非常有用。
 
### 编译环境

*   **JDK**: Java 11 或更高版本 (项目配置为 `sourceCompatibility = JavaVersion.VERSION_11`)。
*   **Android SDK**:
    *   `compileSdk`: 36
    *   `minSdk`: 21 (Android 5.0)
    *   `targetSdk`: 36
*   **Gradle**: 项目包含 Gradle Wrapper，建议直接使用 `./gradlew` 命令。

### 编译步骤

1.  克隆项目到本地。
2.  使用 Android Studio 打开项目根目录。
3.  同步 Gradle 项目。
4.  连接设备或启动模拟器。
5.  运行以下命令安装调试包：

```bash
./gradlew installDebug
```

或者构建发布包：

```bash
./gradlew assembleRelease
```

## 使用说明

### 前置条件
*   您的 Android 设备必须**已获取 Root 权限** (Magisk, KernelSU, APatch 等)。
*   授予应用 Root 权限（首次点击 "TRIGGER ALERT" 时会请求）。

### App 端使用
1.  打开应用。
2.  输入警报消息内容。
3.  选择警报等级（如 "Presidential" 或 "ETWS: Earthquake"）。
4.  (可选) 设置延迟时间。
5.  点击 **"TRIGGER ALERT (ROOT)"** 按钮。
6.  观察手机是否弹出系统级警报弹窗。

### ADB 命令行使用
您也可以通过 ADB 直接调用本应用的底层逻辑，适合自动化测试。

**基本格式：**
```bash
adb shell "CLASSPATH=\$(pm path top.stevezmt.cellbroadcast.trigger | cut -d: -f2) app_process /system/bin top.stevezmt.cellbroadcast.trigger.RootMain '<Base64编码的消息内容>' <类型代码> <延迟毫秒> <是否ETWS>"
```

**参数说明：**
*   `Base64编码的消息内容`: 将字符串转为 UTF-8 的 Base64 字符串（防止乱码）。
*   `类型代码`:
    *   **CMAS (isEtws=false)**: 0=Presidential, 1=Extreme, 2=Severe, 3=Amber, 4=Test
    *   **ETWS (isEtws=true)**: 0=Earthquake, 1=Tsunami, 2=Quake+Tsunami, 3=Test, 4=Other
*   `延迟毫秒`: 整数，0 表示立即发送。
*   `是否ETWS`: `true` 或 `false`。

**示例：发送一条中文“地震预警”的 ETWS 警报**
("地震预警" 的 Base64 为 `5Zyw6ZyH6aKE6K2m`)

```bash
adb shell "CLASSPATH=\$(pm path top.stevezmt.cellbroadcast.trigger | cut -d: -f2) app_process /system/bin top.stevezmt.cellbroadcast.trigger.RootMain '5Zyw6ZyH6aKE6K2m' 0 0 true"
```

## 免责声明

**请务必负责任地使用此工具。**

*   本应用仅供**开发、测试和研究**目的使用。
*   **严禁在公共场合**或在可能引起他人误解、恐慌的情况下使用此应用触发警报。
*   模拟的警报声音和震动与真实警报完全一致，可能会对周围人群造成干扰或惊吓。
*   开发者不对因滥用此工具造成的任何后果负责。

## 📄 License

[GPL v3](LICENSE)

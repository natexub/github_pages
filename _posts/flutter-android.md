---
title: Flutter和Android
date: 2019-10-21 22:25:56
updated: 2019-10-21 22:25:56
categories:
- flutter
tags:
- Flutter
- Android
- Gradle
---


## 参考
1. [给 Android 开发者的 Flutter 指南 - Flutter 中文文档 - Flutter 中文开发者网站 - Flutter](https://flutter.cn/docs/get-started/flutter-for/android-devs)

## 控件对应

Android开发常用控件在Flutter中的对应。

| Android 控件                         |   Flutter widget |
|:-----------------------------------|-----------------:|
| View                               |           Widget |
| LinearLayout                       |      Column, Row |
| RelativeLayout                     | Column+Row+Stack |
| ScrollView, RecyclerView, ListView |         ListView |
| TextView                           |             Text |
| EditText                           |        TextFeild |


## 问题记录

### 日志

#### 编译项目时，系统日志和Gradle结果混合在一起，如何把他们分开？

##### 问题表现

Android Studio Dolphin | 2021.3.1 Patch 1
Gradle 7.4
![[flutter-log.png]]

##### 解决方法

暂无

### Flutter doctor

#### 找不到SDK

##### 问题表现

```cmd
[!] Android toolchain - develop for Android devices
    X Unable to locate Android SDK.
```

##### 问题原因

未把Android SDK放到默认路径。

##### 解决方法

指定Android SDK位置

```cmd
flutter config --android-sdk D:\env\Android\Sdk
```

##### 扩展阅读

[flutter: The Flutter command-line tool | Flutter](https://docs.flutter.dev/reference/flutter-cli#flutter-commands)

```cmd
PS D:\Projects\flutter_demo> flutter help config                            
Configure Flutter settings.

To remove a setting, configure it to an empty string.

The Flutter tool anonymously reports feature usage statistics and basic crash reports to help improve Flutter tools over time. See Google's privacy policy:https://www.google.com/intl/en/policies/privacy/aster and beta channels.

    --[no-]enable-android                       Enable or disable Flutter for Android.This setting will take effect on the master, beta, and stable channels.
    --[no-]enable-ios                           Enable or disable Flutter for iOS. This setting will take effect on the master, beta, and stable channels.
    --[no-]enable-fuchsia                       Enable or disable Flutter for Fuchsia. This setting will take effect on the master channel.
    --[no-]enable-custom-devices                Enable or disable Early support for custom device types. This setting will take effect on the master, beta, and stable channels.
    --clear-features                            Remove all configured features and restore them to the default values.

Run "flutter help" to see global options.

Settings:
  android-sdk: D:\env\Android\Sdk

Analytics reporting is currently enabled.

```

#### HTTP 可访问性检查错误

##### 问题表现

```cmd
 X HTTP host "https://maven.google.com/" is not reachable. Reason: An error occurred while checking the HTTP host: 信号灯超时时间已到

    X HTTP host "https://pub.dev/" is not reachable. Reason: An error occurred while checking the HTTP host: Connection terminated during handshake
    X HTTP host "https://cloud.google.com/" is not reachable. Reason: An error occurred while checking the HTTP host: 信号灯超时时间已到

```

##### 问题原因

You know

##### 解决方法

配置`https_proxy`和`NO_PROXY`环境变量。见[[cli-proxy|命令行使用代理]]


### 编译时错误 FATAL EXCEPTION: ScreenDecorations

#### 问题表现

```cmd
Launching lib\main.dart on sdk gphone64 x86 64 in debug mode...
Running Gradle task 'assembleDebug'...
E/AndroidRuntime(28818): FATAL EXCEPTION: ScreenDecorations
E/AndroidRuntime(28818): Process: com.android.systemui, PID: 28818
E/AndroidRuntime(28818): java.lang.NullPointerException: Parameter specified as non-null is null: method kotlin.jvm.internal.Intrinsics.checkNotNullParameter, parameter bottomLeft
E/AndroidRuntime(28818): 	at com.android.systemui.statusbar.events.PrivacyDotViewController.initialize(Unknown Source:22)
E/AndroidRuntime(28818): 	at com.android.systemui.ScreenDecorations.setupDecorations(ScreenDecorations.java:315)
E/AndroidRuntime(28818): 	at com.android.systemui.ScreenDecorations.startOnScreenDecorationsThread(ScreenDecorations.java:252)
E/AndroidRuntime(28818): 	at com.android.systemui.ScreenDecorations.$r8$lambda$bRx4s-frKyGv-SmpobluoualBbQ(Unknown Source:0)
E/AndroidRuntime(28818): 	at com.android.systemui.ScreenDecorations$$ExternalSyntheticLambda2.run(Unknown Source:2)
E/AndroidRuntime(28818): 	at android.os.Handler.handleCallback(Handler.java:938)
E/AndroidRuntime(28818): 	at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(28818): 	at android.os.Looper.loopOnce(Looper.java:201)
E/AndroidRuntime(28818): 	at android.os.Looper.loop(Looper.java:288)
E/AndroidRuntime(28818): 	at android.os.HandlerThread.run(HandlerThread.java:67)
```


#### 检错过程

Demo都运行不起来，那么不会是代码的问题。
切换AVD到Android 8版本，继续尝试。
```
Launching lib\main.dart on sdk gphone64 x86 64 in debug mode...
Running Gradle task 'assembleDebug'...


```
log变为卡在`Running Gradle task 'assembleDebug'...`不动，然后我意识到`E/AndroidRuntime(28818)`什么的是Android系统的Log而不是编译的报错。

接下来问题就变成了解决卡Gradle。随后我意识到我这台电脑没有手动装Gradle，那么就可能是IDE在下载Gradle过程中出了问题，或者Gradle在构建时卡在了下载依赖上。

手动下载Gradle，并配置环境变量。也没有配置代理，也没有把仓库改成国内镜像，问题就解决了。

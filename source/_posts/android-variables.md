---
title: Android常用的环境变量
date: 2019-10-21 22:23:42
updated: 2019-10-21 22:23:42
categories:
tags:
- Android
---


|       SDK环境变量       |                                                                                                                                                                                        |
|:-------------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `ANDROID_HOME`    | 设置 SDK 安装目录的路径。类似的`ANDROID_SDK_ROOT`也指向该目录，但已弃用。 如果继续使用，Android Studio 和 Android Gradle 插件会检查新旧变量是否一致。                                                                                 |
| `ANDROID_USER_HOME` | 为作为 Android SDK 一部分的工具设置用户首选项目录的路径。 默认为`$HOME/.android/`。一些较旧的工具，例如 Android Studio 4.3 及更低版本，不读取`ANDROID_USER_HOME`。 要覆盖那些旧工具的用户首选项位置，请将`ANDROID_SDK_HOME`设置为您希望在其下创建`.android`目录的父目录。 |

|         AVD环境变量         |                                                                                                                                                |
|:-----------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------|
| `ANDROID_EMULATOR_HOME` | 设置特定于用户的模拟器配置目录的路径。默认设置为 `$ANDROID_USER_HOME`。较旧的工具（如 Android Studio 4.3 及更早版本）不会读取`ANDROID_USER_HOME`，对于这些工具，默认值为`$ANDROID_SDK_HOME/.android` |
|   `ANDROID_AVD_HOME`    | 设置包含所有 AVD 特定文件的目录的路径，这些文件大多包含非常大的磁盘映像。默认位置是 `$ANDROID_EMULATOR_HOME/avd/`。如果默认位置的磁盘空间不足，您可能需要指定新位置。                                           |



## 参考

1. [环境变量  |  Android 开发者  |  Android Developers](https://developer.android.com/studio/command-line/variables)
2. [命令行工具  |  Android 开发者  |  Android Developers](https://developer.android.com/studio/command-line)
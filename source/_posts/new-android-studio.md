---
title: New to Android Studio
date: 2019-10-20 00:22:25
updated: 2019-10-20 00:22:25
categories:
- New to
tags:
- Android
- Gradle
---

备忘记录

首先设置代理

## 下载和安装

Android SDK
Android SDK Platform
Android Virtual Device
需关闭HyperV 
Gradle

### 配置环境变量

以Windows为例，需要向Path追加以下路径：
- `%ANDROID_HOME%\tools`
- `%ANDROID_HOME%\tools\bin`
- `%ANDROID_HOME%\platform-tools`

详见
[[android-variables]]
[命令行工具  |  Android 开发者  |  Android Developers](https://developer.android.com/studio/command-line)


## 虚拟机管理

### AVD路径

默认的Android Virtual Device Manager文件夹保存在`C:\Users\<user_name>\.android\avd`，要将其移动到新的目录需要增加环境变量`ANDROID_USER_HOME`。`ANDROID_USER_HOME`的默认值为``C:\Users\<user_name>\.android\`或`$HOME/.android/`
- 关闭Android Studio
- 配置环境变量ANDROID_USER_HOME为新位置。
- 启动Android Studio
- 如果新位置生成了`avd`目录则说明配置成功，把之前的内容剪切过来。

### 配置Android studio

#### 配置Android Studio中的Gradle

Gradle官方推荐执行构建采用Gradle Wrapper的方式。Wrapper 是一个脚本，帮助开发人员快速启动并运行项目。相关配置保存在`gradle/wrapper/gradle-wrapper.properties`文件中。

![[gradle-wrapper-properties-default.png]]

Android Studio默认采取这种方式。当然也可以手动配置。

![[android-studio-gradle-setting-default.png]]

Wrapper 执行构建时，会首先会根据`gradle-wrapper.properties`中标明的Gradle版本，寻找`GRADLE_USER_HOME`下是否存在，不存在就会依照`distributionUrl`下载，下载到相对于`GRADLE_USER_HOME`的`distributionPath`中
由于网络原因，很多时候Demo跑不起来，卡在Gradle构建这一步时原因就在这。要么手动修改Android Studio指定Gralde，要么修改`distributionUrl`，要么手动下载相关版本到`GRADLE_USER_HOME`，要么配置代理。

## 问题记录

### 把ANDROID_SDK_HOME设置成了SDK根目录

我想移动AVD保存路径，一开始查到相关的环境变量是`ANDROID_SDK_HOME`, 我把`ANDROID_SDK_HOME`设置为`D:\env\Android\Sdk`, 结果点击Android Studio的 Device Manager 中的Create device 按钮时出现 IDE error 报错。

```log
com.android.prefs.AndroidLocationsException: ANDROID_SDK_HOME is set to the root of your SDK: D:\env\Android\Sdk
ANDROID_SDK_HOME was meant to be the parent path of the preference folder expected by the Android tools.
It is now deprecated.

To set a custom preference folder location, use ANDROID_USER_HOME.

It should NOT be set to the same directory as the root of your SDK.
To set a custom SDK location, use ANDROID_HOME.

```

大意为：
环境变量`ANDROID_SDK_HOME`已经标记为deprecated，
`ANDROID_SDK_HOME`应该设置为Android tools路径的父路径。要定义的SDK路径应该用`ANDROID_HOME`。

### Gradle sync failed

#### 问题表现

```
Gradle sync failed: Plugin [id: 'com.android.application', version: '7.3.1', apply: false] was not found in any of the following sources:
- Gradle Core Plugins (plugin is not in 'org.gradle' namespace)
- Plugin Repositories (could not resolve plugin artifact 'com.android.application:com.android.application.gradle.plugin:7.3.1')
Searched in the following repositories:
Gradle Central Plugin Repository
Google
```

#### 问题背景

Material 3 preview demo API 32
Android Studio Dolphin | 2021.3.1 Patch 1

#### 解决过程

检查代理配置。发现Android Studio的代理设置的是socks，端口自然是socks协议10808。但给Gradle配置的http和https默认依照这一端口10808，显示提示框询问Proxy设置，但我一开始没仔细检查。把`gradle.properties`中的端口改为http的10809。点底部Build窗口的Recresh。Android Studio重新弹窗询问了Proxy配置。



### 编译卡在Running Gradle task

见[[flutter-android#编译时错误 FATAL EXCEPTION: ScreenDecorations|另一次错误]]


参考
1. [Android Emulator - AMD Processor & Hyper-V Support](https://android-developers.googleblog.com/2018/07/android-emulator-amd-processor-hyper-v.html)
2. [The Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html)
3. [Gradle构建详解（gradle-wrapper.properties） - AntarcticPenguin - 博客园](https://www.cnblogs.com/jichui/p/7780614.html)
4. [Android 调试桥 (adb)  |  Android 开发者  |  Android Developers](https://developer.android.com/studio/command-line/adb)



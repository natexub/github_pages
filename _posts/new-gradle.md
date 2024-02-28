---
title: New to gradle
date: 2019-10-20 12:54:48
updated: 2019-10-20 12:54:48
categories:
- New to
tags:
- Gradle
---

## 参考
1. [Installing Gradle](https://docs.gradle.org/current/userguide/installation.html#installation)
2. [Build Environment#gradle_environment_variables](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_environment_variables)
3. [Build Environment](https://docs.gradle.org/current/userguide/build_environment.html#build_environment)
4. [配置 Android Studio  |  Android 开发者  |  Android Developers](https://developer.android.com/studio/intro/studio-config#gradle-plugin)



## 下载和安装

### 步骤一：下载

发布有两个版本：
- bin
- with docs 和 source

下载页面：[Gradle | Releases](https://gradle.org/releases/)

### 步骤二：安装

#### Windows

创建一个目标目录如`D:\env\gradle`，把ZIP压缩包中的文件解压到此。

### 步骤三：配置环境变量

#### 1. bin（可执行文件）路径添加到`Path`

以Windows为例。可以直接在`Path`后面追加，也可以线创建一个`GRADLE_HOME`变量，再把`%GRADLE_HOME%\bin`追加到`Path`后面，以便后期升级或者切换Gradle版本时直接修改`GRADLE_HOME`变量的值即可。
我设置`GRADLE_HOME`为``D:\env\gradle\`

#### 2. GRADLE_USER_HOME

指定用户目录，不指定默认为`$USER_HOME/.gradle`。
我改为了`D:\env\gradle\gradle-6.9.3`

#### 3.JAVA_HOME

指定用作client VM的JDK的路径。也可以通过在配置文件中指定`org.gradle.java.home`来覆盖（见下文）。

有时候Gradle版本不兼容当前jdk，可以通过修改这个环境变量来指定合适的jdk。

## 配置文件和代理

针对本身或者单个项目，Gradle有多种配置方式：
- [命令行参数](https://docs.gradle.org/current/userguide/command_line_interface.html#command_line_interface)优先级高于properties和 环境变量。 如 `--build-cache`。
- [System_properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_system_properties) 保存在 `gradle.properties` 文件中。如`systemProp.http.proxyHost=somehost.org`
- [Gradle properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties) 保存在项目根目录的`gradle.properties`文件中，或保存在`GRADLE_USER_HOME`环境变量指定的目录下的配置文件中。如 `org.gradle.caching=true`.
- [环境变量](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_environment_variables) 比如`GRADLE_OPTS`

如果用国内镜像源便不用配置代理。

在上一步我对`GRADLE_USER_HOME`进行了修改，在该目录下新建一个名为 `gradle.properties` 文件。

代理方式依情况三选一：http、https、socks。

http代理
```
systemProp.http.proxyHost=www.somehost.org
systemProp.http.proxyPort=8080
systemProp.http.proxyUser=userid
systemProp.http.proxyPassword=password
systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost
```

https代理
```
systemProp.https.proxyHost=www.somehost.org
systemProp.https.proxyPort=8080
systemProp.https.proxyUser=userid
systemProp.https.proxyPassword=password
# NOTE: this is not a typo.
systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost
```

socks代理
```
systemProp.socksProxyHost=www.somehost.org
systemProp.socksProxyPort=1080
systemProp.java.net.socks.username=userid
systemProp.java.net.socks.password=password
```

还有另外一种代理方法就是编辑项目根目录下的`build.gradle`。详见参考4。

```gradle
plugins {
  id 'com.android.application'
}

android {
    ...

    defaultConfig {
        ...
        systemProp.http.proxyHost=proxy.company.com
        systemProp.http.proxyPort=443
        systemProp.http.proxyUser=userid
        systemProp.http.proxyPassword=password
        systemProp.http.auth.ntlm.domain=domain
    }
    ...
}
```


## 问题记录



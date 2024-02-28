---
title: 禁用chrome自动更新的7种方法(Windows/MacOS)
date: 2022-04-24 16:28:48
updated: 2022-04-24 16:28:48
categories:
tags:
---

Chrome动不动静默更新并通过图标引导重启更新的做法甚是恼人，本文描述了禁止Chrome自动更新的7种方法。

## 搞清楚Chrome是怎么进行自动更新的?

Github中搜索google update，结果列表中第一个项目是[omaha](https://github.com/google/omaha)。打开了解到，它是Chrome的更新程序Google Update for Windows的一个开源版本。在omaha自述文件提供的[unofficial tutorial](https://fman.io/blog/google-omaha-tutorial/)中, 可以了解到chrome的更新机制。下面摘抄过来作为记录。

## Google Update 的工作原理

当安装Chrome时, 运行的便是Omaha。该程序向Google服务器查询最新版本的Chrome，服务器返回可用的Chrome.exe文件的URL。Omaha下载并运行，同时提供一个带有进度的GUI：

![Chrome 安装](chrome_installing.png)

下载得到的exe文件会把Chrome安装到 `C:\Program Files (x86)\Google\Chrome` 或者 `C:\Program Files\Google\Chrome` 之中。在HKLM\Software\Wow6432Node\Google\Update\Clients创建注册表项：

![Chrome 注册表项](chrome_registry_keys.png)

为了与现有系统实现最大兼容性, Omaha是32位应用程序，所以会把注册项放到Wow6432Node之下，












[Google Omaha Tutorial](https://omaha-consulting.com/google-omaha-tutorial-chrome-updater)
[32-bit and 64-bit Application Data in the Registry](https://docs.microsoft.com/en-us/windows/win32/sysinfo/32-bit-and-64-bit-application-data-in-the-registry)
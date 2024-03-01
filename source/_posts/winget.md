---
title: Winget
date: 2021-12-01 06:13:31
updated: 2021-12-01 06:13:31
categories:
- 环境配置
tags:
- windows
---

2020年5月发布，2021年5月7日 winget 发布了 1.0 版。在这之前有[Chocolatey](https://chocolatey.org/)还有 [Scoop](https://scoop.sh/)

主要是配置安装路径，包括默认安装路径、绿色版Portable路径、架构


## 常用命令

### install

`-I`等价于`--location`，指定要追加其后的安装路径，需要软件包支持（manifest中指定`InstallLocationRequired`）

```powershell
winget install 包名 -I "D:\Apps\"
```

### upgrade
```powershell
winget upgrade
```
不加参数会列出可用更新。

## 配置文件

`winget settings`打开配置文件。

```json
{
    "$schema": "https://aka.ms/winget-settings.schema.json",

    // For documentation on these settings, see: https://aka.ms/winget-settings
    // "source": {
    //    "autoUpdateIntervalInMinutes": 5
    // },

    "installBehavior": {
        "defaultInstallRoot": "D:/Apps", // 默认安装的根目录
        "portablePackageUserRoot": "D:/Apps", // 绿色版用户安装路径
        "portablePackageMachineRoot": "C:/Program Files/Packages/Portable", // 绿色版全部用户安装路径
        "preferences": {
            "architectures": ["x64"] // 架构
        }
    },
    "uninstallBehavior": {
        "purgePortablePackage": true //卸载时清除安装的绿色版文件
    }
}
```

## 疑虑

我有一些疑虑，比如在很多软件的安装程序中，会自动加入开机自启动或者把自己设置成默认打开方式，winget是如何避免这些的呢，upgrade时是否会忽略掉原来的目录。

一些软件安装时需要指定很多选项，如果

Winget提交流程是：
- 厂商编写相关程序的Manifest提交一个Pull request(PR) 到 [winget-pkgs](https://github.com/microsoft/winget-pkgs) 仓库
- 自动化流程验证或者人工审核是否符合[包管理策略](https://learn.microsoft.com/en-us/windows/package-manager/package/windows-package-manager-policies)

不清楚这个自动化验证是严格到什么程度，通过使用慢慢测试吧。

同花顺远航版upgrade会自己再次安装到默认位置，而不是之前的安装路径，之前的路径下程序依然保留没有卸载；同样Calibre也是，但原路径的软件被卸载删除。有些软件则会遵循之前的安装路径，比如网易云音乐。另外一提，同花顺远航版的安装器默认会开机自启动，而winget安装没有。

因此可以说，虽然设置中指定了安装路径，但**安装位置仍是不确定的**，哪怕是更新（同花顺，calibre），具体表现取决于程序自己的安装器。

## 参考

1. [Use the winget tool to install and manage applications | Microsoft Learn](https://learn.microsoft.com/th-th/windows/package-manager/winget/)
2. [winget-cli/Settings.md at master · microsoft/winget-cli · GitHub](https://aka.ms/winget-settings)
3. [GitHub - microsoft/winget-pkgs: The Microsoft community Windows Package Manager manifest repository](https://github.com/microsoft/winget-pkgs)
4. [Windows Package Manager repository policies | Microsoft Learn](https://learn.microsoft.com/en-us/windows/package-manager/package/windows-package-manager-policies)

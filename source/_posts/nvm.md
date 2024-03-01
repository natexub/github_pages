---
title: NVM - Node Version Manager Node版本管理
date: 2020-10-19 23:45:25
updated: 2020-10-19 23:45:25
categories:
- 环境配置
tags:
- Node.js
---


由于Node.js的版本变化特别快，所以安装Node.js建议使用版本管理器，根据不同项目需求，于不同Node.js版本之间切换。[Node Version Manager](https://github.com/nvm-sh/nvm)，但NVM只适用于Mac/Linux，不支持Windows。因此，需要使用最流行的[NVM for Windows](https://github.com/coreybutler/nvm-windows)来安装Node.js和NPM(Node Package Manager)。

## NVM for Windows

### 下载
发布页面：[Releases · coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows/releases)
依照仓库Readme安装。

### 常用命令
列出当前安装的 Node 版本
```powershell
nvm ls
```

安装最新版本的 Node.js（用于测试最新的功能改进，但比 LTS 版本更容易出现问题）
```powershell
nvm install latest
```

安装 Node.js 的最新稳定 LTS 版本（推荐）, 首先查找当前 LTS 版本号是什么。然后安装 LTS 版本号。
```powershell
PS D:\Projects> nvm list available

|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    19.0.0    |   18.12.0    |   0.12.18    |   0.11.16    |
|   18.11.0    |   16.18.0    |   0.12.17    |   0.11.15    |
|   18.10.0    |   16.17.1    |   0.12.16    |   0.11.14    |
|    18.9.1    |   16.17.0    |   0.12.15    |   0.11.13    |
|    18.9.0    |   16.16.0    |   0.12.14    |   0.11.12    |
|    18.8.0    |   16.15.1    |   0.12.13    |   0.11.11    |
|    18.7.0    |   16.15.0    |   0.12.12    |   0.11.10    |
|    18.6.0    |   16.14.2    |   0.12.11    |    0.11.9    |
|    18.5.0    |   16.14.1    |   0.12.10    |    0.11.8    |
|    18.4.0    |   16.14.0    |    0.12.9    |    0.11.7    |
|    18.3.0    |   16.13.2    |    0.12.8    |    0.11.6    |
|    18.2.0    |   16.13.1    |    0.12.7    |    0.11.5    |
|    18.1.0    |   16.13.0    |    0.12.6    |    0.11.4    |
|    18.0.0    |   14.20.1    |    0.12.5    |    0.11.3    |
|    17.9.1    |   14.20.0    |    0.12.4    |    0.11.2    |
|    17.9.0    |   14.19.3    |    0.12.3    |    0.11.1    |
|    17.8.0    |   14.19.2    |    0.12.2    |    0.11.0    |
|    17.7.2    |   14.19.1    |    0.12.1    |    0.9.12    |
|    17.7.1    |   14.19.0    |    0.12.0    |    0.9.11    |
|    17.7.0    |   14.18.3    |   0.10.48    |    0.9.10    |

This is a partial list. For a complete list, visit https://nodejs.org/en/download/releases
```

```powershell
PS D:\Projects> nvm install 18.12.0
Downloading node.js version 18.12.0 (64-bit)...
Extracting...
Complete


Installation complete. If you want to use this version, type

nvm use 18.12.0
```

安装完需要的 Node.js 版本号后，选择要使用的版本`nvm use 18.12.0。

**注意：** 切换新版本后，需要为每个版本重新安装安装过的全局工具（e.g. yarn）。
```powershell
nvm use 14.0.0
npm install -g yarn
nvm use 12.0.1
npm install -g yarn
```

除了nvm-windows当然还有其他方案可供选择比如：
- [nvs](https://github.com/jasongin/nvs)一个跨平台的`nvm`替代方案，可以与VSCode集成。
- [Volta](https://github.com/volta-cli/volta#installing-volta)LinkedIn 团队的新版本经理，声称提高了速度和跨平台支持。

### 原理
从其安装时创建的环境变量名`NVM_SYMLINK`也可以知晓，就是之前写过的软链接，建立一个软链接，环境变量指向这。由软件控制修改链接的目标。

## 参考

1. [Set up NodeJS on native Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-windows)
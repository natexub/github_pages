---
title: 新建React Native项目
date: 2022-01-20 00:18:08
updated: 2022-01-20 00:18:08
categories:
- New to
tags:
- ReactNative
---

备忘记录
[搭建开发环境 · React Native 中文网](https://www.reactnative.cn/docs/environment-setup)
[搭建开发环境·React Native](https://reactnative.dev/docs/environment-setup)
[Initializing new project](https://github.com/react-native-community/cli/blob/main/docs/init.md)

## React Native CLI 命令行工具（完整沙盒环境）

需要：
- Node.js 开发环境
- Android 开发环境
- React Native CLI

### React Native CLI的安装

现在React流行直接使用npx执行package，而不是使用一个固定版本全局安装的CLI工具来新建项目。因此，如果存在，把之前全局安装的CLI工具删掉。

```powershell
npm uninstall -g react-native-cli @react-native-community/cli
```

```powershell
npx react-native init [项目名称]
```

带模板

```powershell
npx react-native init rndemo --template react-native-template-typescript
```


## 问题记录

### react-devtools

#### 安装报错

##### 问题表现

```powershell
PS D:\Projects> yarn global add react-devtools
yarn global v1.22.19
[1/4] Resolving packages...
info There appears to be trouble with your network connection. Retrying...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
[-/2] ⠠ waiting...
error D:\env\yarn-global\node_modules\electron: Command failed.
Exit code: 1
Command: node install.js
Arguments:
Directory: D:\env\yarn-global\node_modules\electron
Output:
RequestError: connect ETIMEDOUT 20.205.243.166:443
    at ClientRequest.<anonymous> (D:\env\yarn-global\node_modules\@electron\get\node_modules\got\source\request-as-event-emitter.js:178:14)
    at Object.onceWrapper (node:events:628:26)
    at ClientRequest.emit (node:events:525:35)
    at ClientRequest.origin.emit (D:\env\yarn-global\node_modules\@szmarczak\http-timer\source\index.js:37:11)
    at TLSSocket.socketErrorListener (node:_http_client:494:9)
    at TLSSocket.emit (node:events:513:28)
    at emitErrorNT (node:internal/streams/destroy:157:8)
    at emitErrorCloseNT (node:internal/streams/destroy:122:3)
    at processTicksAndRejections (node:internal/process/task_queues:83:21)
```

##### 解决过程

根据log可以看到，是执行electron包下`install.js`时出的请求超时问题。
根据[npm explorer](https://www.runpkg.com/?electron@21.2.0/index.js)工具可看到不包含dist目录，执行install.js时才通过 [`@electron/get`](https://github.com/electron/get)下载预构建的二进制文件。那么这一步的就不依赖npm和yarn，不是npm源的问题。

##### 解决办法
1. 全局走代理
2. 配置国内的Electron Mirror
根据 [@electron/get](https://github.com/electron/get#using-environment-variables-for-mirror-options)Readme文件和参考1描述，Windows需要配置环境变量`ELECTRON_MIRROR`为：`https://npmmirror.com/mirrors/electron/`


#### 一直等待连接模拟器

##### 问题表现

![[react-devtools.png]]

##### 解决过程

读react-devtools的[文档](https://github.com/facebook/react/tree/main/packages/react-devtools#usage-with-react-native)有下面一段话：

>If you're not using a local simulator, you'll also need to forward ports used by React DevTools:
>`adb reverse tcp:8097 tcp:8097`
>If you're using React Native 0.43 or higher, it should connect to your simulator within a few seconds. (If this doesn't happen automatically, try reloading the React Native app.)

`adb reverse tcp:8097 tcp:8097`
这条命令的意思是，Android允许我们通过ADB，把Android上的某个端口映射到电脑（adb forward），或者把电脑的某个端口映射到Android系统（adb reverse），在这里假设电脑上开启的服务，监听的端口为8081。Android手机通过USB连接电脑后，在终端直接执行adb reverse tcp:8081 tcp:8081，然后在手机中访问127.0.0.1:8081，就可以访问到电脑上启动的服务了。

然后Reload就可以了。

## 参考

1. [Advanced Installation Instructions | Electron](https://www.electronjs.org/docs/latest/tutorial/installation#custom-mirrors-and-caches)





















## Expo GO （简易沙盒环境）

[Expo](https://expo.dev/client)
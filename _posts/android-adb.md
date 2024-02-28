---
title: android-adb
date: 2018-10-30 18:51:28
updated: 2018-10-30 18:51:28
categories:
- Android
tags:
- Android
---
## ADB

### 常用的adb命令

#### adb forward

```powershell
 forward [--no-rebind] LOCAL REMOTE
     forward socket connection using:
       tcp:<port> (<local> may be "tcp:0" to pick any open port)
       localabstract:<unix domain socket name>
       localreserved:<unix domain socket name>
       localfilesystem:<unix domain socket name>
       dev:<character device name>
       jdwp:<process pid> (remote only)
       vsock:<CID>:<port> (remote only)
       acceptfd:<fd> (listen only)
```

将PC上LOCAL端口的请求转发到移动设备的REMOTE端口上。


##### 应用场景

把Android上的某个端口映射到PC

##### 示例

 `adb forward tcp:6100 tcp:7100`
 `adb forward --list`
 `adb forward --remove tcp:6100`

#### adb reverse
```powershell
 reverse [--no-rebind] REMOTE LOCAL
     reverse socket connection using:
       tcp:<port> (<remote> may be "tcp:0" to pick any open port)
       localabstract:<unix domain socket name>
       localreserved:<unix domain socket name>
       localfilesystem:<unix domain socket name>
```

##### 应用场景

不同网络环境，查看PC上的API服务器地址并不方便。使移动设备通过ADB访问PC的后端。手机访问`http://locahost:80`映射到计算机的3000端口。
```powershell
PS D:\Projects> adb reverse tcp:80 tcp:3000

PS D:\Projects> adb reverse --list
host-32 tcp:8097 tcp:8097

PS D:\Projects> adb reverse --remove tcp:8097
```




---
title: windows沙盒和Jetbrains软件发生端口占用冲突
date: 2021-10-13 09:33:14
updated: 2021-10-13 09:33:14
categories:
    - 琐碎问题
tags:
---

更改动态端口范围，来解决端口占用问题。

### 记录默认端口范围

```cmd
C:\Windows\system32>netsh int ipv4 show dynamicport tcp

协议 tcp 动态端口范围
---------------------------------
启动端口        : 1024
端口数          : 13977


C:\Windows\system32>netsh int ipv6 show dynamicport tcp

协议 tcp 动态端口范围
---------------------------------
启动端口        : 1024
端口数          : 13977


C:\Windows\system32>netsh int ipv4 show dynamicport udp

协议 udp 动态端口范围
---------------------------------
启动端口        : 49152
端口数          : 16384


C:\Windows\system32>netsh int ipv6 show dynamicport udp

协议 udp 动态端口范围
---------------------------------
启动端口        : 49152
端口数          : 16384
————————————————

```

### 修改动态端口范围

```cmd
netsh int ipv4 set dynamicport tcp start=49152 num=16384
```

### 重启
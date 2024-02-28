---
title: 为windows store中的app配置代理
date: 2020-11-10 01:42:22
updated: 2020-11-10 01:42:22
categories:
tags:
- proxy
- windows
---

所有的UWP应用都运行在隔离沙箱中，阻止网络流量发送到本机（loopback回环）。需要将应用排除网络隔离。

## CheckNetIsolation

```powershell
foreach ($n in (get-appxpackage).packagefamilyname) {checknetisolation loopbackexempt -a -n="$n"}
```

使用说明：

![CheckNetIsolation.exe](CheckNetIsolation.png)


## 修改组策略[废弃]

测试该方法**无效**不能让app走代理，并与之前不同app网络长时间无响应。

- 运行 GPEDIT.MSC或者直接搜索组策略
- 计算机配置（Computer Configuration) > 管理模板（Administrative Templates）> 网络 > 网络隔离（Network Isolation)
	![本地组策略编辑器](gpedit.png)
- 选择应用的Internet代理服务器（Internet proxy servers for apps），这里有两个（区别在哪没搞懂）。
- 域代理下输入代理的地址。
	![应用的Internet代理服务器](gpedit2.png)
- 应用后重启。

### 参考

[如何为 Windows 10 UWP 应用设置代理 - 知乎](https://zhuanlan.zhihu.com/p/29989157)

[Communicating with Localhost - Windows IoT | Microsoft Learn](https://learn.microsoft.com/en-us/windows/iot-core/develop-your-app/loopback#enabling-loopback-for-a-uwp-application)

[Isolating Windows Store Apps on Your Network | Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831418(v=ws.11)?redirectedfrom=MSDN)

[How to set up Proxy for Windows Store apps in Windows 11/10](https://www.thewindowsclub.com/setup-proxy-metro-application-windows-8)


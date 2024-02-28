---
title: 如何释放被占用的端口
date: 2021-09-06 16:24:51
updated: 2021-09-06 16:24:51
categories:
tags:
---

每次都要搜，干脆花半小时总结一劳永逸。

## 一. 如何找到占用着目标端口的程序

### Windows

#### 1. PowerShell Get-Process

##### TCP

语法：

```powershell
Get-Process -Id (Get-NetTCPConnection -LocalPort [目标端口号]).OwningProcess
```

示例：

```powershell
Get-Process -Id (Get-NetTCPConnection -LocalPort 3000).OwningProcess
```
结果：

```powershell
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    335     172   220252     229204       8.14  31168   1 node
```

*Id 即为 PID

##### UDP

语法：

```powershell
Get-Process -Id (Get-NetUDPEndpoint -LocalPort [目标端口号]).OwningProcess
```

示例：

```powershell
Get-Process -Id (Get-NetUDPEndpoint -LocalPort 56045).OwningProcess
```

结果：

```
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    443      28    15936       9888       1.25  18544   0 QQProtect
```

*Id 即为 PID

参考：

>[Get-Process (Microsoft.PowerShell.Management) - PowerShell | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-process)
>
>[Get-NetTCPConnection (NetTCPIP) | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/nettcpip/get-nettcpconnection)
>
>[Get-NetUDPEndpoint (NetTCPIP) | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/nettcpip/get-netudpendpoint)

#### 2. CMD

windows使用findstr替代grep

语法：

```cmd
netstat -a -b -n -o|findstr [目标端口号]
```

示例：

```cmd
netstat -a -b -n -o|findstr 3000
```

结果：

```cmd
  TCP    0.0.0.0:3000           DESKTOP-P8GN8IS:0      LISTENING       31168
  TCP    127.0.0.1:3000         DESKTOP-P8GN8IS:9929   ESTABLISHED     31168
  TCP    127.0.0.1:9929         DESKTOP-P8GN8IS:3000   ESTABLISHED     17424
```

*最后一列为PID

参考：

>[netstat | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/netstat)
>
>[findstr | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr)
>
>[管道（进程间通信）Pipes (Interprocess Communications) - Win32 apps | Microsoft Docs](https://docs.microsoft.com/en-us/windows/win32/ipc/pipes)

#### 3. 资源监视器（resmon）

优势：记不住命令直接使用系统自带GUI

如图：

![resmon.exe](resmon.png)

#### 4.TCPView

优势：专用工具，可直接右键结束程序

如图：

![TCPView](tcpview.png)

参考：

>[TCPView for Windows - Windows Sysinternals | Microsoft Docs](https://docs.microsoft.com/en-us/sysinternals/downloads/tcpview)

## 二. 结束占用端口的程序

### Windows

#### 1. Stop-Process (powershell下）

语法：

```powershell
Stop-Process -Id [之前得到的Id(PID)] -Confirm -PassThru
```

示例：

```powershell
Stop-Process -Id 1948 -Confirm -PassThru
```

结果：

```powershell
确认
是否确实要执行此操作?
正在目标“有道云笔记 (1948)”上执行操作“Stop-Process”。
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“Y”):

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    287      91    95504      72168      32.66   1948   1 有道云笔记
```

此命令停止记事本进程的特定实例。它使用进程 ID 3952 来标识进程。Confirm参数指示 PowerShell在停止进程之前提示您。因为提示除了 ID 之外还包括进程名称，这是最佳实践。PassThru参数将进程对象传递给格式化程序进行显示。没有这个参数，命令后不会有显示Stop-Process。（机翻）

#### 2.taskkill (CMD下)

语法：

```cmd
taskkill /f /t /im [之前得到的PID]
```

示例：

```cmd
taskkill /f /t /im 19144
```

结果：

```cmd
成功: 已终止 PID 19192 (属于 PID 19144 子进程)的进程。
成功: 已终止 PID 19288 (属于 PID 19144 子进程)的进程。
成功: 已终止 PID 19400 (属于 PID 19144 子进程)的进程。
成功: 已终止 PID 19144 (属于 PID 736 子进程)的进程。
```

参考：

> [Stop-Process (Microsoft.PowerShell.Management) - PowerShell | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/stop-process?view=powershell-7.2)
>
> [taskkill | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/taskkill)


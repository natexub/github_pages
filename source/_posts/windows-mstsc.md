---
title: windows mstsc相关问题
date: 2019-10-01 01:12:51
updated: 2019-10-01 01:12:51
categories:
tags:
- windows
---

## windows10 mstsc默认不允许空密码。

1. 打开本地安全策略（ secpol.msc ）
2. 本地策略 > 安全选项 > 使用空密码的本地账户只允许进行控制台登录 > 禁用

![本地安全策略](secpol.png)

## 保持唤醒

目标主机安装powertoys，其中有个工具叫保持唤醒。

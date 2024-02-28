---
title: 硬链接和软链接
date: 2020-10-19 23:54:11
updated: 2020-10-19 23:54:11
categories:
    - 操作系统**
tags:
---

如果C盘满了，怎么把软件转移到其他目录？
多个文档软件如何共同使用同一个目录作为仓库？
不支持其他目录同步的云盘比如Onedrive如何同步其他位置的目录文件？
本文按道理应该属于本科操作系统课程知识，但平时却应用频繁，重温一下这个知识。

## 概念

软链接和硬链接概念属于操作系统的文件系统中的概念。

## 方法

### CMD

#### 语法

```cmd
mklink [[/d] | [/h] | [/j]] <link> <target>
```

#### 参数




[mklink | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/mklink)

[符号链接、硬链接及其在 Windows 上的应用举例 - 少数派](https://sspai.com/post/66834)
[符号链接 - 维基百科](https://en.wikipedia.org/wiki/Symbolic_link)
[Hard link - Wikipedia](https://en.wikipedia.org/wiki/Hard_link)
---
title: 换行符
date: 2018-11-08 04:48:14
updated: 2018-11-08 04:48:14
categories:
tags:
---


**换行符（Newline）** 是字符编码规范（如ASCII、EBCDIC、Unicode等）中的控制字符或控制字符序列。此字符或一系列字符，用于表示一行文本的结束和新行的开始。

- 回车（carriage return缩写CR）：`\r`,`0x0D`将光标移动到行首而不前进到下一行。
- 换行（line feed缩写LF）：`\n`,`0x0A`— 将光标向下移动到下一行而不返回到行首。

CRLF、`\r\n`，`0x0D0A`将光标向下移动到下一行，然后移动到行首（为什么倒过来了？）

UNIX/Linux 使用的是单个字符 LF，DOS/Windows 一直使用两个字符CRLF来换行。
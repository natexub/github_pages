---
title: NodeJS 文件操作
date: 2021-02-17 14:02:07
updated: 2021-02-17 14:02:07
categories:
tags:
 - Node.js
---

使用原生`fs`的替代品[fs-extra](https://github.com/jprichardson/node-fs-extra)，为原生一些方法提供了`Promise`支持。



## 基本方法

### `fs.open`

`fsPromises.open(path, flags[, mode])`

[`flag`](https://nodejs.org/dist/latest-v19.x/docs/api/fs.html#file-system-flags)常用的有:


## 使用流

### 单行读写

涉及方法：

`fs.createReadStream`, `fs.createWriteStream`, 

引入依赖：

---
title: Chrome中的线程和任务
date: 2021-06-16 11:33:38
updated: 2021-06-16 11:33:38
categories:
  - 浏览器
tags:
  - chrome
---

当我们在讨论EventLoop的时候, 如果深究比如涉及到现代浏览器的一些原理. 本文尝试总结一下chrome的多进程架构和其中的渲染器进程


## Chrome多进程架构

Chrome 有一个多进程架构(Multi-process Architecture)，而每个进程有多个多线程。之所以使用多个进程, 是利用操作系统独立进程相互隔离这一功能, 使整个Chrome应用**免于渲染引擎或其他组件中的错误和故障的影响继而导致崩溃**; 限制了每个渲染引擎进程对其他进程和系统其余部分的访问, 从而起到内存保护和访问控制等作用;

多进程架构会带来一个问题, 每个进程都有自己的私有内存空间, 同时每个标签页对应的进程都有一套相同的基础设施副本, 因此, 会导致更多的内存使用. Chrome通过服务化, 限制进程数等方式来寄生和控制内存.

一个进程可以要求操作系统启动另一个进程来运行不同的**任务**。这会为新进程新分配内存。如果两个进程需要通信，它们通过使用进程间通信(IPC)来实现。

![浏览器任务](task-manager.png)

这里不再赘述计算机科学中进程,线程,IPC等概念。

## chrome中的进程

- 浏览器进程,或不妨直接称之为浏览器(Browser).
  - 控制应用程序, 包括地址栏、书签、后退和前进按钮。
  - 处理网络浏览器中不可见的特权部分，例如文件访问。
  - 运行UI
  - 管理其他进程
- 网络进程: 主要负责页面的网络资源加载，之前是作为一个模块运行在浏览器进程里面的，直至最近才独立出来，成为一个单独的进程。
- 渲染器(renderer), 也就是我们说的浏览器内核
  - 负责页面渲染，脚本执行，事件处理等
  - **每个tab标签页对应一个独立的渲染器进程**
- 插件(Plugin)
  - 控制网站使用的任何插件
- GPU: 独立于其他进程处理3D绘制等GPU任务。由于GPU要处理来自多个应用程序的请求并将它们绘制在同一个表面上, 所以GPU被分成多个进程.

![浏览器进程](processes.png)

下面重点讨论一下渲染器进程.

## 渲染器(内核)

### 渲染进程中的线程

渲染器进程包含以下线程:

- GUI渲染线程
  - 负责渲染页面，布局和绘制
  - 页面需要重绘和回流时，该线程就会执行
  - 与js引擎线程互斥，防止渲染结果不可预期
- JS引擎线程
  - 负责处理解析和执行javascript脚本程序
  - 只有一个JS引擎线程（单线程）
  - **与GUI渲染线程互斥，防止渲染结果不可预期**
- 定时触发器线程
  - setInterval与setTimeout所在的线程
  - 定时任务并不是由JS引擎计时的，是由定时触发线程来计时的
  - 计时完毕后，通知事件触发线程
- 异步http请求线程
  - 浏览器有一个单独的线程用于处理AJAX请求
  - 当请求完成时，若有回调函数，通知事件触发线程



## 参考

[Threading and Tasks in Chrome](https://chromium.googlesource.com/chromium/src.git/+/HEAD/docs/threading_and_tasks.md)

[Multi-process Architecture](https://www.chromium.org/developers/design-documents/multi-process-architecture/)

[Inside look at modern web browser (part 3)](https://developer.chrome.com/blog/inside-browser-part3/)






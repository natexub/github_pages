---
title: 记录一些我的模板
date: 2022-10-30 19:23:15
updated: 2022-10-30 19:23:15
categories:
- obsidian
tags:
- obsidian
---

## 标题

### 解决问题

要写一些记录问题的段落，形如：

```md
#### 问题1
##### 问题表现
xxx
##### 解决过程
xxx
##### 解决方法
xxx
```


```javascript
<%*
let text = await tp.file.selection()
	.split('\n')
	.map(t => t.match(/^[#]+\s+/gi) ? `#${t}` : t)
	.join('\n');
%><% text %>
```

### 用法示例

一些命令或者语法示例。

```md
## adb reverse
### 应用场景
### 示例
### 参数说明
```
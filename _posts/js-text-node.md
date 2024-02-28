---
title: 文本节点
date: 2021-03-22 18:30:07
updated: 2021-03-22 18:30:07
categories:
tags:
- JavaScript
- DOM
---

今天了解到一个问题，获取`innerHTML`的值发现其中的内容部分被转义部分没有。

![问题1](issue1.png)

MDN中文和西语等版本的文档中（英文版没有）有以下备注：

中文版：

![中文版](note1.png)

西语版：

![西语版](note2.png)

接下来开始实验。

```js
const divElement = document.createElement('div')
const hElement =  document.createElement('h1')
const textNode = document.createTextNode('<div><strong>Hello div</strong></div>')
const textNode2 = document.createTextNode('<div><strong>Hello h1</strong></div>')
divElement.appendChild(textNode)
hElement.appendChild(textNode2)
document.body.appendChild(divElement)
document.body.appendChild(hElement)

const divElement2 = document.createElement('div')
divElement2.innerHTML = '<div><strong>Hello div2</strong></div>'
document.body.appendChild(divElement2)

```

结果如下:

![值](result1.png)

![显示](result2.png)

这里涉及到相关的知识点**文本节点**和API:

[`HTMLElement.innerHTMl`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/innerHTML)、
[`HTMLElement.innerText`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/innerText)：获取标签内部的文本，设置标签内部的文本
[`HTMLElement.textContent`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent)、
[`Document.createTextNode()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createTextNode)、

DOM node 有以下几种类型：
[Node.nodeType - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType)
随便找的：[节点的十二种类型 - 前端小白银 - 博客园](https://www.cnblogs.com/forever-ljf/p/16500357.html)
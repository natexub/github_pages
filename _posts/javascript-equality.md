---
title: JS中的比较
date: 2018-11-23 18:47:14
updated: 2018-11-23 18:47:14
categories:
tags:
- Javascript
---

JavaScript中有三种比较值的方式：
- [`===`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) 严格相等比较（三等号）
- [`==`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality) 相等比较（双等号）
- [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)（ES6）

简要区别：

- 双等号会执行隐式类型转换，并处理`NaN`、`+0`、`-0`。
- 三等号不进行类型转换
- `Object.is()`不进行类型转换也不对`NaN`、`-0`、`-0`进行特殊处理

经常产生疑惑的点在于`==`的类型转换的比较规则，`===`则更简单清楚。



## 示例


```js
"1" == 1 //真
null == undefined == false == 0 //真
NaN == NaN //假
"" == 0 //真
```

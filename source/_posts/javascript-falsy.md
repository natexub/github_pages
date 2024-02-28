---
title: JS中虚值的检查和处理
date: 2020-11-09 16:48:38
updated: 2020-11-09 16:48:38
categories:
tags:
- Javascript
---

JavaScript 在需要用到布尔类型值的上下文中使用强制类型转换 (Type Conversion ) 将值转换为布尔值，例如条件语句、循环语句、`==`。
在 JavaScript 中有 8 个 **falsy** 值（忽略document.all），其中`null`和`undefined`称为**nullish**值

| 值           | 说明                  |
| ------------ | --------------------- |
| false        |                       |
| 0            | 0.0 0x0               |
| -0           | -0.0 -0x0             |
| 0n           | 0x0n BigInt类型的0    |
| "", '', \`\` |                       |
| null         |                       |
| undefined    |                       |
| NaN          |                       |
| document.all | 忽略\[\[IsHTMLDDA\]\] |

理解和熟练使用逻辑操作符`!!`, `??`, `?.`

## !!的使用 - 转换成布尔值

参考：
[JavaScript中 !! 运算符能做什么？-码云笔记](https://www.mybj123.com/6540.html)
[Logical NOT (!) - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_NOT#double_not_!!)

`!!`并非运算符，而是连续两个逻辑非操作。

将右操作数转换成相应的布尔值。


## ?? - 空值合并运算符

当左操作数为nullish值时，返回右操作数，否则返回左操作数。

### 使用示例

基本用法

```javascript
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```


[Nullish coalescing operator (??) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)


## ?. 可选链运算符

访问对象的属性或调用函数，如果对象是nullish值，它会返回undefined而不是抛出错误。

### 使用示例

代替一种惯用的访问对象属性的方式：

```js
const nestedProps = obj.first && obj.first.secend
```

```js
const nestedProp = obj.first?.second;
```

当然，以上两个并不能等价。假如`obj.first`是个 Falsy 值，但不是`null`或者`undefined`，比如`0`。前者短路返回`0`，后者返回`undefined`

### 注意

会有以下误用：


````ad-error

```js
const a = {} //没有元素的对象
a?.aa = 100 // SyntaxError: Invalid left-hand side in assignment
```

````

不能进行左侧赋值，可使用`??=`

## 应用场景

### 默认值

与`??`相比，`||`不返回任何虚值，即除了nullish值，`0`，`""`，`NaN`，`false`都不会返回。

```javascript
let foo; // undefined
const someDummyText = foo || "hello!"; //"hello!"
```

```javascript
const count = 0;
const text = "";

const qty = count || 42; // 42 而不是 0
const message = text || "hi!"; // "hi!" 而不是 ""
```

### 访问对象可能不存在的属性

与`?.` 搭配。

```js
const foo = { someFooProp: "hi" };

console.log(foo.someFooProp?.toUpperCase() ?? "not available"); // "HI"
console.log(foo.someBarProp?.toUpperCase() ?? "not available"); // "not available"
```

### 调用可能不存在的方法

```js
const result = someInterface.customMethod?.();//不存在返回undefined
```


### 传参检查

```js
function doSomething(onContent, onError) {
  try {
    // Do something with the data
  } catch (err) {
    onError?.(err.message); // No exception if onError is undefined
  }
}
```

### 对可能不存在的左侧变量进行赋值

```js
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// 不赋进行值 仍然是50

a.speed ??= 25;
console.log(a.speed);
// undefined属性，进行赋值，25

```

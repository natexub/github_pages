---
title: JS中的参数
date: 2021-11-10 00:05:05
updated: 2021-11-10 00:05:05
categories:
tags:
- Javascript
---

函数参数有在两种情况下有两个概念：形参（parameter）和实参（argument）

- 形参（parameter）是**函数定义**中列出的变量。
- 实参（argument）是**调用函数**时提供给函数的值。

## rest 语法

### rest 参数
js中执行可变参数函数（variadic functions）的方法。最后一个形参前加`...`，使之成为剩余参数（rest parament），一个容纳了提供的其他省剩余参数的数组。

```js
function sum(...theArgs) {
  let total = 0;
  for (const arg of theArgs) {
    total += arg;
  }
  return total;
}

console.log(sum(1, 2, 3));
// expected output: 6

console.log(sum(1, 2, 3, 4));
// expected output: 10
```

### rest 属性（property）

类似于rest参数，`...rest`可以结束对对象和数组的解构，把剩余的属性存储到一个变量之中。

```js
const {a, ...others} = {a:1, b:2, c:3}
console.log(others); //{b:2, c:3}

const [first, ...others2] = [1,2,3]
console.log(others2); // [2,3]
```

## Arguments 对象



包含了传递给该函数实参的Array-like对象。
所有非箭头函数中都会有一个arguments局部变量。


### 把Arguments对象转成数组

```js
const args = Array.from(arguments);
// 或者
const args = [...arguments]
// 或者
const args = Array.prototype.slice.call(arguments);
```


### Arguments对象和函数参数的关系

#### 非严格模式
非严格模式下，`arguments`里的参数和函数参数都是指向同一个值的引用，对`arguments`的修改会直接影响函数参数。

```js
function func(a) {
  arguments[0].aa = 99; // 修改 arguments[0] 也会修改变量a的值
  console.log(a);
}
func({}); // {aa: 99}

function func2(a) {
  a.aa = 99; // 修改变量 a 的值也会修改 arguments[0]
  console.log(arguments[0]);
}
func2({}); // {aa: 99}
```

#### 严格模式

严格模式下，`Arguments`对象和参数变量之间不会互相影响。

```js
function func(a) {
  arguments[0] = 99; // updating arguments[0] also updates a
  console.log(a);
}
func(10); // 10
```

无法访问`arguments.callee`

>TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them


### 应用示例

#### 计算实际传递给该函数的实参数量

```js
function foo(){
	console.log(arguments.length)
}
foo(1,2,3) //3
```

多说一句，要获取**函数声明时形参**的数量，使用函数的`length`属性

```js
function foo (arg1,arg2,...manyMoreArgs){}
console.log(foo.length) //2，不包括剩余参数
```


### 问题

#### Function.prototype.arguments

`Function`对象有一个`Function.prototype.arguments` （已被标注为废弃）。这个和函数块作用域中局部变量中的Arguments不是一回事。
见：[Function.prototype.arguments - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/arguments)


## rest parameters参数和Arguments对象的区别

还是强行比较一下吧。

- 剩余参数是Array，而Arguments对象是Array-like
- `...restParam`不包含剩余参数之前的形参
- Arguments对象附带了指向当前执行函数的`callee`属性

### 优先选择rest-params

除了 Arguments 对象文档的描述中，ESLint 也有一项可配置的规则`prefer-rest-params`。处理可变参数的场景，优先选择剩余参数语法。
原因是Arguments不是Array对象，使用起来不方便。

## 展开语法Spread（...)

在函数参数或数组字面量中扩展可迭代对象；在对象中

```js
myFunction(a, ...iterableObj, b)
[1, ...iterableObj, '4', 'five', 6]
{ ...obj, key: 'value' }
```

## rest 语法和展开语法

扩展运算符和剩余参数语法看起来一样，但实际在某种程度上相反。展开语法是将数组扩展成多个元素，而rest语法则是将多个元素「凝聚」成单个元素。

## 参考

[The arguments object - JavaScript | MDN](https://developer-mozilla-org.translate.goog/en-US/docs/Web/JavaScript/Reference/Functions/arguments?_x_tr_sl=en&_x_tr_tl=zh-CN&_x_tr_hl=en&_x_tr_pto=wapp)
[Rest parameters - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
[prefer-rest-params - ESLint - Pluggable JavaScript Linter](https://eslint.org/docs/latest/rules/prefer-rest-params)
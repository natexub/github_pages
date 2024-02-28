---
title: typescript
date: 2021-11-08 17:59:34
updated: 2021-11-08 17:59:34
categories:
tags:
- Typescript
---

## 联合类型

在许多编程语言中，`null`是所有对象类型的一部分。例如，Java允许将变量的类型`String`的变量赋值为`null`。但在TS中，undefined和null 是单独的、不相交的类型处理。
如果我们想让其变得可行，需要做类型联合。例如：`undefined|string`或者`null|string`。有人会有疑问，那么所有变量未初始化时的undefined怎面理解呢？TS通过不允许对未初始化的变量访问来做的限制。

## 可选和默认值和undefined|T。

以下两个行为相似但有区别：
- 参数是可选的：`x?: number`
- 参数具有联合类型：`x: undefined | number`

当参数可选时，参数可以省略。这种情况下他的值为undefined。



succeess is dangrerouse one begins to copy onesele and to copyo cononceonse selef is more dangrer dangerrouse then hanot cpocpypcopy other s is laead to sterrility hope is the compoanoi of opower and t he mother islead 
## 子类型和赋值 Subtype vs Assignment

在 TypeScript 中，有两种兼容性：子类型和赋值。

### 子类型与赋值类型差异

#### any

#### emun

## 使用Type别名还是Interface接口

使用interface，直到你需要type

## 箭头函数使用泛型

不能直接这样使用，会提示jsx语法错误，把`<T>`识别成一个未闭合的标签。

```ad-error
~~~ts
const foo = <T>(x: T) => x;
~~~
```

正确做法是

```ts
const foo = <T,>(x: T) => x;
const foo = <T extends {}>(x: T): T => x;
```

[Fetching Title#8511](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

[TypeScript: Documentation - Type Compatibility](https://www.typescriptlang.org/docs/handbook/type-compatibility.html#subtype-vs-assignment)
[详解 TS 中的子类型兼容性 - 掘金](https://juejin.cn/post/7144654082777546765)

## tsconfig.json

### compilerOptions.noEmit

设置为`true`时，只进行类型检查，不进行代码转义，方便将这些工作交给 Babel，swc等工具。

### project reference

应用场景：划分模块.

需要被引用的项目下的 tsconfig.json 中配置项`compilerOptions.composite`为`true`

[Site Unreachable](https://www.typescriptlang.org/tsconfig/composite.html)
---
title: React搭配typescript
date: 2020-11-08 06:37:05
updated: 2020-11-08 06:37:05
categories:
tags:
- React
- Typescript
---

## 函数组件

### 为什么不鼓励使用 `React.FC`

许多代码库中会使用 `React.FunctionComponent` ：

```ts
const App: React.FunctionComponent<{ message: string }> = ({ message }) => (
  <div>{message}</div>
);
```

函数组件使不使用 `React.FC` 的原因在于：
- 写不写差别不大，但会引入问题
- 在React 18之前，会有潜在的`children`属性，哪怕并不需要这一属性。
- 无法使用defaultProps的默认值。

### 函数组件上的 defaultProps 将在未来停止使用 

在函数组件中，默认值更具有可读性。

[дэн 在 Twitter: "@hswolff @reactjs Because defaultProps on functions will eventually get deprecated." / Twitter](https://twitter.com/dan_abramov/status/1133878326358171650)
https://github.com/reactjs/rfcs/pull/107 
[Stop using defaultProps • Notes and anecdotes](https://notes.webutvikling.org/stop-using-defaultprops/)

## 问题记录

### `React.Element`，`React.Component`，`React.ReactInstance`， `React.ReactNode`的区别

[React Components, Elements, and Instances – React Blog](https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html)

### active , selected, open 的区别

### 传递的activeKey可能为空，如何设置默认值。



## 参考

[Function Components | React TypeScript Cheatsheets](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/function_components)

[Static Type Checking – React](https://reactjs.org/docs/static-type-checking.html#typescript)

向现有的**Create React App**创建的项目提供Typescirpt 支持
[Adding TypeScript | Create React App](https://create-react-app.dev/docs/adding-typescript/)
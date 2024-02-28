---
title: Typescript React 函数组件
date: 2021-01-03 18:36:36
updated: 2021-01-03 18:36:36
categories:
tags:
- Typescript
- React
---

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

## 参考

[Function Components | React TypeScript Cheatsheets](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/function_components)
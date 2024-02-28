---
title: 新建React项目
date: 2020-07-30 22:46:03
updated: 2020-07-30 22:46:03
categories:
  - 环境配置
tags:
  - React
---

### 不要全局安装

[`create-react-app`](https://create-react-app.dev/docs/getting-started/#quick-start)

见[[npx]]

### 根据包管理器选择

```shell
# Run this to use npm
npx create-react-app my-app
# Or run this to use yarn
yarn create react-app my-app    
```

### 选择模板

```shell
--template [template-name]sd
```

```Shell
npx create-react-app my-app --template typescript
```

默认提供两个模板
- [cra-template](https://github.com/facebook/create-react-app/tree/main/packages/cra-template)
- [cra-template-typescript](https://github.com/facebook/create-react-app/tree/main/packages/cra-template-typescript)

#### 自定义模板

[Custom Templates | Create React App](https://create-react-app.dev/docs/custom-templates/)

##### 自定义模板包命名规则

带范围的模板命名：`@[scope-name]/cra-template` 或者 `@[scope-name]/cra-template-[template-name]`

分别对应：`@[scope]` 和 `@[scope]/[template-name]`
不带范围的：[`cra-template-[template-name]`](https://www.npmjs.com/search?q=cra-template-*)

## `create-react-app`做了什么

[`create-react-app`](https://create-react-app.dev/docs/getting-started/#quick-start)

## 参考

[Getting Started | Create React App](https://create-react-app.dev/docs/getting-started/)

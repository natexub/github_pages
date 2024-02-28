---
title: Button组件
date: 2021-12-24 20:55:27
updated: 2021-12-24 20:55:27
categories:
tags:
- typescript
---

## 需求: 混合`<button/>`和`<a/>`

从功能上来说，Button按钮分成三种：
- 实心按钮（ contained ），高强调
- 边框按钮（ outlined ），中强调
- 文字按钮（ text ），弱强调

## 实现

文字按钮用`<a/>`标签进行实现，其他按钮用`<button/>`标签实现。因此遇到一个问题，除特定属性外，同时需要接受两类基础HTML属性`React.ButtonHTMLAttributes`和`React.AnchorHTMLAttributes`，传递给底层的HTML标签。

一开始我想按照语义这样使用（第36行，Markdown应该扩展一个特定行高亮特性，如同Github）：

```ts
/** 基础Props */  
interface BaseButtonProps {  
  className?: string;  
  disabled?: boolean;  
  size?: ButtonSize;  
  btnType?: ButtonType;  
  children?: React.ReactNode;  
  href?: string;  
}

type NativeButtonProps = React.ButtonHTMLAttributes<HTMLElement>  
type AnchorProps = React.AnchorHTMLAttributes<HTMLElement>
type ButtonProps = (NativeButtonProps | AnchorProps)  & BaseButtonProps

const Button = (props: ButtonProps) => {
//...
}
```

但这样行不通。

![type error](1.png)

采用了AntDesign[源码](https://github.com/ant-design/ant-design/blob/master/components/button/button.tsx#L132)中的处理方式：

将自定义属性和React type提供的原生属性进行Typescript的交叉类型(Intersection Types)并集 运算，将属性合并到一起，再做一个Partial。
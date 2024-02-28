---
title: menu
date: 2023-01-31 12:39:24
updated: 2023-01-31 12:39:24
categories:
tags:
---

## 问题

要参考的项目：
[RC-MENU](https://menu-react-component.vercel.app/?path=/story/rc-menu--readme)

1. 如何将列表转成DomNode

创建一个函数，将对象列表`map`成需要的jsx元素。并用`useMemo`对jsx元素进行包裹，来检查对象列表的依赖。

3. 如何使`<li/>`disable
4. 这是什么语法（className）
```js
//[GitHub1s](https://github1s.com/fis-components/rc-menu/blob/HEAD/lib/MenuItem.js):141
 var attrs = _extends({}, props.attribute, {
      title: props.title,
      className: (0, _classnames2['default'])(classes),
      role: 'menuitem',
      'aria-selected': props.selected,
      'aria-disabled': props.disabled
    });
```


### key 是特殊props

`<Menu/>`使用`useItems()`来将对象列表转成 Html Node. 在`useItems`中为每个`<MenuItem/>`分配了`key`. 

```jsx
const mergedKey = key ?? `tmp-${index}`  
return (  
  <MenuItem key={mergedKey}>  
    {label}  
  </MenuItem>  
)
```

在`<MenuItem/>`中，试图从props中访问这个key，触发警报：

```shell
Warning: MenuItem: `key` is not a prop. Trying to access it will result in `undefined` being returned. If you need to access the same value within the child component, you should pass it as a different prop. 
```

相关警告帮助：[Special Props Warning – React](https://reactjs.org/link/special-props)

>	有两个特殊的 props (`ref`和`key`) 被 React 使用，因此不会传递给组件。

如果想传递需要换名字，一共两个，一个用于标定唯一的key一个用于传递。

### 如何实现多级递归嵌套，同时使样式依赖上一级相应改变。

### 如何向子组件透传active等属性

[GitHub1s](https://github1s.com/react-component/menu/blob/HEAD/src/context/MenuContext.tsx#L13)

Menu 向 MenuItem 传递 onActive。MenuItem在onFocus, MouseEnter, 时触发这一onActive事件, MouseLeave触发onInActive
Menu的onActive改变activeKey。


## 需求

Item 鼠标进入时会 active，离开时会 inActive， 选中时会 selected
使用 mouseEnter 来产生 CSS 的 hover 效果。
